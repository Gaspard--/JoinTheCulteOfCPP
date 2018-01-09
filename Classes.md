# Classes

*First, God created `virtual`, God saw `virtual` was good, then God created Java and everybody stopped using C++.*

I will be using `class` and `struct` interchageably, because they are nearly the same thing.

## `class` *as a service*

### Virtual is ~~google~~ evil

To quote the bringer of the apocalipse (@El irondoge#6707) *"Virtual is used for capitalistic code."* QED if you use virtual you voted *Trump*, and you wouldn't want the *New York Times* to know that, would you?

The problem with virtual is that it looks like a solution to your problems, but often isn't one.

So if you thought classes mean `virtual`, think again.

A `class` as a service usually represents some big functionnality. It is very tempting to make everything a funtionallity.

Tha quickly leads to unoptimised OOP code, and *miss Cache* will be angry.

// insert image of angry  miss cache here

If you feel the need to use virtual, I advise to use variants instead.

virtual also doesn't handle more than one parameter polymorphisme (this) and while you can make two or more work, it quickly becomes a mess.

Compare this `std::visit`, which allows for about the same ideas, whithout the hassle of virtual inheritence and instead just plain old classes with plain old member functions, what do you think is better?

## ~~Blame~~ Responsibillity, the human intoxication

Ever noted how we like assign blame to people?

Well good news! Instead of dumping your anger on your mates you can dump it on your classes!

This is a very simple concept, however it creates a whole series of code patterns.

### RAII

The worst acronyme ever.

The concept is simple: assign a resource to a class which the class will release in it's destructor.

Example: `std::unique_ptr`, which holds an allocated pointer that it will free upon destruction, `std::fstream`, which can open a file and hold it's handle, and close it upon destruction, and many more from the standart library.

It is strongly advised to never have a ressource that isn't managed by a class.

### Ownership

Using only scopes and allocated memory as places to create and destroy objects works pretty well, but forces you to write some underoptimal code.

Welcome to the concept of ownership: a `std::unique_ptr` can abandone it's pointer using it's ```release()``` method.

But that is still a little to low-level and easy to get wrong.

#### Move assignement and construction

`std::unique_ptr` is movable, this allows you to write:

```std::unique_ptr b(std::move(a))```

`a` gives it's pointer to `b`, allowing you to freely move pointers around without having the risk of pointer not being free'd.

Adding move constructors is generally a good idea, be carefull about the fact that moving an object invalidates references to it however.

## `class` as data

When thinking about preformance, logic, sanity, and memory, this is usually what a `class` should be about.

Fundamentally in C, a struct serves to regroup data together:
 - this garanties data locality (miss Cache will *love* you)
 - allows for easier understanding of what goes together

In C++, `class` can be templated, this allows even more:
 - Create abstraction over multiple types to avoid code duplication
 - Reduce memory usage, increase performance, and reduce bug possibillities by having compile-time data.

Data can usually be owned, so when building classes with data in mind, it's usually easy to apply ressource ownership concepts.

Now for the thing where miss Cache *hates* you:

Pointers, wait you thought that was kinda bad? Function pointers!

Think about every time you use a pointer or reference:
 - When using pointer, is to spare data? Are you realy sparing a noticeable amount of data? (more often than not, no)
 - Does the data structure you're building require pointers? Could they be indexes to a contigous memory area saving memory, increassing data locality? (*Do it for miss Cache!*)
 - Is this safe at all? A pointer is the typical case of something that looks simple, acts simple, and sudenly breaks, because a pointer can become invalid. (think especially about reallocation, it's the classical \#1 catch)
 - Could this pointer be an iterator, which could both be easier to understand, and provide better context?

Think even more when using a function pointer:
 - Could this pointer be known compile-time? (you'll be suprised sometimes)
 - Consider using `std::function`
 - Can you extract this pointer / `std::function` further (__This is very important__)
 - If this pointer is set often, this usually a bad sign (miss Cache is watching *you*!)
 
This type is generally expected to be able to live a long time.
Generally speaking, pointers make more sense in data classes than references.

### `std::variant`, `std::optional` etc.

Variants can be very usefull to help organise data state.

The goal should be to avoid impossible states.

Variants do add the cost of an additional branching of course, but if you already have a branch they only provide benefits and potentially save memory.

*If your class contains an non-constexpr enum field, using variants usually will usually result in better code.*

## `class` as proxy

Sometimes you want more flexibillity with your return type (or, rarelly, parameter), so you decide to use a proxy.

If you're writting a proxy because __there is nothing suitable in the standart library__ then it's usually not a bad sign.

_Try to avoid reimplementing things._

(Special case: `std::optional`. The Rust version is better when you have a nullable value or a value with an unused state. ~~Go make some Rust~~. Make your own proxy or return the raw value if you consider it worth it. :P)

A proxy should:
 - Have a short or limited lifetime
 - Help with code clarity
 - Not be too heavy

Proxies can have different uses:
 - Access proxies, which serve to access something else (through the `operator=` member)
   - Iterators are an important subcategory here. They help simplify code a lot.
 - Return value proxies, which usually act as data holders too (typical use of std::opional), they usually are there because additional information needs to be returned.
 - Parameter proxies. Yeah. Rare case. Good luck with those. There's usually a better, superior and simpler way.

## When should I make a subclass?

This is the question about life, the universe, and how much your mum is affected by the universes expansion rate.

~~Python is the answer.~~

This is usually the moment to think about encapsulation, i.e. who has access:
 - Be carefull not to think about what your class will do but more about to what it have access.
 - Who should be able to instanciate this class?
   - Should this class be inherited and not inherited directly? -> protected destructor
   - Should this class be a singleton? -> private destructor (Not advocating for it, but you never know)
   - Is this classe an interface for runtime polymorphisme? -> virtual protected destructor (*Remember what I said and reflect on your life choices*)
 - Is the data of the subclass always used together, or am I running into the "many accessors, many parameters hell"?
 - Is this class theoretically possible? (yeah I get asked to invent classes that can't theoretically exist sometimes...)

## `struct` vs `class`

As I said earlier no big difference, unless you're making POD: struct is better due to the constructor it generates when none is specified which allows to initialize each member easilly.

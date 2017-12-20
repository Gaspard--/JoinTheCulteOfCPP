# Tutorial C++

## Prerequisits

This tutorial is intended for those that have some knowledge of C and C++.
For those already know C++ quite well the begining will try to be as fast as possible to skim over.

### Part 1: You're reading the wrong tutorial!

Obviously you should reading about js, it's a language far superior to c++ and faster, crossplateforme, blockchain, as a service than c++.
C++ is very slow, you'll end up writting underperforming code in more time and less effectivelly, while you could be using the tight and precise memory management of nodejs. C++ is the past, dart is the future. You wouldn't learn perl6 right? Come on get on with the times. A true devops knows deep down that everything can be solved with pHp and Java, so do yourself a favor and skip this tutorial.

### Part 2: So you suck to much at ruby on rails, maybe C++ will be easier?

Learning a language can be split up into 2 parts:
- how does the language work
- how should you do things

I'll be concentrating on the `how you should do things` part.
A fair word of warning: many of this may be ideologically biassed by my love of C# and swift.

## The obvious

### Programming is about tradeoff

Sometimes you may brake some rules due to tight deadlines or lack of knowledge to avoid the problem.
Losing to much time on elegance is not very productive...
That's life! You can always fixup later.

### Scopes

Use scopes to express code better. Many people seem to miss this.
Scopes have several advantages:
- variables local to the scope can't be seen outside
- grouping statements into a scope halps understanding which parts of a function belong together

### Code duplication is *BAD*

Pretty self explanatory.
Careful: sometimes something looks like code duplication, but isn't.

### Functions

Functions are really cool. You should use them more often!
Functions have the same benefits of scopes, but some bonuses:
- they isolate the other way around too!
- constnes of all passed variable refs can be changed
- help reduce code duplication because they are reusable.

However try not to end up in the land of one assembly instruction per function.
Also if 2 functions must always be called after another, that's a bad sign.

### Types

#### `auto` is your friend

`auto` is flexible, alows for easier refatoring, and uses only four chars. Don't be scared of it!
`decltype(...)` can help if knowing the type is important.
Reminder: `auto` does not deduce constness or references.

#### `const` and references

When declaring a variable, a few questions can quickly lead to cleaner code:
- Is the variable about some other memory location that will be set? Yes means non-const ref. `&`
- Otherwise, does the variable refer to some other big memory location? Yes means const ref. `const &`
- Or is the variable small? Yes means no ref no const. ``
- I don't know which of the two last (ex: template type etc.) `&&`

Mixing up the second and third case is not really bad. Note that `&&` can also mean move semantics. More on that later.

#### Do you really need a pointer?

Think about it. Would a reference do the job? (Note that a reference is like `* const` more or less...)
If using `nullptr`, would `std::optional` do a *better* job?

#### Is that the right reference?

It's not rare and hard to spot when someone takes a reference on something bigger than he needs.
It's not a major sin: just under optimal and badly conveys intent, or even makes some optimisations / refactoring non-obvious.

### End of the basics

This concludes he basics. Nothing crazy or flashy. Maybe you should be doing some Python?


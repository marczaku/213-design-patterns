# 1 Introduction

This Chapter talks about what De

## Writing a New Feature
What do you need to do, when you write a new Feature?
- understand problem
- develop pseudo-code for logic
- translate to code
- test
- publish

## Making a Change to a Feature
What do you need to do, when you make a change to an existing Feature?\
Or add one that affects other Features? (happens more often than you think)
- understand problem
- **find affected existing code**
- **understand existing code**
- develop pseudo-code for logic
- translate to code
- test
- publish

## Finding Existing Code
How easy this goes is majorly affected by
- your code structure
  - are code files sorted into folders and namespaces?
  - is it clear, whether files are sorted by type or by modules?
- your naming
  - are symbols consistently named?
  - do they have names that communicate their purpose?
- the amount of existing code
  - the less, the better
  - at some point, you might want to split up your project
    - e.g. a separate library project for your path-finding logic

But this is not today's topic.

## Understanding Existing Code
Understanding the existing code is where it'll usually get you.\
The more time this step takes you, the slower you will be at creating cool new stuff.

### Technical Debt
Technical Debt describes the debt that you create, when you write bad code instead of investing the time needed to write well-structured, well-tested and functional code.

Over the course of a project, you usually collect more and more technical debt in favor of short-term development speed.

But each bit of technical debt continuously slows down the development of further features
- finding code becomes more difficult
- understanding code becomes more difficult
- unintended side-effects happen
- more bugs get introduced

### How much (time)?
We spend a lot of time reading and understanding existing code:

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write.
― Robert C. Martin, Clean Code: A Handbook of Agile Software Craftsmanship

### How much (amount)?
The question is: How much code do we need to read and understand when we want to change a feature?

It depends on how decoupled your code is.

## Decoupling

> When we say two pieces of code are “decoupled”, we mean a change in one usually doesn't require a change in the other. When you change some feature in your game, the fewer places in code you have to touch, the easier it is.

### How much (costs)?
How much does this additional decoupling cost?

Development Costs:
- it costs some extra time to plan and introduce additional abstraction and decoupling in your code.

Complexity Costs:
- it can increase project complexity if you introduce a lot of abstraction to your code.

Runtime Costs:
- usually, it costs a (rather low) extra runtime overhead due to virtualization etc.
- optimizations are usually based on restrictions/assumptions. decoupling is the opposite of that

### How much (benefit)?
How much is the benefit of decoupling?

Development Benefits:
- when developing new features, you need to understand less existing code and can get to developing your feature sooner

Complexity Benefits:
- the complexity can appear lower than it is thanks to not having to understand HOW underlying systems work, only knowing WHAT they do

Runtime Benefits:
- They rarely exist, but Template Meta Programming (Compile-Time Polymorphism) can reduce or remove the effect

### Balance
Generally, we need to find the right balance:

If you decouple to little:
- adding new features will become increasingly difficult due to high dependencies and many side effects.
- the existing feature will become more and more rigid and hard to replace.

If you decouple too much:
- adding each feature will become increasingly difficult due to highly fragmented code pieces

To simplify the extremes: Imagine either:
- a class with one method with 700 lines of code
- a class with 700 methods with 1 line of code

Both are not ideal, what you want is maybe something like:
- three classes with 10 methods of each 25 lines of code

## Goals
> We want nice architecture so the code is easier to understand over the lifetime of the project.
> We want fast runtime performance.
> We want to get today’s features done quickly.”
- Robert Nystrom, Game Programming Patterns

## Solution
Reduce Complexity
- reduce amount of total code
- use simple and obvious intent and language
- split problems into multiple classes and functions where it makes sense

## Design Patterns
Design Patterns offer standardized, battle-proven solutions to problems commonly found in programming. They offer:
- clear conditions under which to be used
- clear implementation instructions
- sometimes: possible modifications
- a name to simplify conversations with colleagues

e.g.:
- Hey, I need to switch the Music to a more nervous one when the Player Health is Low.
- Sure, just create a new Script and use the `Observer`-Pattern to listen to updates of the Player's Health.
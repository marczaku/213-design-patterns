# Facade

What's a facade in real life?

## Summary

a facade is an object that serves as a front-facing interface masking more complex underlying or structural code.

## Motivation

> “The Principle of Least Knowledge guides us to reduce the interactions between objects to just a few close “friends.”

Head First Design Patterns, 2nd Edition (Eric Freeman)

> “There is a well-known heuristic called the Law of Demeter that says a module should not know about the innards of the objects it manipulates. As we saw in the last section, objects hide their data and expose operations.”

Clean Code: A Handbook of Agile Software Craftsmanship (Martin, Robert C.)

## Example
- Easy SFX Player in Unity

## Use Cases
- Simplify actions that you need a lot like
  - playing sound effects
  - saving or loading data
- Build a facade for a complex external library
  - similar to Adapter! (just with a different intention)

## See also
- Encapsulation
- Deep Classes
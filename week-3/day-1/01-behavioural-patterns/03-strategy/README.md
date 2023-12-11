# Strategy

## Summary

In computer programming, the strategy pattern (also known as the policy pattern) is a software design pattern that enables an algorithm’s behavior to be selected at runtime. The strategy pattern:

- defines a family of algorithms,
- encapsulates each algorithm,
- makes the algorithms interchangeable within that family.

## Motivation

Allow us to make the behavior of one part of a class interchangeable.

## State Pattern?

The `State` controls the entire object’s behavior, and it’s reasonable to think that the state will be changed at some point, either by the Context or by the States themselves.

The `Strategy` helps us to provide a custom behavior to some actions, which might or might not require being changed during the object’s lifetime. Strategies often are set from outside and not aware of each other as they don’t have a natural flow of changes like states have.

[Source](https://www.baeldung.com/cs/design-state-pattern-vs-strategy-pattern)

## Example
- Drone Movement

## Use Cases
- Different Types of Attacks
- Different Types of Movements
- 

## See also
- Encapsulation
- Open-Closed Principle

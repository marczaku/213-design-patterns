# Type Object

## Summary

Define a `type object` class and a `typed object` class. Each `type object` instance represents a different logical type. Each `typed object` stores a reference to the `type object` that describes its type.

## Motivation

Allow the flexible creation of new “classes” by creating a single class, each instance of which represents a different type of object.

## Example
- Resource Types

## Use Cases
- Anytime you want to introduce an enum named `XXXType`
- RPG: Race Type, Class Type, Stat Type, Resource Type

## See also
- Open-Closed Principle
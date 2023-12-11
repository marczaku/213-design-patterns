# Service Locator

## Summary

A service class defines an abstract interface to a set of operations. A concrete service provider implements this interface. A separate service locator provides access to the service by finding an appropriate provider while hiding both the provider’s concrete type and the process used to locate it.

## Motivation

Provide a global point of access to a service without coupling users to the concrete class that implements it.

## Example
- Skybox

## Use Cases
- Anytime you don't use Dependency-Injection

## See also
- Dependency-Injection
# SOLID Principles

From Clean Code by Robert C. Martin

## Single Responsibility

> a class should do one thing and therefore it should have only a single reason to change.

Example: `PlayerInput`, `PlayerMovement`

## Open/Closed

> classes should be open for extension and closed to modification.

Example: `PowerUp`, `Heal`, `Damage`, `SlowDown`

## Liskov Substitution

> subclasses should be substitutable for their base classes.

Example: `Rectangle`, `Square`

## Interface Segregation
> many client-specific interfaces are better than one general-purpose interface. Clients should not be forced to implement a function they do no need.

Example: `Worker`, `Human`, `Robot`

## Dependency Inversion

>  classes should depend upon interfaces or abstract classes instead of concrete classes and functions.

Example: `Health`, `HealthLabel`, `HealthBar`, `(Achievement)`
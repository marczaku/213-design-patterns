# DRY Principle

> “Giving a computer two contradictory pieces of knowledge was Captain
> James T. Kirk’s preferred way of disabling a marauding artificial
> intelligence. Unfortunately, the same principle can be effective in
> bringing down your code.”

The Pragmatic Programmer by Dave Thomas, Andy Hunt


## Stay DRY

`DRY - Don't repeat yourself`

> “The alternative is to have the same thing expressed in two or more
> places. If you change one, you have to remember to change the
> others, or, like the alien computers, your program will be brought to
> its knees by a contradiction. It isn’t a question of whether you’ll
> remember: it’s a question of when you’ll forget.”

The Pragmatic Programmer by Dave Thomas, Andy Hunt

## Types of Repetition

### Duplication in Code
- classes, functions, fields
- there's exceptions (e.g. validate age and quantity)

### Duplication in Documentation
- comments that repeat code

### Duplication in Data
- line points and length 
- exception: performance optimization

### Duplication in APIs
- both provider and consumer need to know API

### Inter-Developer Duplication
- happens when people don't know someone else solved a problem
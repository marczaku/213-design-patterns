# 2 Iterator Pattern

The Iterator Pattern is a Design Patterns used in Programming to allow us to sequentially visit all items in any type of collection.

## The Problem

We will learn about many different kinds of Collections. They all store items in different ways. Many of which don't have a clear order or a clear beginning and an end.

But then, how can we define a standardized way for looking at all items in a collection?

## The Solution

```cs
public interface ICollection {
    // Returns a class used for iterating over all elements
    IIterator GetIterator();
}
```

```cs
public interface IIterator {
    // tells you, whether there is any more elements
    bool HasNext();
    // returns the next element
    object GetNext();
}
```

Now, this Interface can be used as follows:

```cs
ICollection anyCollection = CreateRandomCollection();
IIterator iterator = anyCollection.GetIterator();
while(iterator.HasNext()){
    Console.WriteLine($"Next Element: {iterator.GetNext()}");
}
```

## IEnumerable

The `IEnumerable`-Interface is defined twice. Once as `System.Collections.IEnumerable` and once as `System.Collections.Generic.IEnumerable<T>`. I guess you know, why? Because the latter is Type-Safe and used for Generic Collections.

The Purpose is for a class to implement the Iterator-Pattern, which is a very popular Design Pattern. A Design Pattern is a Pattern that has been specified as a solution to a certain kind of problem. Usually, Design Patterns require you to implement them for your specific use-case, but the code-structure usually looks very similar to the original.

The Iterator-Pattern is a smart solution which allows you to easily iterate over a collection of variable elements.

To iterate means to go through it and look at all elements which are stored in the Collection.

You need to know, that not all collections have an Order to it. Which means, that not all Collections allow you to use the indexer like `collection[0]` and `collection[25]` to access the elements at a certain position in the collection.

Iterators solve the problem, because you get access to all elements without any guaranteed order.

The Interface is defined as such:

```cs
namespace System.Collections;

public interface IEnumerable
{
    // Returns an IEnumerator for this enumerable Object.  The enumerator provides
    // a simple way to access all the contents of a collection.
    IEnumerator GetEnumerator();
}
```

Alright. This does not look all too interesting. All the Magic happens in the `IEnumerator`-Interface, I suppose?

## IEnumerator

```cs
namespace System.Collections;

public interface IEnumerator
{
    // Advances the enumerator to the next element of the enumeration and
    // returns a boolean indicating whether an element is available. Upon
    // creation, an enumerator is conceptually positioned before the first
    // element of the enumeration, and the first call to MoveNext
    // brings the first element of the enumeration into view.
    //
    bool MoveNext();

    // Returns the current element of the enumeration. The returned value is
    // undefined before the first call to MoveNext and following a
    // call to MoveNext that returned false. Multiple calls to
    // GetCurrent with no intervening calls to MoveNext
    // will return the same object.
    //
    object? Current
    {
        get;
    }

    // Resets the enumerator to the beginning of the enumeration, starting over.
    // The preferred behavior for Reset is to return the exact same enumeration.
    // This means if you modify the underlying collection then call Reset, your
    // IEnumerator will be invalid, just as it would have been if you had called
    // MoveNext or Current.
    //
    void Reset();
}
```

Alright, the Interface looks straightforward enough, doesn't it?

`bool MoveNext()` moves the Iterator to the next Element in the Collection. In the beginning, it is placed BEFORE the First Element. Which means, that you need to move it first before trying to access any element. It returns `true`, if it successfully moved the Iterator. If it returns `false`, it means that you've reached over the end of the Collection.

`Current {get;}` gives you the current Element of the Collection. It points at different Elements after calling `MoveNext`.

`void Reset()` allows you to reset the Enumerator, in case you want to start over from the first element again. This is pretty uncommon.

## foreach

If you have done the previous Exercise, then you have actually implemented your own Implementation of `foreach`.

`foreach` is only syntactic sugar which makes Code especially readable, if you want to iterate over a collection. Instead of calling `GetEnumerator()`, `MoveNext()` and `Current`, you can simply write:

```cs
using System.Collections.Generic;

List<int> numbers = new List<int>();
numbers.Add(2);
numbers.Add(5);
foreach(int number in numbers){
    System.Console.WriteLine(number);
}
```

Pretty neat, or? Now, you've come to understand that this keyword does not do any Magic, but just uses existing Program Code under the hood. And that you are able to provide `foreach`-Support for your class by implementing the `IEnumerable` or `IEnumerable<T>`-Interface.

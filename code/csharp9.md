# What's new in C# 9

[Record types](#record-types)  
[Top level statement](#top-level-statement)  
[Init only setter](#init-only-setter)  
[Object instantiation](#object-instantiation)  
[Relational and logical pattern matching](#relational-and-logical-pattern-matching)  
[Is not in pattern matching](#is-not-in-pattern-matching)  

## Record types
Record is a reference type, which acts like value type.
Record is immutable (the values in a record cannot be changed).
There are ways, how to change values inside a record, it is possible, but is is sort of violating the design of records.

## Top level statement

Consider following program
```csharp
using System;
namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```
In C# 9 the code can be replaced with
```csharp
using System;

Console.WriteLine("Hello World!");
```

## Init only setter

```csharp
class PersonModel
{
    public int Id {get ; init; }
    public string Name {get; set; }
}
```
Init only property can be only be assigned on an object initializer or on `this` or `base` in on instance constructor or on `init` ancessor.

Object initializer:
```csharp
var person = new PersonModel() { Id = 1, Name = "Tomas"};
```
Constructor:

```csharp
 PersonModel(int id)
 {
     Id = id;
 }
```

## Object instantiation

In stead of following code
```csharp
PersonModel personModel1 =  new PersonModel(); 
PersonModel personModel2 =  new PersonModel(10, "Tomas");
PersonModel personModel3 = new PersonModel() { Id = 1, Name = "Tomas"};
```

We can write

```csharp
PersonModel personModel1 =  new (); 
PersonModel personModel2 =  new (10, "Tomas");
PersonModel personModel3 = new () { Id = 1, Name = "Tomas"};
```

## Relational and logical pattern matching

Relation pattern matching are `>`, `<`, `>=` and `<=`.
Logical pattern matching are `and`, `or` and `not`.

```csharp
var message = myValue switch
{
    <= 0 => "Less than or equal to 0",
    > 0 and <= 10 => "More than 0 but less than or equal to 10",
    _ => "More than 10"
};
Console.WriteLine(message);
```

## Is not in pattern matching
```csharp
if (person is not null)
{
    // Do something.
}
```
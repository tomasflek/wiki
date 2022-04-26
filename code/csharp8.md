# What's new in C# 8
## Pattern matching
### Ussage of `{}`
Following code can be simplified by using  `{}`:
```csharp
if (!(person?.MiddleName is null person))
{
    // Code ...
}
``` 
Can be simplified to:
```csharp
if (person?.MiddleName is { } person)
{
    // Code ...
}
```
The `null` check for `FirstName` property can be embedded  in pattern matching:
```csharp
if (person is {FirstName.Length: var length})
{
    return length;
}
return 0;
```
### Switch statement (previeous versions of C#)

``` csharp
Fruit fruit = new Apple(Color.Green);
switch (fruit)
{
	// Pattern matching with when condition.
	case Apple apple when apple.Color == Color.Green:
		Console.WriteLine($"Green Apple");
		break;
	case Apple apple:
		Console.WriteLine($"Apple with color {apple.Color}");
		break;
	case null:
		Console.WriteLine($"Is null");
		break;
}
```

### Switch expression
```csharp
Fruit fruit = new Apple(Color.Green);
string text = fruit switch
{
	Apple => "This is a apple",
	Orange => "This is a orange",
	_ => "Not a apple nor a orange",
};
```

`_` stands for whatever type, even `null`.

It can be also used as a result of a`property`:

``` csharp
public string WhatAmI => this switch
{
	Apple => "I am an apple",
	Orange => "I am an orange",
	_ => "I am not an apple nor an orange",
};
```

### Switch expression with property patterns (C# 8)

```csharp
string result = fruit switch
{
	Apple { Size: 10} apple when apple.Color == Color.Green => "Green apple of size 10",
	Apple { Size: 11 } apple when apple.Color == Color.Green => "Green apple of size 11",
	Apple => "Just a apple",
	_ => "everything else"
};
```

For more infomation about property, positional and tuple patterns see [Do more with patterns in C# 8.0](https://devblogs.microsoft.com/dotnet/do-more-with-patterns-in-c-8-0/)

## Suppress *Dereference of a possibly null reference warning*
If a compiler detects possible null reference most of the time the warning is valid. ![Null reference](pics/nullReferenceWarning.png)

The warning can be suppressed in a code by adding `!`.

```csharp
var length = person.MiddleName!.Length;
```

## Attribute for nullability

An custom method which is checking whether a string is null or empty  returns a `bool` value.

```csharp
private static int GetLastNameLength(Person person)	
{
	if (IsNullOrEmpty(person.MiddleName))
    	return 0;
    return person.MiddleName.Length;
}

private static bool IsNullOrEmpty(string? personMiddleName)
{
    if (personMiddleName == string.Empty)
		return true;
	if (personMiddleName is null)
		return true;
	return false;
}
```

The code below causes warning about possible null reference.

![Null reference 2](pics/nullReferenceWarning.png)

In order to fix the warning `NotNullWhenFalse` attribute must be used.

``` csharp
private static bool IsNullOrEmpty([NotNullWhen(false)]string? personMiddleName)
```

.NET `string.IsNullOrEmpty` implementation is written in the same way:

```csharp
public static bool IsNullOrEmpty([NotNullWhen(false)] string? value)
```

## IAsyncEnumerable interface

``` csharp
static async Task Main(string[] args)
{
	await foreach (var entry in GetEntries())
	{
		Console.WriteLine(entry);
	}
}

private static async IAsyncEnumerable<string> GetEntries()
{
	for (int i = 0; i < 1000; i++)
	{
		if (i % 10 == 0)
			await Task.Delay(2500);
		yield return i.ToString();
	}
}
```
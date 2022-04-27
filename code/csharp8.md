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
The `null` check for `FirstName` property can be embedded  in pattern matching, also `FirstName.Length` can be stored into new variable:
```csharp
if (person?.FirstName is { Length: var length })
{
    return length;
}
return 0;
```

Pass nultiple property values into new variables in pattern matching:
```csharp
if (pperson is { FirstName: var firstName, LastName: var lastName})
    Console.WriteLine($"{firstName} {lastName}");
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
## Indices and Ranges (C# 8)
```csharp
string[] words =
{
			    // Index from start     indexf from end
	"The",      // 0                    ^9
    "quick",    // 1                    ^8
    "brown",    // 2                    ^7
    "fox",      // 3                    ^6
    "jumped",   // 4                    ^5
    "over",     // 5                    ^4  
    "the",      // 6                    ^3
    "lazy",     // 7                    ^2
    "dog"       // 8                    ^1
};

// Result is "The quick brown fox" - index number 4 is exluded.
Console.WriteLine(string.Join(' ', words[0..4]));

// Result is "lazy dog"
Console.WriteLine(string.Join(' ', words[^2..^0]));

// Write all words.
Console.WriteLine(string.Join(' ', words[..]));

// Write all words up to index 3 (index 4 is not includede).
Console.WriteLine(string.Join(' ', words[..4]));

// Write all words starting from index 6 - "the lazy dog".
Console.WriteLine(string.Join(' ', words[6..]));

// ^x = words.Length - x
var x = 5;
Console.WriteLine(string.Join(' ', words[^x]));
Console.WriteLine(string.Join(' ', words[words.Length - x]));

// Range object ussage.
Range range = 3..5;
Console.WriteLine(string.Join(' ', words[range]));
```

## Using statement (C# 8)
```csharp
HttpWebRequest req = ...;

using (var resp = req.GetResponse())
using (var stream = resp.GetResponseStream())
using (var reader = new StreamReader(stream))
{
    TextBox1.Text = reader.ReadToEnd(); // or whatever
}
```

## Static local functions
The ussage is simmilar as in C# 7. The difference is that a function is marked as static, which cause that the function cannot use variables from outer scope.

Non static implementation:
```csharp
string text = "text";

string ToUpperCase()
{
    return text.ToUpper();
}
```

Static implementation:
```csharp
string text = "text";

static string ToUpperCase(string text)
{
    return text.ToUpper();
}
```

## Null coalescing assignment (C# 8)

In previous C# code we had to write following code:
```csharp
var names =  null;
if (names is null)
	names = new List<string>();
```
In C# 8 we can simplify the code:
```csharp
List<string> names = null;
names ??= new List<string>();
```
# FuncSharp, extensions and other methods ...

[FuncSharp](#funcsharp)  
[Match](#match)  
[Map](#map)  
[GetOrDefault](#getordefault)  
[Extensions](#extensions)  

## FuncSharp

### `Match`

Guru [card](https://app.getguru.com/card/cxReMd9i/Match) for `Match`.

#### `Match` ussage for `int`

``` csharp
int b = 10;
var isTen = b.Match
(
    10,
    _ => "IsTen",
    _ => "InNotTen"
);
```

#### `Match` ussage for `IOption<int>`

``` csharp
IOption<int> customOption = Option.Create(20);
bool getOption = customOption.Match(t => t > 10, _ => false);
```

or

``` csharp
IOption<string> customOptionString = Option.Create("test string");
bool getOptionString = customOptionString.Match(t => t.Equals("sdf"), _ => false);
```

#### `Match` ussage for `IOtion<int>` - call a function based on option value

``` csharp
int? test = 10;
int matchingValue = 10;
            
var optionTwenty = test.ToOption();
optionTwenty.Match(v => v.Match(matchingValue, t => Console.WriteLine($"Is {matchingValue}"), t => Console.WriteLine($"Is NOT {matchingValue}")), v => Console.WriteLine("Is NULL"));
```

#### `Match` ussage for a `class` whith derives from `Coproduct`

``` csharp
Job job = new Job(new FinishedJob());
    var test = job.Match(
        inProgress => "In progress",
        finishedJob => "finished",
        pendingJob => "pending job" 
    );

// Classes
private class Job : Coproduct3<InProgressJob, FinishedJob, PendingJob>
{
    public Job(InProgressJob firstValue) : base(firstValue) { }
    public Job(FinishedJob secondValue) : base(secondValue) { }
    public Job(PendingJob thirdValue) : base(thirdValue) { }
}

private class InProgressJob { }
private class PendingJob { }
private class FinishedJob { }
```
#### `Match` ussage for `IOption<PendingJob>`

``` csharp
IOption<PendingJob> pendingJobObption = Option.Create(new PendingJob());
string test2 = pendingJobObption.Match(_ => "pending", _ => "null");
```

### `Map`

[Nice explanation](https://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)

``` csharp
IOption<int> fiveOption = 5.ToOption();
IOption<int> resultOption = fiveOption.Map(p => p + 10);

resultOption.Match(
    hasValue => Console.WriteLine(hasValue),
    _ => Console.WriteLine("No value")
    );
```

Sum two `IOptions<int>`:

``` csharp
IOption<int> fiveOption = Option.Create(5);
IOption<int> tenOption = Option.Create(10);

IOption<IOption<int>> resultOption = fiveOption.Map(five => tenOption.Map(ten => ten + five));
IOption<int> resultFlatten = resultOption.Flatten();
resultFlatten.Match(hasValue => Console.WriteLine(hasValue), _ => Console.WriteLine("No value"));
```

or `Flatten`ussage can be replaced by following
``` csharp
IOption<int> resultOption = fiveOption.FlatMap(five => tenOption.Map(ten => ten + five));
```

### `GetOrDefault`

Sum two `IOptions<int>`:

``` csharp
IOption<int> fiveOption = Option.Create(5);
IOption<int> tenOption = Option.Create(10);

int result = fiveOption.GetOrDefault() + fiveOption.GetOrDefault();
```

## Extensions

### `MapRef`

#### `MapRef` for null check

Used if a variable can be `null`.

``` csharp
// StorageData can be null
public string StorageData { get; set; }

public string MaskedStorageData => StorageData?.MapRef(d => d.MaskToken());
```
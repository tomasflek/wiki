# FuncSharp, extensions and other methods ...

## FuncSharp

### `Match` ussage for `int`

``` csharp
int b = 10;
var isTen = b.Match
(
    10,
    _ => "IsTen",
    _ => "InNotTen"
);
```

### `Match` ussage for `IOption<int>`

``` csharp
IOption<int> customOption = Option.Create(20);
bool getOption = customOption.Match(t => t > 10, _ => false);
```

or

``` csharp
IOption<string> customOptionString = Option.Create("test string");
bool getOptionString = customOptionString.Match(t => t.Equals("sdf"), _ => false);
```

### `Match` ussage for `IOtion<int?>` - call a function based on option value

``` csharp
int? test = 10;
int matchingValue = 10;
            
var optionTwenty = test.ToOption();
optionTwenty.Match(v => v.Match(matchingValue, t => Console.WriteLine($"Is {matchingValue}"), t => Console.WriteLine($"Is NOT {matchingValue}")), v => Console.WriteLine("Is NULL"));
```

### `Match` ussage for a `class` whith derives from `Coproduct`

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
### `Match` ussage for `IOption<PendingJob>`

``` csharp
IOption<PendingJob> pendingJobObption = Option.Create(new PendingJob());
string test2 = pendingJobObption.Match(_ => "pending", _ => "null");
```

## Extensions

### `MapRef` for null check

Used if a variable can be null.

``` csharp
// StorageData can be null
public string StorageData { get; set; }

public string MaskedStorageData => StorageData?.MapRef(d => d.MaskToken());
```
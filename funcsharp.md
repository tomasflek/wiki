# FuncSharpt and other methods ...

## `Map` ussage for `int`

``` csharp
int b = 10;
var isTen = b.Match
(
    10,
    _ => "IsTen",
    _ => "InNotTen"
);
```

## `Map` ussage for `IOption<int>`

``` csharp
IOption<int> customOption = Option.Create(20);
bool getOption = customOption.Match(t => t > 10, _ => false);
```

or

``` csharp
IOption<string> customOptionString = Option.Create("test string");
bool getOptionString = customOptionString.Match(t => t.Equals("sdf"), _ => false);
```

## `Map` ussage for a `class` whith derives from `Coproduct`

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
## `Map` ussage for `IOption<PendingJob>`

``` csharp
IOption<PendingJob> pendingJobObption = Option.Create(new PendingJob());
string test2 = pendingJobObption.Match(_ => "pending", _ => "null");
```

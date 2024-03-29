# Singleton

[Link](https://csharpindepth.com/Articles/Singleton) to different singleton implementation (thread safe, etc ...)

# Behavior Design patterns

## Strategy pattern

Usage:
``` csharp
Calculator calculator = new Calculator(new OparationAdd());
Console.WriteLine(calculator.Execute(10, 15.4));
```
Strategy pattern code functionality:
``` csharp
public class Calculator
{
    private ICalculeBehaviour calculateBehaviour;

    public Calculator(ICalculeBehaviour calculateBehaviour)
    {
        this.calculateBehaviour = calculateBehaviour;
    }
    
    public double Execute(double i, double j)
    {
        return calculateBehaviour.Calculate(i, j);
    }
}

public interface ICalculeBehaviour
{
    double Calculate(double i, double j);
}

public class OparationAdd : ICalculeBehaviour
{
    public double Calculate(double i, double j)
    {
        return i + j;
    }
}

public class OperationSub : ICalculeBehaviour
{
    public double Calculate(double i, double j)
    {
        return i - j;
    }
}

public class OperationDiv : ICalculeBehaviour
{
    public double Calculate(double i, double j)
    {
        return i / j;
    }
}

public class OperationMult : ICalculeBehaviour
{
    public double Calculate(double i, double j)
    {
        return i * j;
    }
}
```
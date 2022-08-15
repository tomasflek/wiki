# F#

## File, namespaces and modules

There can be several modules on one file and one namespaces.

Namespaces are grouping of the code that are grouped together by a name. Namespaces contain types and modules.  
Module is grouping of code elements (usually implemented as a static file).  
Modules can be also nested.

``` fsharp
namespace Calculator

module Adder =
    let add x y = x + y

module Multiplier =
    let mult x y = x * y
    let square x = x * x
```
From different `.fs` file we can access those methods

``` fsharp
module Program
open Calculator

[<EntryPoint>]
let main args =
    printfn "Arguments passed to function : %A" (Adder.add 5 5)
    0
```

## Functions
In F# functions are values. They can be passed around.

``` fsharp
// int -> int -> int - function that expects int as a parameter and returns int
let add x y = x + y
```

**Lambda expression**  
The same function as above, only written by lambda

``` fsharp
// int -> int -> int
let add = fun x y -> x + y
```
**Currying/Baking-In**

You can give a function parameter and it creates a new function then takes that parameter.

In F# all functions are curried.

``` fsharp
// int -> int -> int
let add x = fun y -> x + y
```
**Nesting**  
Let bindings can be nested. The last expression in the function is the return value.
```fsharp
// int -> int
let add5 x =
    let five = 5
    x + five
```

**Define parameter types**
``` fsharp
// int -> int -> int
let add (x : int) (y : int) : int = x + y
```

**Prefix and Infix operation notation**  
Function 
``` csharp
add3 num = num + 3
```
can be written like this
``` csharp
// 3 + number
add3 num = (+) num 3
```
or
can be written like this
``` csharp
// 3 + number
add3 = (+) 3
```

**Pipe operator**
Following function
```fsharp
let calculation' number = pow2 (times2 (add3 number))
```
can be written like
``` fsharp
let calculation number =
    let add = add3 number
    let times = times2 add
    pow2 times
```
or even better with pipes

``` fsharp
let calculation number =
    number
    |> add3
    |> times2
    |> pow2
```


**Composition operator**
```fsharp
let operation'''' =
    add3
    >> times2
    >> pow2 

```

## Define new operator

``` fsharp
let operation (>>) f g =

```

### Access control

By default modules are public.

### Main function

Main function must be annotated by `EntryPoint` attribute.

``` fsharp
module Program =

    [<EntryPoint>]
    let main args =
        0
```
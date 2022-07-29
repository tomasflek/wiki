# F#

## Namespaces and modules

There can be several modules on one file and one namespaces.

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

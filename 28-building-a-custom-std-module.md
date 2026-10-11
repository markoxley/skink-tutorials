# Tutorial 28: Building a custom std module

This tutorial explains how to add your own module so the Skink compiler can find it.

## How Skink finds modules

When you write `import "mylib"`, Skink looks for `mylib.skink` in these places, in order:

1. The directory of the file being compiled.
2. `SKINK_HOME`, which is the root of the Skink distribution.
3. `SKINK_HOME/std/`.

So `import "std/time"` resolves to `std/time.skink` under `SKINK_HOME`.

## Adding a personal module

Create `mylib.skink` next to your main program:

```skink
// mylib.skink
module mylib

pub fn Greet(name: string) -> string {
    return "Hello, " + name
}
```

Then use it:

```skink run=skip
// main.skink
module main

import "mylib"

fn main() -> int {
    print(mylib.Greet("Skink"))
    return 0
}
```

## Adding a module to `std/`

If you want the module to be part of the standard library:

1. Create `std/mymodule.skink` in the Skink source tree.
2. Use `module mymodule` at the top of the file.
3. Export the public API with `pub fn` and `pub struct`.
4. Rebuild the `skink` binary if the compiler caches `std` modules.

## Module names and file paths

The module name inside the file does not have to match the file name, but matching makes imports predictable. Use lowercase names to avoid issues with case-sensitive file systems.

## Exercises

1. Create a `stringsutil.skink` module with `Reverse` and `IsPalindrome` functions.
2. Import `stringsutil` from another file and call both functions.
3. Add your module to `std/` and run `make test` to make sure nothing else breaks.

## Next

[Capstone project](29-capstone-project.md)

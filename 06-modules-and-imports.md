# Tutorial 6: Modules and imports

This tutorial explains how to split code across files and import it.

## Modules

Every Skink source file belongs to a module. The module name is declared at the top:

```skink
module math_utils
```

A file in module `math_utils` can define functions. If a function is marked `pub`, other modules can use it.

```skink
// math_utils.skink
module math_utils

pub fn double(x: int) -> int {
    return x * 2
}
```

## Importing a module

Another file imports the module by its file path. If `math_utils.skink` is in the same directory, import it like this:

```skink run=skip
// main.skink
module main

import "math_utils"

fn main() -> int {
    result := math_utils.double(7)
    print(result)
    return 0
}
```

## Standard library imports

The standard library is organised under `std/`. Import the `std/time` module when you need time utilities:

```skink
import "std/time"

fn main() -> int {
    time.SleepMs(500)
    return 0
}
```

## The `pub` keyword

Only `pub` functions, structs, and methods are visible outside the module they are defined in. Keep internal helpers private by leaving off `pub`.

## Compile order

When you compile, pass the entry point file. The compiler follows imports automatically:

```bash
SKINK_HOME=/path/to/skink skink -o myprogram main.skink
```

## Exercises

1. Move the `Rectangle` code from the previous tutorial into its own module `shapes`.
2. Import `shapes` from `main.skink` and call `area`.
3. Add a private helper function to `shapes` and try calling it from `main.skink` to confirm it is not accessible.

## Next

[Error handling](07-error-handling.md)

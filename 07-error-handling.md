# Tutorial 7: Error handling

This tutorial explains how to represent and propagate errors in Skink.

## The error type

Functions that can fail return an `errors.Error` value alongside their normal result:

```skink
module main

import "std/errors"

fn divide(a: float, b: float) -> (float, errors.Error) {
    if b == 0.0 {
        return 0.0, errors.New("division by zero")
    }
    return a / b, errors.None()
}
```

`errors.None()` means no error; `err != nil` still works for the check.

## Checking errors

Call the function and check the second return value:

```skink
import "std/errors"

fn divide(a: float, b: float) -> (float, errors.Error) {
    if b == 0.0 {
        return 0.0, errors.New("division by zero")
    }
    return a / b, errors.None()
}

fn main() -> int {
    result, err := divide(10.0, 2.0)
    if err != nil {
        print("Failed: ", err.Error())
        return 1
    }
    print("Result: ", result)
    return 0
}
```

## The `?` operator

The `?` operator propagates an error automatically. If the function returns an error, the current function returns early with that error.

```skink
import "std/errors"

fn divide(a: float, b: float) -> (float, errors.Error) {
    if b == 0.0 {
        return 0.0, errors.New("division by zero")
    }
    return a / b, errors.None()
}

fn compute() -> (float, errors.Error) {
    x := divide(10.0, 2.0)?
    y := divide(x, 0.0)?    // division by zero, so compute returns the error
    return x + y, errors.None()
}

fn main() -> int {
    result, err := compute()
    if err != nil {
        print("Compute failed: ", err.Error())
        return 1
    }
    print("Result: ", result)
    return 0
}
```

`?` is a shorthand for the check-and-return pattern. It only works in functions that return an `errors.Error`.

## Exercises

1. Write `sqrt(x: float) -> (float, errors.Error)` that rejects negative numbers.
2. Use `?` in a `calculate` function that calls `sqrt` and then `divide`.
3. Print a friendly message when an error occurs.

## Next

[Concurrency](08-concurrency.md)

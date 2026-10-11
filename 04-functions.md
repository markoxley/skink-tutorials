# Tutorial 4: Functions

This tutorial covers defining functions, parameters, return values, and calling functions.

## Defining a function

Use `fn`, a name, parameters, and an optional return type:

```skink
fn greet(name: string) {
    print("Hello, ", name, "!")
}

fn main() -> int {
    greet("Skink")
    return 0
}
```

## Returning values

Add a return type after the parameter list:

```skink
fn add(a: int, b: int) -> int {
    return a + b
}
```

## Multiple return values

Functions can return more than one value:

```skink
fn divide(a: float, b: float) -> (float, bool) {
    if b == 0.0 {
        return 0.0, false
    }
    return a / b, true
}

fn main() -> int {
    result, ok := divide(10.0, 2.0)
    if ok {
        print("Result: ", result)
    } else {
        print("Cannot divide by zero")
    }
    return 0
}
```

## Calling functions

Call a function by name and provide values for each parameter:

```skink
fn square(n: int) -> int {
    return n * n
}

fn main() -> int {
    x := square(7)
    print("7 squared is ", x)
    return 0
}
```

## Local variables

Variables declared inside a function are local to that function. They shadow outer variables only within their own scope.

## Exercises

1. Write a function `max(a: int, b: int) -> int` that returns the larger value.
2. Write a function `swap(a: int, b: int) -> (int, int)` that returns `b, a`.
3. Combine two functions: `area` computes `width * height`, and `perimeter` computes `2 * (width + height)`. Call both from `main`.

## Next

[Structs and methods](05-structs-and-methods.md)

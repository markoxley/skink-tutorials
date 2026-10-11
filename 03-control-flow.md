# Tutorial 3: Control flow

This tutorial shows how to make decisions and repeat code in Skink.

## If statements

```skink
fn main() -> int {
    temp := 22.0
    if temp > 30.0 {
        print("hot")
    } else if temp > 15.0 {
        print("comfortable")
    } else {
        print("cold")
    }
    return 0
}
```

## While loops

Use `while` when you do not know the number of iterations ahead of time:

```skink
fn main() -> int {
    n := 5
    total := 0
    while n > 0 {
        total = total + n
        n = n - 1
    }
    print("Sum: ", total)
    return 0
}
```

Note: the assignment operator is `=`, not `:=`.

## For loops

Use `for` when you have an init, condition, and update expression:

```skink
fn factorial(n: int) -> int {
    result := 1
    for i := 1; i <= n; i = i + 1 {
        result = result * i
    }
    return result
}
```

## For-in loops

Iterate over an array with `for ... in`:

```skink
fn main() -> int {
    temps := [20.0, 22.5, 19.0]
    sum := 0.0
    for t in temps {
        sum = sum + t
    }
    print("Average: ", sum / 3.0)
    return 0
}
```

## Exercises

1. Write an `if` chain that classifies an `int` as negative, zero, or positive.
2. Use a `while` loop to print the powers of 2 up to `1024`.
3. Write a `for` loop that counts from `10` down to `1`.

## Next

[Functions](04-functions.md)

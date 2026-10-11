# Tutorial 2: Variables and types

This tutorial covers the basic data you can store and manipulate in Skink.

## Declaring variables

You can declare a variable and let the compiler infer its type:

```skink
module main

fn main() -> int {
    x := 10
    y := 3.14
    name := "Skink"
    print(x, " ", y, " ", name)
    return 0
}
```

Or you can write the type explicitly with `var`:

```skink
var count: int = 10
var price: float = 3.14
var label: string = "item"
```

## Common types

| Type     | Example    | Description           |
| -------- | ---------- | --------------------- |
| `int`    | `42`       | Signed integer        |
| `float`  | `3.14`     | Floating-point number |
| `bool`   | `true`     | `true` or `false`     |
| `string` | `"hello"`  | Text                  |
| `byte`   | `byte(65)` | A single byte         |

## Constants

Use `const` for values that never change:

```skink
const MAX_RETRIES = 5
```

## Arrays

Create an array with square brackets:

```skink
scores := [85, 92, 78]
first := scores[0]
```

Array indices start at `0`.

## Type casting

Convert between numeric types with a constructor-style cast:

```skink
value := 3.7
rounded := int(value)  // 3
```

## A small program

```skink
fn main() -> int {
    width := 10
    height := 4
    area := width * height
    print("Area: ", area)
    return 0
}
```

## Exercises

1. Store your age in a variable and print a sentence that includes it.
2. Create an array of three floating-point temperatures and print the first one.
3. Cast `float` to `int` and verify the result.

## Next

[Control flow](03-control-flow.md)

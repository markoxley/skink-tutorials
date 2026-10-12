# Tutorial 1: Hello, world

In this tutorial you will write, compile, and run your first Skink program.

## Your first program

Create a file called `hello.skink`:

```skink
fn main() -> int {
    print("Hello, world!")
    return 0
}
```

`main` is the entry point of every Skink program. It returns an `int` status code: `0` means success.

## Compile and run

```bash
SKINK_HOME=/path/to/skink skink -o hello hello.skink
./hello
```

You should see:

```
Hello, world!
```

## Using a variable

Variables are declared with `:=`. The type is inferred from the value.

```skink
fn main() -> int {
    name := "Skinker"
    print("Hello, ", name, "!")
    return 0
}
```

Skink's `print` and `println` builtins accept multiple arguments and print them in order, so you can pass string literals and variables as separate arguments.

## Exercises

1. Change the program to print your own name.
2. Declare a second variable `greeting` and use it in the printed message.
3. What happens if `main` returns `1` instead of `0`?

## Next

[Variables and types](02-variables-and-types.md)

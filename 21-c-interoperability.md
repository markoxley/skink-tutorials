# Tutorial 21: C interoperability

This tutorial explains how to call C functions from Skink.

## Declaring extern functions

Use `extern fn` to tell Skink about a C function:

```skink
module main

extern fn getpid() -> int

fn main() -> int {
    pid := getpid()
    print("Current process ID: ", pid)
    return 0
}
```

The function must be available at link time. The C standard library is usually available by default.

## Importing C headers

If you have a C header file, you can import it directly:

```skink
import "C:mylib.h"
```

This exposes the declarations in `mylib.h` to Skink. You will need to link the corresponding object file or shared library when compiling.

## Using libc helpers

`std/libc` wraps many common C functions so you do not have to redeclare them:

```skink
import "std/libc"
import "std/str"

fn main() -> int {
    text := "hello"
    n := libc.strlen(text)
    print("Length: ", n)
    return 0
}
```

## Wrapping C in Skink functions

A typical pattern is to expose a safe Skink interface over a raw C call:

```skink
fn currentProcessId() -> int {
    return getpid()
}
```

## Exercises

1. Call `getuid()` and print the current user ID.
2. Use `std/libc` to copy a string with `strncpy`.
3. Declare and call a small C function in a separate `math.c` file and link it with `skink -o`.

## Congratulations

You have now covered the practical core of Skink, from first programs to concurrency, databases, web servers, and C interop. For deep dives, see `skink-advanced/`, the standard library `std/`, and `docs/language_manual.md`.

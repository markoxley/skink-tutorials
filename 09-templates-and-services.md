# Tutorial 9: Templates and services

This tutorial explains how to define and use interface-like contracts in Skink.

## Templates (duck typing)

A `template` describes a set of methods. Any type that has those methods can be used where the template is expected. No `implements` keyword is needed.

```skink
template Printer {
    fn Print(self: Printer, message: string)
}

struct Console {
    pub fn Print(self: Console, message: string) {
        print(message)
    }
}

fn sayHello(p: Printer) {
    p.Print("Hello from a template")
}

fn main() -> int {
    c := Console{}
    sayHello(c)
    return 0
}
```

The compiler checks at the call site that `Console` has a matching `Print` method.

## Services (named interfaces)

A `service` is a named contract. Unlike templates, the type must explicitly be considered an implementation of that service. They are useful when you want an explicit type to carry through the program.

```skink
service Writer {
    fn Write(self: Writer, data: []byte) -> int
}
```

Services are still under active development in Skink. For now, prefer `template` for most day-to-day use.

## When to use each

- Use `template` when you want flexible, duck-typed interfaces.
- Use `service` when you need a named interface for documentation or tooling.

## Exercises

1. Define a `Named` template with a `Name(self: Named) -> string` method and implement it for a `Person` struct.
2. Write a function `greet(n: Named)` that prints `"Hello, {n.Name()}!"`.
3. Read `std/rules.skink` and identify the `RuleSource` template methods.

## Next

[Reading and writing files](10-reading-and-writing-files.md)

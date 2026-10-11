# Tutorial 5: Structs and methods

This tutorial shows how to group related data into structs and attach methods to them.

## Defining a struct

```skink
struct Point {
    x: int
    y: int
}
```

Fields are listed by name and type. Each field goes on its own line.

## Creating instances

```skink
struct Point {
    x: int
    y: int
}

fn main() -> int {
    p := Point{x: 3, y: 4}
    print("p = (", p.x, ", ", p.y, ")")
    return 0
}
```

## Methods

Methods are functions with a special first parameter, usually called `self`:

```skink
struct Rectangle {
    width: int
    height: int

    pub fn area(self: Rectangle) -> int {
        return self.width * self.height
    }

    pub fn perimeter(self: Rectangle) -> int {
        return 2 * (self.width + self.height)
    }
}

fn main() -> int {
    r := Rectangle{width: 4, height: 5}
    print("Area: ", r.area())
    print("Perimeter: ", r.perimeter())
    return 0
}
```

Mark methods `pub` if you want them to be usable from other modules.

## Pointer receivers

If a method needs to modify the struct, use a pointer receiver:

```skink
struct Counter {
    value: int

    pub fn increment(self: *Counter) {
        self.value = self.value + 1
    }
}

fn main() -> int {
    c := Counter{value: 0}
    c.increment()
    print("Count: ", c.value)
    return 0
}
```

## Exercises

1. Add a `scale` method to `Rectangle` that multiplies `width` and `height` by a factor.
2. Create a `Circle` struct with `radius` and `area` methods.
3. Implement a `BankAccount` struct with `deposit` and `balance` methods.

## Next

[Modules and imports](06-modules-and-imports.md)

# Tutorial 11: Strings and text

This tutorial covers common string operations in Skink using `std/str`.

## Length and equality

```skink
import "std/str"

fn main() -> int {
    s := "hello"
    print("Length: ", str.Len(s))
    if str.Equal(s, "hello") {
        print("equal")
    }
    return 0
}
```

## Searching

```skink
if str.Contains("hello world", "world") {
    print("found")
}

idx := str.IndexOf("hello world", "world")
print("Index: ", idx)
```

## Substrings and case

```skink
sub := str.Substr("hello world", 0, 5)
upper := str.ToUpper(sub)
print(upper)
```

## Splitting and joining

```skink
parts := str.Split("one,two,three", ",")
joined := str.Join(parts, " | ")
print(joined)
```

## Building strings efficiently

Use `Builder` when concatenating in a loop to avoid many allocations:

```skink
b := str.NewBuilder(64)
b.Write("first")
b.Write(" ")
b.Write("second")
print(b.String())
```

## Exercises

1. Count the number of words in a sentence by splitting on spaces and printing the length.
2. Convert a comma-separated string to a slice, then join the same values with colons.
3. Use `str.Trim` to remove leading and trailing whitespace from a string.

## Next

[JSON](12-json.md)

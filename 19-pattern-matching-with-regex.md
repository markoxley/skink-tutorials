# Tutorial 19: Pattern matching with regex

This tutorial covers `std/regex`. It supports literal patterns and POSIX ERE quantifiers (`*` and `?`).

## Compiling a pattern

```skink
module main

import "std/regex"

fn main() -> int {
    re, _ := regex.Compile("h?llo")
    if re.Match("hello") {
        print("matched hello")
    }
    return 0
}
```

In this pattern `?` is an ERE quantifier that makes the preceding `h` optional, so `h?llo` matches both "hllo" and "llo".

## Matching full strings

`Match` returns `true` only if the whole string matches the pattern from start to finish:

```skink
re, _ := regex.Compile("cat")
if re.Match("cat") {      // true
    print("exact match")
}
if re.Match("catalog") {  // false
    print("this won't run")
}
```

## Finding a substring

`Find` returns the first match inside a larger string:

```skink
re, _ := regex.Compile("cat")
m := re.Find("concatenate cats")
if m.Start >= 0 {
    print("found '", m.Text, "' at ", m.Start)
}
```

## Quantifiers

- `*` is an ERE quantifier matching zero or more of the preceding element.
- `?` is an ERE quantifier matching zero or one of the preceding element.

```skink
re, _ := regex.Compile("s*n")
if re.Match("shin") {     // s* matches zero or more 's', then 'n'
    print("matched")
}
```

## Replacing matches

```skink
re, _ := regex.Compile("cat")
result := re.ReplaceAll("one cat two cats", "dog")
print(result) // "one dog two dogs"
```

## Exercises

1. Compile a pattern that matches any 3-letter word starting with `c`.
2. Use `Find` to locate the first number in a string.
3. Write a small `grep` program that prints each line of a file matching a pattern.

## Next

[Reading and writing CSV](20-reading-and-writing-csv.md)

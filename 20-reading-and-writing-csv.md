# Tutorial 20: Reading and writing CSV

This tutorial shows how to use `std/csv` to parse and generate CSV text.

## Reading CSV

```skink
module main

import "std/csv"

fn main() -> int {
    data := "name,age\nAlice,30\nBob,25\n"
    reader := csv.NewReader(data)

    for {
        rec, err := reader.Read()
        if err.message != "" {
            break
        }
        name, _ := rec.Get(0)
        age, _ := rec.Get(1)
        print(name, " is ", age)
    }
    return 0
}
```

`Read` returns a `Record` and an error. When there is no more data, the error message is non-empty.

## Working with fields

A `Record` exposes its length and individual values:

```skink
reader := csv.NewReader("name,age\nAlice,30\n")
rec, _ := reader.Read()
print("columns: ", rec.Len())
```

## Writing CSV

Use a `csv.NewWriter()` and call `Write(rec)` to build output. The `Record` is typically produced by `reader.Read()`. You can also create one if the `fields` field is accessible in your context.

```skink
import "std/csv"
import "std/str"

fn main() -> int {
    w := csv.NewWriter()
    rec, _ := csv.NewReader("header1,header2").Read()
    w.Write(rec)
    print(w.String())
    return 0
}
```

## Exercises

1. Parse a CSV string and print the average of a numeric column.
2. Read a CSV file with `std/fs` and `csv.NewReader`.
3. Write a program that converts a CSV file to a JSON object using `std/json`.

## Next

[C interoperability](21-c-interoperability.md)

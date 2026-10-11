# Tutorial 12: JSON

This tutorial shows how to parse and build JSON values with `std/json`.

## Parsing JSON

`std/json` exposes a dynamic `Value` type that can hold any JSON value.

```skink
module main

import "std/json"
import "std/str"

fn main() -> int {
    raw := "{\"name\":\"skink\",\"version\":1.5}"
    v, err := json.Unmarshal(raw)
    if err.message != "" {
        print("parse failed")
        return 1
    }

    name := json.AsString(json.ObjectGet(v, "name"))
    version := json.AsNumber(json.ObjectGet(v, "version"))
    print(name, " v", version)
    return 0
}
```

## Building JSON

Use the constructor functions to create values:

```skink
v := json.FromObject(
    ["name", "version"],
    [json.FromString("skink"), json.FromNumber(1.5)]
)
text := json.MarshalValue(v)
print(text)
```

## Arrays

```skink
arr := json.FromArray([json.FromNumber(1.0), json.FromNumber(2.0)])
print(json.MarshalValue(arr))
```

## Exercises

1. Parse a JSON array and print each number using `json.ArrayGet` and `json.AsNumber`.
2. Build a JSON object representing a user with `name` and `age` fields.
3. Read a JSON file with `std/fs` and `std/json.Unmarshal`, then print a specific field.

## Next

[Unit testing](13-unit-testing.md)

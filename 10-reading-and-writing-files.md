# Tutorial 10: Reading and writing files

This tutorial introduces file operations with the `std/fs` module.

## The fs module

`std/fs` provides helpers for reading and writing files without manually managing C file handles.

```skink
module main

import "std/fs"

fn main() -> int {
    err := fs.WriteAll("config.txt", "name=skink\n")
    if err.message != "" {
        print("Could not write file")
        return 1
    }

    text, rerr := fs.ReadAll("config.txt")
    if rerr.message != "" {
        print("Could not read file")
        return 1
    }
    print(text)
    return 0
}
```

`WriteAll` replaces existing contents. `ReadAll` returns the file as a string along with an error.

## Checking existence

```skink
if fs.Exists("config.txt") {
    print("file exists")
} else {
    print("file does not exist")
}
```

## Appending data

```skink
err := fs.AppendAll("log.txt", "another line\n")
if err.message != "" {
    print("append failed")
    return 1
}
```

## Removing and renaming

```skink
fs.Rename("old.txt", "new.txt")
fs.Remove("temp.txt")
```

## Exercises

1. Write a program that creates a file, reads it back, and prints its length using `std/str.Len`.
2. Append a timestamp from `std/time` to a log file each run.
3. Create a small program that only overwrites `data.txt` if it does not already exist.

## Next

[Strings and text](11-strings-and-text.md)

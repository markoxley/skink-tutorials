# Tutorial 16: Building a small project

This tutorial walks through a tiny but complete Skink program: a command-line note manager that stores notes in a JSON file.

## Project outline

- `notes/` — the project directory
- `notes/storage.skink` — read/write notes from `notes.json`
- `notes/main.skink` — parse a subcommand and call storage functions

## The data model

A note has a title and a body:

```skink
// storage.skink
module storage

import "std/json"
import "std/fs"
import "std/str"

pub struct Note {
    title: string
    body: string
}

pub fn loadNotes() -> ([]Note, bool) {
    if !fs.Exists("notes.json") {
        var empty []Note
        return empty, true
    }
    _, err := fs.ReadAll("notes.json")
    if err.message != "" {
        var empty []Note
        return empty, false
    }
    // In a real project, parse the JSON text here.
    var empty []Note
    return empty, true
}
```

For brevity this tutorial shows the design. A complete implementation would convert `[]Note` into `json.Value` and back.

## The main program

```skink run=skip
// main.skink
module main

import "std/str"
import "storage"

fn main(args: []string) -> int {
    if len(args) < 2 {
        print("usage: notes <list|add>")
        return 1
    }
    cmd := args[1]
    if str.Equal(cmd, "list") {
        notes, ok := storage.loadNotes()
        if !ok {
            print("failed to load notes")
            return 1
        }
        for n in notes {
            print(n.title, ": ", n.body)
        }
    } else if str.Equal(cmd, "add") {
        print("add not implemented yet")
    } else {
        print("unknown command: ", cmd)
        return 1
    }
    return 0
}
```

## Building and running

```bash
SKINK_HOME=/path/to/skink-lang skink -o notes main.skink storage.skink
./notes list
```

## What to add next

- Implement `storage.saveNotes` using `json.MarshalValue` and `fs.WriteAll`.
- Support `add "title" "body"` by appending to the list and saving.
- Add a `delete` command.
- Write `storage_test.skink` to verify `loadNotes` and `saveNotes`.

## Congratulations

You have now seen the core parts of Skink and built a small project end to end. For more advanced examples, see the `skink-advanced/` directory and `docs/language_manual.md`.

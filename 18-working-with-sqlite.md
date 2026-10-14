# Tutorial 18: Working with SQLite

This tutorial shows how to use `std/db` to talk to a SQLite database.

## Opening a connection

```skink
module main

import "std/db"

fn main() -> int {
    conn, err := db.Open("sqlite3", "todos.db")
    if err.message != "" {
        print("open failed: ", err.message)
        return 1
    }

    _, rerr := conn.Exec("CREATE TABLE IF NOT EXISTS todos (id INTEGER PRIMARY KEY, text TEXT)")
    if rerr.message != "" {
        print("create failed: ", rerr.message)
        conn.Close()
        return 1
    }

    conn.Close()
    return 0
}
```

Note: `std/db` uses `errors.Error`; an empty `message` means success.

## Inserting data

Use `Exec` for `INSERT`, `UPDATE`, and `DELETE` statements:

```skink
conn, oerr := db.Open("sqlite3", "todos.db")
if oerr.message != "" {
    return 1
}
_, rerr := conn.Exec("CREATE TABLE IF NOT EXISTS todos (id INTEGER PRIMARY KEY, text TEXT)")

title := "Learn Skink"
sql := "INSERT INTO todos (text) VALUES (" + db.QuoteString(title) + ")"
_, err := conn.Exec(sql)
if err.message != "" {
    print("insert failed: ", err.message)
    return 1
}
```

`db.QuoteString` wraps a string in single quotes and escapes internal quotes.

## Querying data

Use `Query` and iterate with `Next` and `Scan`:

```skink
conn, oerr := db.Open("sqlite3", "todos.db")
if oerr.message != "" {
    return 1
}

result, err := conn.Query("SELECT id, text FROM todos")
if err.message != "" {
    print("query failed: ", err.message)
    return 1
}

for result.Next() {
    id, _ := result.Scan(0)
    text, _ := result.Scan(1)
    print(id, ": ", text)
}
```

## Closing

Call `conn.Close()` when you are done to release the SQLite handle.

## The easier path: `std/orm`

For the common case — "rows as structs" — `std/orm` writes the SQL for
you. Field attributes declare the mapping on a plain struct, then a
`Table` exposes `Create`/`Insert`/`Get`/`Save`/`Delete`/`All`/`Where`:

```skink
module main

import "std/orm"
import "std/reflect"

pub struct Todo {
    [db(pk,auto)]
    id: int64
    text: string
    done: bool
}

fn main() -> int {
    conn, _ := orm.Open("todos.db")
    var t Todo
    todos, terr := orm.TableOf(conn, reflect.TypeOf<Todo>(t))
    if terr.message != "" {
        print("table bind failed: ", terr.message)
        return 1
    }
    _ = todos.Create()

    t.text = "learn the orm"
    _ = todos.Insert(int64(&t))   // t.id is filled in automatically

    rows := todos.All()
    var row Todo
    for rows.Next(int64(&row)) {
        print(row.id, ": ", row.text)
    }
    rows.Close()
    conn.Close()
    return 0
}
```

Notes:

- `orm.TableOf` needs the field layout explicitly — generics erase
  type parameters, so you pass `reflect.TypeOf<Todo>(value)` plus
  `int64(&obj)` for the struct pointer.
- `[db(pk,auto)]` marks the auto-increment primary key; an untagged
  `id` field is the implicit pk. `[column(name=x)]` renames a column;
  `[db(ignore)]` excludes a field.
- `Where("done = ?", []string{"0"})` filters with bound parameters;
  `Save`, `Delete`, and `Drop` cover the rest of the lifecycle.
- Reach for raw `db` when you need joins, transactions, or non-integer
  primary keys — the orm covers the 80% case.

## Exercises

1. Create a `todos` table with a `done` integer column.
2. Write a function `addTodo(conn: *db.Connection, text: string)`.
3. Write a function `listTodos(conn: *db.Connection)` that prints every row.

## Next

[Pattern matching with regex](19-pattern-matching-with-regex.md)

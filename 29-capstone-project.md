# Tutorial 29: Capstone project — a tiny monitoring dashboard

This tutorial brings together many of the topics you have learned. You will design a small system that collects sensor data, stores it in SQLite, and exposes it over HTTP.

## System overview

- A `SensorSource` ruleset (from Tutorial 14) watches a `temperature` variable and a dynamic MQTT or synthetic source.
- When a rule fires, the current reading is saved to a SQLite `readings` table.
- A web server (from Tutorial 17) provides `/` and `/api/readings` endpoints.
- `/` renders an HTML template (Tutorial 25).
- `/api/readings` returns the latest readings as JSON.

## Project layout

```
monitor/
  main.skink       // entry point: start ruleset and web server
  sensor.skink     // SensorSource and RuleSource implementation
  storage.skink    // SQLite helpers
  handlers.skink   // HTTP routes
  dashboard.tmpl   // HTML template
```

## Key pieces

### Storage

```skink
// storage.skink
module storage

import "std/db"
import "std/errors"

pub fn initSchema(conn: *db.Connection) -> errors.Error {
    _, err := conn.Exec("CREATE TABLE IF NOT EXISTS readings (id INTEGER PRIMARY KEY, value REAL, ts INTEGER)")
    return err
}
```

### Recording data

```skink
import "std/db"
import "std/errors"
import "std/fmt"

pub fn recordReading(conn: *db.Connection, value: float, ts: int64) -> errors.Error {
    sql := "INSERT INTO readings (value, ts) VALUES (" + fmt.Ftoa(value) + ", " + fmt.Itoa(int(ts)) + ")"
    _, err := conn.Exec(sql)
    return err
}
```

In a real implementation, use `db.QuoteString` or prepared statements to avoid SQL injection.

### Web handlers

```skink
// handlers.skink
module handlers

import "std/web/router"
import "std/web/types"

pub fn index(req: types.Request, w: *types.ResponseWriter) {
    w.HTML(200, "<h1>Dashboard</h1>")
}
```

### Main flow

```skink run=skip
// main.skink
module main

import "std/db"
import "std/web/router"
import "std/web/server"
import "handlers"
import "storage"

fn main() -> int {
    conn, err := db.Open("sqlite3", "readings.db")
    if err.message != "" {
        print("db open failed")
        return 1
    }
    storage.initSchema(&conn)

    // Start the ruleset and web server.
    // (SensorSource and ruleset setup omitted for brevity.)

    r := router.NewRouter()
    r.Get("/", handlers.index)

    s := server.NewServer(8080, &r)
    serr := s.ListenAndServe()
    if serr.message != "" {
        return 1
    }
    return 0
}
```

## What to add

1. A `SensorSource` that updates `temperature` and triggers `storage.recordReading`.
2. An `/api/readings` route that queries the database and returns JSON.
3. A `dashboard.tmpl` with a simple table and a chart drawn with plain JavaScript.
4. Tests for `storage` and `handlers` using `skink test`.

## Congratulations

You have now completed the Skink tutorial series. You can write programs, tests, web servers, databases, networking clients, and numerical code. Use `docs/language_manual.md` and the `skink-advanced/` examples as reference for deeper exploration.

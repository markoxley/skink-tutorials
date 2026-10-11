# Tutorial 17: Building a web server

This tutorial introduces Skink's `std/web` packages by building a tiny HTTP server.

## Router and handlers

A `Handler` is a function that receives a `Request` and writes to a `*ResponseWriter`:

```skink
module main

import "std/web/router"
import "std/web/server"
import "std/web/types"

fn home(req: types.Request, w: *types.ResponseWriter) {
    w.Write("Hello, Skink!")
}

fn greet(req: types.Request, w: *types.ResponseWriter) {
    name := req.URLParam("name")
    w.Write("Hello, " + name + "!")
}

fn jsonDemo(req: types.Request, w: *types.ResponseWriter) {
    w.JSON(200, "{\"ok\":true}")
}
```

## Wiring up routes

Create a `Router`, register paths, and start the server:

```skink run=compile
fn main() -> int {
    r := router.NewRouter()
    r.Get("/", home)
    r.Get("/hello/:name", greet)
    r.Get("/api/status", jsonDemo)

    s := server.NewServer(8080, &r)
    err := s.ListenAndServe()
    if err.message != "" {
        print("server error: ", err.message)
        return 1
    }
    return 0
}
```

## Accessing request data

`Request` gives you the method, path, query, headers, and body:

```skink
fn search(req: types.Request, w: *types.ResponseWriter) {
    q := req.QueryParam("q")
    w.Write("You searched for: " + q)
}
```

## Middleware (advanced)

A middleware wraps a handler. `router.Use` registers one for all routes:

```skink
fn logging(next: types.Handler) -> types.Handler {
    return fn(req: types.Request, w: *types.ResponseWriter) {
        print(req.Method(), " ", req.Path())
        next(req, w)
    }
}
```

## Exercises

1. Add a `POST /echo` handler that reads `req.Body()` and writes it back.
2. Create a route `/user/:id` that returns JSON like `{"id":"123"}`.
3. Run the server and test it with `curl http://localhost:8080/hello/world`.

## Next

[Working with SQLite](18-working-with-sqlite.md)

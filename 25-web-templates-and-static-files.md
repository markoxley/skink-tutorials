# Tutorial 25: Web templates and static files

This tutorial expands on the web server by adding HTML templates and static file serving.

## HTML templates

`std/web/tmpl` provides a small Go-style template engine.

```skink
module main

import "std/web/tmpl"
import "std/web/types"

fn home(req: types.Request, w: *types.ResponseWriter) {
    t, err1 := tmpl.Parse("<h1>Hello, {{.name}}!</h1>")
    if err1.message != "" {
        w.WriteHeader(500)
        w.Write("bad template")
        return
    }

    data := make(map[string]string)
    data["name"] = "Skink"
    rendered, err2 := tmpl.Execute(t, data)
    if err2.message != "" {
        w.WriteHeader(500)
        w.Write("render error")
        return
    }

    w.HTML(200, rendered)
}
```

Templates use `{{.Key}}` for variable substitution and are HTML-escaped for safety.

## Static files

`std/web/static` serves files from a directory:

```skink run=compile
import "std/web/router"
import "std/web/server"
import "std/web/static"
import "std/web/types"

// home is the handler defined in the templates section above.
fn home(req: types.Request, w: *types.ResponseWriter) {
    w.Write("home")
}

fn main() -> int {
    r := router.NewRouter()
    r.Get("/", home)
    r.Get("/assets/*", static.FileServer("./public"))

    s := server.NewServer(8080, &r)
    serr := s.ListenAndServe()
    if serr.message != "" {
        return 1
    }
    return 0
}
```

`static.FileServer` returns a `types.Handler` that maps URL paths to files under the given root. It rejects paths containing `..`.

## Putting it together

1. Create `public/style.css`.
2. Reference it in the template: `<link rel="stylesheet" href="/assets/style.css">`.
3. Run the server and load the page.

## Exercises

1. Render a list of items using `{{range .items}}`.
2. Add a conditional `{{if .admin}}` block.
3. Serve a favicon from `public/favicon.ico`.

## Next

[Cryptography basics](26-cryptography-basics.md)

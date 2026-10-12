# Tutorial 22: Async and await

This tutorial introduces `async` and `await`, a higher-level concurrency pattern built on top of `spawn` and channels.

## Running a task asynchronously

Use `async` to start a function call in the background. It returns a *future* that you can `await` later.

```skink
fn compute(id: int) -> int {
    print("Task ", id, " starting...")
    return id * 10
}

fn main() -> int {
    f1 := async compute(1)
    f2 := async compute(2)

    print("Main thread is not blocked")

    res1 := await f1
    res2 := await f2

    print("Task 1 returned: ", res1)
    print("Task 2 returned: ", res2)
    return 0
}
```

`async` starts the work immediately, but `await` blocks until the result is ready.

## Returning multiple values

An `async` function must return a **single** value — the compiler rejects
multi-return functions used with `async`. To pass back several results,
bundle them in a struct or map:

```skink
import "std/fmt"

struct FetchResult {
    data: string
    ok:   bool
}

fn fetch(id: int) -> FetchResult {
    return FetchResult{data: "data-" + fmt.Itoa(id), ok: true}
}

fn main() -> int {
    f := async fetch(1)
    res := await f
    if res.ok {
        print(res.data)
    }
    return 0
}
```

## When to use async/await vs spawn

- Use `async`/`await` when you need a result back.
- Use `spawn` when the task is fire-and-forget or communicates through channels.

## Compilation note

Programs that use `async`/`await` must link with `compiler/runtime/conc_rt.c`:

```bash
SKINK_HOME=/path/to/skink skink -o async_demo main.skink
```

The Skink runtime links the concurrency runtime by default when needed.

## Exercises

1. Run two `async` tasks concurrently and sum the results.
2. Write an `async` task that returns `(int, error)` and handle the error.
3. Compare the `async`/`await` version with a `spawn`/`chan` version of the same program.

## Next

[Networking with TCP](23-networking-with-tcp.md)

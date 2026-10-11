# Tutorial 8: Concurrency

This tutorial introduces Skink's lightweight concurrency primitives: channels and `spawn`.

## Channels

A channel is a typed pipe for sending values between concurrent tasks. An
unbuffered send blocks until another task receives, so this first example
uses a buffered channel with room for one value:

```skink
fn main() -> int {
    ch := make(chan<int>, 1)
    ch <- 42          // send 42 on the channel
    val := <-ch       // receive a value from the channel
    print("Received: ", val)
    return 0
}
```

The arrow points in the direction the value travels. `ch <- 42` sends; `<-ch` receives.

## Running tasks with spawn

`spawn` starts a new concurrent task. It is useful for work that would block or that can run in parallel.

```skink
fn worker(ch: chan<int>) {
    ch <- 99
}

fn main() -> int {
    ch := make(chan<int>)
    spawn worker(ch)
    val := <-ch
    print("Got: ", val)
    return 0
}
```

`spawn` can also run a zero-argument function directly:

```skink
fn sayHello() {
    print("hello from a task")
}

fn main() -> int {
    spawn sayHello()
    // Wait a moment so the task has time to run before the program exits.
    // In real programs, use a channel to wait for completion.
    return 0
}
```

## Synchronising with channels

Use channels to wait for a task to finish:

```skink
fn produce(out: chan<int>) {
    out <- 1
    out <- 2
    out <- 3
}

fn main() -> int {
    ch := make(chan<int>)
    spawn produce(ch)

    for i := 0; i < 3; i = i + 1 {
        val := <-ch
        print(val)
    }
    return 0
}
```

## Shared memory and mutexes

For shared mutable state, use `std/sync`:

```skink
import "std/sync"

var counter: int = 0

fn incrementer(mu: *sync.Mutex) {
    mu.Lock()
    counter = counter + 1
    mu.Unlock()
}
```

Skink prefers channels for most communication, but mutexes are available when you need them.

## Exercises

1. Spawn two tasks that each send one value on the same channel and receive both in `main`.
2. Write a `fib(n: int, out: chan<int>)` function that sends the first `n` Fibonacci numbers.
3. Use a channel to signal that a spawned worker has completed.

## Congratulations

You have completed the introductory Skink tutorials. You now know how to write, compile, and run Skink programs. For more advanced topics, see the `skink-advanced/` examples and `docs/language_manual.md`.

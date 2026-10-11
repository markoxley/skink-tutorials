# Tutorial 15: Mutexes and waitgroups

This tutorial covers the two main ways to coordinate concurrent work in Skink.

## Mutexes

`std/sync` provides `Mutex` for exclusive access to shared data:

```skink
module main

import "std/sync"
import "std/time"

var counter: int = 0

fn worker(mu: *sync.Mutex, id: int) {
    for i := 0; i < 1000; i = i + 1 {
        mu.Lock()
        counter = counter + 1
        mu.Unlock()
    }
}

fn main() -> int {
    mu := sync.NewMutex()
    spawn worker(mu, 1)
    spawn worker(mu, 2)
    time.SleepMs(100)
    print("counter: ", counter)
    return 0
}
```

Always unlock the mutex, even on error paths, or your program will deadlock.

## WaitGroups

A `WaitGroup` lets you block until a set of spawned tasks finish:

```skink
import "std/waitgroup"

fn produce(wg: *waitgroup.WaitGroup, ch: chan<int>, val: int) {
    ch <- val
    wg.Done()
}

fn main() -> int {
    ch := make(chan<int>, 2)
    wg := waitgroup.New()

    wg.Add(2)
    spawn produce(wg, ch, 1)
    spawn produce(wg, ch, 2)

    for i := 0; i < 2; i = i + 1 {
        print(<-ch)
    }
    wg.Wait()
    return 0
}
```

## Read-write locks

Use `std/sync.RWMutex` when reads are common and writes are rare:

```skink
rw := sync.NewRWMutex()
rw.RLock()
// read-only access
rw.RUnlock()

rw.Lock()
// write access
rw.Unlock()
```

## Exercises

1. Spawn three workers that each add to a shared counter and use a `WaitGroup` to wait for all of them.
2. Convert a `Mutex` example to use an `RWMutex` where workers only read the counter.
3. Use a `Mutex` to protect a shared `[]int` slice that workers append to.

## Next

[Building a small project](16-building-a-small-project.md)

# Tutorial 13: Unit testing

This tutorial shows how to write and run unit tests in Skink.

## The testing module

Tests live in files ending with `_test.skink`. They import the `testing` module, which provides assertion helpers like `Errorf` and `Failed`.

```skink
// math_test.skink
module math_test

import "std/testing"

pub fn TestAddition() {
    if 2 + 2 != 4 {
        Errorf("addition failed")
    }
}
```

## Running tests

Use the `skink test` command:

```bash
SKINK_HOME=/path/to/skink skink test math_test.skink
```

Skink finds every public function whose name starts with `Test`, runs it, and reports pass or fail for each.

## A complete test example

```skink
module calc_test

import "std/testing"
import "std/fmt"

pub fn add(a: int, b: int) -> int {
    return a + b
}

pub fn TestAdd() {
    result := add(2, 3)
    if result != 5 {
        Errorf("expected 5, got " + fmt.Itoa(result))
    }
}

pub fn TestAddNegative() {
    result := add(-1, -1)
    if result != -2 {
        Errorf("expected -2, got " + fmt.Itoa(result))
    }
}
```

## Resetting state between tests

If your tests share global variables, call `Reset()` at the start of each test:

```skink
import "std/testing"

pub fn TestState() {
    testing.Reset()
    // ... test code
}
```

## Exercises

1. Write a test for the `max` function you created in the functions tutorial.
2. Write a failing test and observe the `skink test` output.
3. Add `Reset()` to a test that uses a global counter.

## Next

[Rulesets and dynamic rules](14-rulesets-and-dynamic-rules.md)

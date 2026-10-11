# Tutorial 14: Rulesets and dynamic rules

This tutorial explains Skink's `ruleset` keyword and dynamic `RuleSource` template.

## Static rules

A `ruleset` declares one or more `rule` blocks with a `when` condition and an `action` block:

```skink
module main

import "std/time"

var temp: float = 20.0

ruleset ClimateRules {
    rule hot when temp > 30.0 {
        action: {
            print("hot")
        }
        priority: 1
    }

    rule cold when temp < 10.0 {
        action: {
            print("cold")
        }
        priority: 2
    }
}

fn main() -> int {
    ClimateRules.start()
    temp = 35.0
    // give the background loop time to evaluate
    time.SleepMs(50)
    ClimateRules.stop()
    return 0
}
```

`priority` is optional. Lower numbers evaluate first.

## Dynamic sources

Dynamic sources implement the `RuleSource` template from `std/rules`:

```skink
import "std/sync"
import "std/time"

struct SensorSource {
    mu: *sync.Mutex
    reading: float
    threshold: float

    pub fn NewSensorSource(threshold: float) -> SensorSource {
        return SensorSource{
            mu: sync.NewMutex(),
            reading: 0.0,
            threshold: threshold,
        }
    }

    pub fn SetReading(self: *SensorSource, r: float) {
        self.mu.Lock()
        self.reading = r
        self.mu.Unlock()
    }

    pub fn name(self: *SensorSource) -> string { return "sensor" }
    pub fn start(self: *SensorSource) {}
    pub fn stop(self: *SensorSource) {}

    pub fn triggered(self: *SensorSource) -> bool {
        self.mu.Lock()
        r := self.reading
        self.mu.Unlock()
        return r > self.threshold
    }

    pub fn Action(self: *SensorSource) {
        print("sensor triggered")
    }

    pub fn Priority(self: *SensorSource) -> int {
        return 5
    }
}
```

Note that `Action` and `Priority` are capitalised because `action` and `priority` are ruleset keywords.

Register and start the source:

```skink run=compile
fn main() -> int {
    src := SensorSource.NewSensorSource(50.0)
    ClimateRules.registerRule(&src)
    ClimateRules.start()

    src.SetReading(60.0)
    time.SleepMs(50)

    ClimateRules.stop()
    return 0
}
```

## Lifecycle

- `ClimateRules.start()` — begin evaluation
- `ClimateRules.stop()` — stop the background loop
- `ClimateRules.restart()` — stop then start
- `ClimateRules.reset()` — clear all registered rules

## Exercises

1. Add a `normal` rule that fires when `temp` is between 10.0 and 30.0.
2. Implement a second `RuleSource` and register both.
3. Try `ClimateRules.reset()` after `stop()` to clear state between runs.

## Next

[Mutexes and waitgroups](15-mutexes-and-waitgroups.md)

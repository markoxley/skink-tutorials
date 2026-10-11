# Tutorial 24: MQTT and IoT

This tutorial shows how to publish and subscribe to MQTT topics with `std/mqtt`.

## Initialising the client

```skink
module main

import "std/mqtt"

fn main() -> int {
    client, err := mqtt.NewClient("skink_client", "test.mosquitto.org", 1883)
    if err.message != "" {
        print("create failed: ", err.message)
        return 1
    }

    err = client.Connect()
    if err.message != "" {
        print("connect failed: ", err.message)
        client.Close()
        return 1
    }

    err = client.Publish("home/livingroom/temperature", "22.5", 0)
    if err.message != "" {
        print("publish failed: ", err.message)
    } else {
        print("published")
    }

    client.Disconnect()
    client.Close()
    mqtt.Cleanup()
    return 0
}
```

## Subscribing to a topic

```skink
import "std/mqtt"
import "std/time"

client, cerr := mqtt.NewClient("skink_client", "test.mosquitto.org", 1883)
if cerr.message != "" {
    return 1
}
cerr = client.Connect()

err := client.Subscribe("home/+/temperature", 0)
if err.message != "" {
    print("subscribe failed: ", err.message)
    return 1
}

// Process incoming messages for a while.
for i := 0; i < 10; i = i + 1 {
    client.Loop(100)
    time.SleepMs(100)
}
```

## Receiving messages

`std/mqtt` `Client` does not yet expose a high-level message callback, so for production use you may want to wrap the low-level C mosquitto functions in your own struct.

## Exercises

1. Publish a few sensor readings in a loop.
2. Subscribe to a topic and print the payload for each received message.
3. Combine `std/mqtt` with `std/rules` so a rule fires when a particular payload is received.

## Next

[Web templates and static files](25-web-templates-and-static-files.md)

# Tutorial 23: Networking with TCP

This tutorial covers the `std/net` module and shows how to create a TCP client and server.

## A TCP client

```skink run=compile
module main

import "std/net"
import "std/str"

fn main() -> int {
    conn, err := net.Connect("127.0.0.1", 12345)
    if err.message != "" {
        print("connect failed: ", err.message)
        return 1
    }

    sent := conn.Send("hello\n")
    if sent < 0 {
        print("send failed")
        conn.Close()
        return 1
    }

    response, rerr := conn.Recv(1024)
    if rerr.message != "" {
        print("recv failed: ", rerr.message)
        conn.Close()
        return 1
    }
    print(response)

    conn.Close()
    return 0
}
```

`net.Connect` takes an IP address and a port. `Send` and `Recv` handle raw bytes as strings.

## A TCP echo server

```skink run=compile
module main

import "std/net"

fn main() -> int {
    listener, err := net.Listen("0.0.0.0", 12345)
    if err.message != "" {
        print("listen failed: ", err.message)
        return 1
    }

    for {
        conn, aerr := net.Accept(listener)
        if aerr.message != "" {
            continue
        }
        // Handle one connection inline.
        data, rerr := conn.Recv(1024)
        if rerr.message == "" {
            conn.Send(data)
        }
        conn.Close()
    }
}
```

`net.Listen` returns a `Connection` that wraps a listening socket. `Accept` blocks until a client connects.

## HTTP helper

`std/net` also provides a simple HTTP GET helper:

```skink run=compile
import "std/net"

resp, err := net.HTTPGet("93.184.216.34", "/", 80)
if err.message == "" {
    print("status: ", resp.status)
    print(resp.body)
}
```

Use the IP address, because `HTTPGet` does not perform DNS resolution.

## Exercises

1. Start the echo server in one terminal and the client in another.
2. Modify the client to read a message from the user.
3. Use `spawn` to handle multiple connections concurrently in the server.

## Next

[MQTT and IoT](24-mqtt-and-iot.md)

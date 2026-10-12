# Skink Tutorials

This directory contains a set of targeted, hands-on tutorials for learning the Skink programming language. Each tutorial focuses on one concept, includes runnable examples, and ends with a small exercise so you can practise before moving on.

## What you need

- A built `skink` compiler binary.
- The `SKINK_HOME` environment variable pointing at the directory that contains the `std/` and `runtime/` directories (for example, the `skink/` repo root).

A typical compile-and-run command looks like:

```bash
SKINK_HOME=/path/to/skink skink -o myprogram myprogram.skink
./myprogram
```

## Suggested order

1. [Hello, world](01-hello-world.md)
2. [Variables and types](02-variables-and-types.md)
3. [Control flow](03-control-flow.md)
4. [Functions](04-functions.md)
5. [Structs and methods](05-structs-and-methods.md)
6. [Modules and imports](06-modules-and-imports.md)
7. [Error handling](07-error-handling.md)
8. [Concurrency](08-concurrency.md)
9. [Templates and services](09-templates-and-services.md)
10. [Reading and writing files](10-reading-and-writing-files.md)
11. [Strings and text](11-strings-and-text.md)
12. [JSON](12-json.md)
13. [Unit testing](13-unit-testing.md)
14. [Rulesets and dynamic rules](14-rulesets-and-dynamic-rules.md)
15. [Mutexes and waitgroups](15-mutexes-and-waitgroups.md)
16. [Building a small project](16-building-a-small-project.md)
17. [Building a web server](17-building-a-web-server.md)
18. [Working with SQLite](18-working-with-sqlite.md)
19. [Pattern matching with regex](19-pattern-matching-with-regex.md)
20. [Reading and writing CSV](20-reading-and-writing-csv.md)
21. [C interoperability](21-c-interoperability.md)
22. [Async and await](22-async-and-await.md)
23. [Networking with TCP](23-networking-with-tcp.md)
24. [MQTT and IoT](24-mqtt-and-iot.md)
25. [Web templates and static files](25-web-templates-and-static-files.md)
26. [Cryptography basics](26-cryptography-basics.md)
27. [Tensors and math](27-tensors-and-math.md)
28. [Building a custom std module](28-building-a-custom-std-module.md)
29. [Capstone project](29-capstone-project.md)

Each tutorial is self-contained. If you already know a topic, feel free to skip ahead, but check the examples because they build on each other.

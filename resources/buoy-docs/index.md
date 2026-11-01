---
title: "System information"
summary: "Read an immutable snapshot of system and process information."
category: "Overview"
order: 0
keywords: ["system", "processors", "memory", "uptime", "thermal", "power"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# System information

The `systeminfo` package snapshots OS and process names, processor counts, physical memory and uptime on macOS, Linux and Windows. Platform-specific readings (thermal state, low-power mode, available memory, swap, load averages) are reported only where the platform provides them, through `Has*` flags and `Optional` fields. It is read-only and does not collect environment variables or credentials.

## Platform support

macOS, Linux and Windows. Gate portable code on `SystemInfoSnapshotSupported()`. `SystemInfoSupported()` keeps its original macOS-only meaning and returns False on Linux and Windows even though snapshots work there.

## Ownership and lifetime

All public properties are read-only values captured at construction. Construct another `SystemInfoSnapshot` when you need a later reading. Native objects are released before construction returns; there is no live observer or Close method.

## Read next

- [Getting started](getting-started.md)
- [API reference](api.md)

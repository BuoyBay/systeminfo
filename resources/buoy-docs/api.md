---
title: "System information API"
summary: "Public symbols and contracts for the systeminfo package."
category: "Reference"
order: 20
keywords: ["system", "processors", "memory", "uptime", "thermal", "power"]
symbols: ["SystemInfoSupported", "SystemInfoSnapshotSupported", "SystemInfoSnapshot", "SystemInfoSnapshot.OperatingSystem", "SystemInfoSnapshot.ProcessName", "SystemInfoSnapshot.ProcessorCount", "SystemInfoSnapshot.ActiveProcessors", "SystemInfoSnapshot.MemoryBytes", "SystemInfoSnapshot.UptimeSeconds", "SystemInfoSnapshot.ThermalState", "SystemInfoSnapshot.LowPowerMode", "SystemInfoSnapshot.HasThermalState", "SystemInfoSnapshot.HasLowPowerMode", "SystemInfoSnapshot.AvailableMemoryBytes", "SystemInfoSnapshot.SwapTotalBytes", "SystemInfoSnapshot.SwapFreeBytes", "SystemInfoSnapshot.LoadAverage1Minute", "SystemInfoSnapshot.LoadAverage5Minutes", "SystemInfoSnapshot.LoadAverage15Minutes"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

Import `systeminfo` from the sealed package. The signatures below describe its public surface.

| Symbol | Contract |
| --- | --- |
| `SystemInfoSupported() As Boolean` | Frozen: True only for the original macOS backend. |
| `SystemInfoSnapshotSupported() As Boolean` | True on macOS, Linux and Windows targets. Compile-time; never probes or prompts. |
| `New SystemInfoSnapshot()` | Capture system/process values now. |
| `OperatingSystem As String` | Read-only native OS version description. |
| `ProcessName As String` | Read-only current process name. |
| `ProcessorCount / ActiveProcessors As Integer` | Read-only logical and currently active processor counts. |
| `MemoryBytes As Integer` | Read-only total physical memory bytes. |
| `UptimeSeconds As Double` | Read-only system uptime in seconds. |
| `ThermalState As Integer` | Native thermal state on macOS, including unfamiliar values; -1 when `HasThermalState` is False. |
| `LowPowerMode As Boolean` | Native low-power-mode state; False when `HasLowPowerMode` is False. |
| `HasThermalState As Boolean` | True on macOS; False on Linux and Windows. |
| `HasLowPowerMode As Boolean` | True on macOS 12+; False elsewhere. |
| `AvailableMemoryBytes As Optional Of Integer` | Linux `MemAvailable`, Windows `ullAvailPhys`; None on macOS or when Linux lacks `MemAvailable`. |
| `SwapTotalBytes / SwapFreeBytes As Optional Of Integer` | Linux sysinfo swap; None on macOS and Windows. |
| `LoadAverage1Minute / LoadAverage5Minutes / LoadAverage15Minutes As Optional Of Double` | Linux sysinfo load averages; None on macOS and Windows. |

## Errors and lifecycle

`MemoryBytes` is physical memory, not available application memory. Linux `AvailableMemoryBytes` comes from `/proc/meminfo` `MemAvailable` and is never replaced by sysinfo `freeram`; Windows reports `ullAvailPhys`. The Windows commit limit is not reported as swap. `UptimeSeconds` uses each platform's clock: NSProcessInfo on macOS, `CLOCK_BOOTTIME` on Linux and `GetTickCount64` on Windows (both include suspend). Thermal values follow the native API: 0 nominal, 1 fair, 2 serious, 3 critical; unfamiliar future values are preserved. Off macOS no thermal or low-power value is invented. Linux native call failures throw `RuntimeException` with the operation name, but do not preserve errno in `ErrorCode`. Windows native call failures include the Win32 code or NTSTATUS in the message and `ErrorCode`. On Linux, an unavailable or unrecognized `/proc/meminfo` `MemAvailable` field yields None for `AvailableMemoryBytes`. Byte counts above the `Integer` range throw `OverflowException`. These are observations, not guarantees about future resource availability.

All public properties are read-only values captured at construction. Construct another `SystemInfoSnapshot` when you need a later reading. Native objects are released before construction returns; there is no live observer or Close method.

## Platform support

macOS, Linux (glibc) and Windows. `SystemInfoSupported()` keeps returning False on Windows and Linux. On a target with no backend the constructor throws `UnsupportedOperationException`. `SystemInfoLinuxBackend` and `SystemInfoWindowsBackend` are internal modules, not public API.

[Getting started](getting-started.md) · [Package overview](index.md)


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)

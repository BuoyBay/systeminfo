---
title: "System information getting started"
summary: "Install systeminfo and use its native API with explicit ownership."
category: "Guides"
order: 10
keywords: ["system", "processors", "memory", "uptime", "thermal", "power"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install the sealed package in `packages/` and import `systeminfo`. Create a snapshot when building a diagnostics view or making an adaptive-work decision. The example only prints descriptive data and changes no system setting.

```buoy
Import systeminfo

Sub Main()
    If Not SystemInfoSnapshotSupported() Then Exit Sub
    Dim info As New SystemInfoSnapshot()
    Print info.OperatingSystem
    Print info.ProcessName
    Print info.MemoryBytes
    Print info.UptimeSeconds
    If info.AvailableMemoryBytes.HasValue Then Print info.AvailableMemoryBytes.Value
    If info.HasThermalState Then Print info.ThermalState
End Sub
```

## Handle native constraints

`MemoryBytes` is physical memory; `AvailableMemoryBytes` is the platform's own estimate of memory available without swapping, and is None where the platform has no such value. `UptimeSeconds` is system uptime in each platform's clock (Linux and Windows include suspend). Thermal values follow the native API (0 nominal, 1 fair, 2 serious, 3 critical) and exist only when `HasThermalState` is True; elsewhere `ThermalState` is -1. Read `LowPowerMode` only when `HasLowPowerMode` is True. These are observations, not guarantees about future resource availability.

[Package overview](index.md) · [API reference](api.md)

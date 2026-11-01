---
title: "systeminfo typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["SystemInfoLinuxSysinfo", "SystemInfoLinuxTimespec", "SystemInfoSnapshot", "SystemInfoWindowsMemoryStatus"]
dependencies: []
symbols: ["SystemInfoLinuxSysinfo", "SystemInfoLinuxTimespec", "SystemInfoSnapshot", "SystemInfoWindowsMemoryStatus"]
related: ["api.md", "getting-started.md", "index.md"]
---

# systeminfo typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## SystemInfo functions

### SystemInfoSupported

`Public Function SystemInfoSupported() As Boolean`

Frozen: True only where the original macOS NSProcessInfo backend runs. Portable code gates on SystemInfoSnapshotSupported instead.

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

### SystemInfoSnapshotSupported

`Public Function SystemInfoSnapshotSupported() As Boolean`

True when this compile target has a SystemInfoSnapshot backend (macOS, Linux and Windows). Compile-time only: it never probes, prompts or reads system state. Per-field availability is reported by the snapshot's Has* flags and Optional fields.

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

## SystemInfoSnapshot

`Public Class SystemInfoSnapshot`

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `OperatingSystem` | `String` | Yes | No | — | — |
| `ProcessName` | `String` | Yes | No | — | — |
| `ProcessorCount` | `Integer` | Yes | No | — | — |
| `ActiveProcessors` | `Integer` | Yes | No | — | — |
| `MemoryBytes` | `Integer` | Yes | No | — | — |
| `UptimeSeconds` | `Double` | Yes | No | — | — |
| `ThermalState` | `Integer` | Yes | No | — | NSProcessInfoThermalState (0 nominal … 3 critical) on macOS. -1 where HasThermalState is False; never a fabricated "nominal" reading. |
| `LowPowerMode` | `Boolean` | Yes | No | — | Meaningful only when HasLowPowerMode is True; otherwise False. |
| `HasThermalState` | `Boolean` | Yes | No | — | True on macOS; False on Linux and Windows (ThermalState = -1). |
| `HasLowPowerMode` | `Boolean` | Yes | No | — | True on macOS 12+ (isLowPowerModeEnabled); False on Linux and Windows. |
| `AvailableMemoryBytes` | `Optional Of Integer` | Yes | No | — | Memory available for new allocations without swapping: Linux /proc/meminfo MemAvailable (never sysinfo freeram), Windows MEMORYSTATUSEX.ullAvailPhys. None on macOS and when Linux lacks MemAvailable. |
| `SwapTotalBytes` | `Optional Of Integer` | Yes | No | — | Linux sysinfo totalswap. None on macOS and Windows (the Windows commit limit is not swap). |
| `SwapFreeBytes` | `Optional Of Integer` | Yes | No | — | Linux sysinfo freeswap. None on macOS and Windows. |
| `LoadAverage1Minute` | `Optional Of Double` | Yes | No | — | Linux sysinfo load averages. None on macOS and Windows. |
| `LoadAverage5Minutes` | `Optional Of Double` | Yes | No | — | — |
| `LoadAverage15Minutes` | `Optional Of Double` | Yes | No | — | — |

### Constructor

`Public Sub Constructor()`

No parameters.

Returns: no value.

## SystemInfoLinuxSysinfo

`Sequential Structure SystemInfoLinuxSysinfo`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Uptime` | `Int64` | — | — |
| `Load1` | `UInt64` | — | — |
| `Load5` | `UInt64` | — | — |
| `Load15` | `UInt64` | — | — |
| `TotalRam` | `UInt64` | — | — |
| `FreeRam` | `UInt64` | — | — |
| `SharedRam` | `UInt64` | — | — |
| `BufferRam` | `UInt64` | — | — |
| `TotalSwap` | `UInt64` | — | — |
| `FreeSwap` | `UInt64` | — | — |
| `Processes` | `UInt16` | — | — |
| `Padding` | `UInt16` | — | — |
| `TotalHigh` | `UInt64` | — | — |
| `FreeHigh` | `UInt64` | — | — |
| `MemoryUnit` | `UInt32` | — | — |

## SystemInfoLinuxTimespec

`Sequential Structure SystemInfoLinuxTimespec`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Seconds` | `Int64` | — | — |
| `Nanoseconds` | `Int64` | — | — |

## SystemInfoLinuxBackend functions

### SystemInfoLinuxReadSysinfo

`Public Function SystemInfoLinuxReadSysinfo() As SystemInfoLinuxSysinfo`

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `SystemInfoLinuxSysinfo` |

### SystemInfoLinuxOnlineProcessors

`Public Function SystemInfoLinuxOnlineProcessors() As Integer`

Online system CPUs, not the process affinity mask or a container's CPU quota.

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### SystemInfoLinuxConfiguredProcessors

`Public Function SystemInfoLinuxConfiguredProcessors() As Integer`

Configured system CPUs, including offline CPUs.

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### SystemInfoLinuxBootSeconds

`Public Function SystemInfoLinuxBootSeconds() As Double`

CLOCK_BOOTTIME: Linux uptime including suspended time.

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `Double` |

### SystemInfoLinuxKernel

`Public Function SystemInfoLinuxKernel() As String`

Kernel name and release from uname(2), e.g. "Linux 6.12.0".

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `String` |

### SystemInfoLinuxMemAvailableKiB

`Public Function SystemInfoLinuxMemAvailableKiB() As Optional Of Integer`

MemAvailable from /proc/meminfo in KiB (Linux 3.14+). None when the kernel does not report it; sysinfo freeram is never substituted.

Availability: `#If TargetLinux Then`

No parameters.

#### Returns

| Type |
| --- |
| `Optional Of Integer` |

## SystemInfoWindowsMemoryStatus

`Sequential Structure SystemInfoWindowsMemoryStatus`

### Fields

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `Size` | `UInt32` | — | — |
| `LoadPercent` | `UInt32` | — | — |
| `TotalPhysicalBytes` | `UInt64` | — | — |
| `AvailablePhysicalBytes` | `UInt64` | — | — |
| `CommitLimitBytes` | `UInt64` | — | — |
| `AvailableCommitBytes` | `UInt64` | — | — |
| `TotalVirtualBytes` | `UInt64` | — | — |
| `AvailableVirtualBytes` | `UInt64` | — | — |
| `Reserved` | `UInt64` | — | — |

## SystemInfoWindowsBackend functions

### SystemInfoWindowsReadMemory

`Public Function SystemInfoWindowsReadMemory() As SystemInfoWindowsMemoryStatus`

Availability: `#If TargetWindows Then`

No parameters.

#### Returns

| Type |
| --- |
| `SystemInfoWindowsMemoryStatus` |

### SystemInfoWindowsUptimeMilliseconds

`Public Function SystemInfoWindowsUptimeMilliseconds() As UInt64`

Milliseconds since boot, including sleep and hibernation.

Availability: `#If TargetWindows Then`

No parameters.

#### Returns

| Type |
| --- |
| `UInt64` |

### SystemInfoWindowsActiveProcessors

`Public Function SystemInfoWindowsActiveProcessors() As Integer`

ALL_PROCESSOR_GROUPS (0xFFFF): logical processors across all groups that are currently active.

Availability: `#If TargetWindows Then`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### SystemInfoWindowsConfiguredProcessors

`Public Function SystemInfoWindowsConfiguredProcessors() As Integer`

Maximum logical processors the system supports across all groups.

Availability: `#If TargetWindows Then`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### SystemInfoWindowsVersion

`Public Function SystemInfoWindowsVersion() As String`

"Windows <major>.<minor> (Build <build>)" from RtlGetVersion.

Availability: `#If TargetWindows Then`

No parameters.

#### Returns

| Type |
| --- |
| `String` |

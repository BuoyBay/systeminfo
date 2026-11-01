# System and process information · 0.1.0

`Import systeminfo`. `New SystemInfoSnapshot()` reads OS and process names, configured and active processor counts, physical memory, uptime, and, where the platform reports them, thermal state, low-power mode, available memory, swap and load averages. Properties are read-only. Nothing is changed, nothing prompts, and environment variables or credentials are not collected.

| Field | macOS | Linux | Windows |
| --- | --- | --- | --- |
| `OperatingSystem` | NSProcessInfo version string | `uname` sysname + release | `RtlGetVersion`: `Windows 10.0 (Build n)` |
| `ProcessName` | NSProcessInfo | executable file name | executable name without `.exe` |
| `ProcessorCount` / `ActiveProcessors` | configured / active | `get_nprocs_conf` / `get_nprocs` | `GetMaximumProcessorCount` / `GetActiveProcessorCount` (all groups) |
| `MemoryBytes` | physical memory | sysinfo `totalram` | `ullTotalPhys` |
| `UptimeSeconds` | `systemUptime` | `CLOCK_BOOTTIME` (includes suspend) | `GetTickCount64` (includes sleep) |
| `HasThermalState` / `ThermalState` | True / 0–3 | False / -1 | False / -1 |
| `HasLowPowerMode` / `LowPowerMode` | True on macOS 12+ | False / False | False / False |
| `AvailableMemoryBytes` | None | `MemAvailable` (never `freeram`) | `ullAvailPhys` |
| `SwapTotalBytes` / `SwapFreeBytes` | None | sysinfo swap | None (commit limit is not swap) |
| `LoadAverage1Minute` / `5Minutes` / `15Minutes` | None | sysinfo loads | None |

`SystemInfoSupported()` is frozen to its original meaning: True only on macOS. Portable code gates on `SystemInfoSnapshotSupported()`, which is True on all three operating systems and never probes the system. Byte counts that would exceed `Integer` throw `OverflowException` instead of wrapping.

Linux and Windows backends live in internal modules `SystemInfoLinuxBackend` and `SystemInfoWindowsBackend`. Their native declarations are copied from the container interop modules (`linuxsystem`, `linuxnative`, `WinSystem`); the package imports no container code.

Maintainers run the private validation suite before release. See the repository README for public build instructions.

Inspired by [macoslib’s Cocoa examples](https://github.com/macoslib/macoslib); implemented independently against [Apple’s API](https://developer.apple.com/documentation/foundation/processinfo).

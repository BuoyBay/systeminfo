# OS / process / CPU / memory / uptime snapshots

`Import systeminfo`. `New SystemInfoSnapshot()` reads OS and process names, configured and active processor counts, physical memory, uptime, and, where the platform reports them, thermal state, low-power mode, available memory, swap and load averages. Properties are read-only. Nothing is changed, nothing prompts, and environment variables or credentials are not collected.

`SystemInfoSupported()` is frozen to its original meaning: True only on macOS. Portable code gates on `SystemInfoSnapshotSupported()`, which is True on all three operating systems and never probes the system. Byte counts that would exceed `Integer` throw `OverflowException` instead of wrapping.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add systeminfo --git https://github.com/BuoyBay/systeminfo.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

```basic
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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer systeminfo
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)

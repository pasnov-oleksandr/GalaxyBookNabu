# Changelog

All notable changes to **GalaxyBookNabu** are documented in this file.

## [1.0.0] - 2026-11-21

### Added
- **Xiaomi Pad 5 (`nabu`) ARM64 Core App Patching (`STEP 5B`)**: Automated extraction and deployment of `AllInNabu.zip` and `SysIdStub.dll` for native Windows 11 ARM64 support.
- **Developer Mode Gate**: Interactive verification (`Assert-DeveloperModeBeforePackaging`) ensuring Windows Developer Mode is enabled prior to unpacking and registering local AppX manifests.
- **Galaxy Connect ARM64 Patch (`Ecma335BoolPatcher`)**: Runtime CIL patching of `HasSupportedBluetooth` and PE checksum recalculation across Continuity assemblies.
- **Samsung Account ARM64 Patch (`AutoNabuPatcher`)**: Redirects `Windows.System.Profile.SystemIdentification` to `SysIdStub.dll` inside `SAClient.dll` and registers the in-process server in `AppxManifest.xml`.
- **Quick Share ARM64 Patch (`QsArm64MasterPatcher`)**: Patches `QSBLE.dll` and `WifiServiceHandler.dll` for BLE/WiFi Direct compatibility on Snapdragon 860 (`nabu`).
- **Galaxy Book Spoofing & SSSE Integration**: Full SMBIOS/registry identity spoofing, scheduled startup tasks (`GalaxyBookNabu`, `GalaxyBookNabu-MultiControlInit`), and patched `GBeSupportService` deployment.

[1.0.0]: https://github.com/pasnov-oleksandr/GalaxyBookNabu/releases/tag/v1.0.0

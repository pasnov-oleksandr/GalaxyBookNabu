# Changelog

## [1.0.1] - 2026-11-27
### Fixed
- Changed `Restart-Service -Name $service.Name -Force` error handling from `-ErrorAction Stop` to `-ErrorAction SilentlyContinue` in `Init-SamsungMultiControlOnLogin.ps1` so non-critical service restart failures do not abort the login initialization script.

## [1.0.0] - 2026-11-21
### Added
- Initial Xiaomi Pad 5 (`nabu`) Windows 11 ARM64 release (`Install-GalaxyBookNabu.ps1`, `AllInNabu.zip`, `SysIdStub.dll`).
- Added `Assert-DeveloperModeEnabled` check prior to extracting and registering `AllInNabu.zip`.

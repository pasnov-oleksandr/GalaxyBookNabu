# Galaxy Book Nabu — Xiaomi Pad 5 (`nabu`) ARM64

Turn your Xiaomi Pad 5 (`nabu`, Snapdragon 860, Adreno 640) into any Samsung Galaxy Book and experience the complete Samsung Galaxy ecosystem on a single device — including dual-booting full **Samsung One UI 8.0 (Android)** alongside **Windows 11 ARM64** with working Microsoft Store Samsung apps.

---

## Overview

Galaxy Book Nabu turns your **Xiaomi Pad 5 (`nabu`)** running **Windows 11 ARM64** into any Samsung Galaxy Book model, giving you full access to Samsung's ecosystem on one device — including Quick Share, Multi Control, Samsung Notes, Second Screen, Storage Share, Samsung Gallery, Samsung Flow, SmartThings, and more.

Because `nabu` also supports dual-booting a full **Samsung One UI 8.0** Android ROM alongside Windows 11 ARM64, you get a complete, all-in-one Samsung tablet and Galaxy Book workstation on a single piece of hardware — with seamless continuity between both environments and your other Galaxy devices.

On Xiaomi Pad 5 (`nabu`) running Windows on ARM, standard x64 scripts fail due to volatile UEFI `SystemIdentification` IDs (`AUT_1302`), Qualcomm Wi-Fi/BLE restrictions, and missing `FamilyMDE` session keys. This fork handles all of these automatically using `AllInNabu.zip` (`SysIdStub.dll`), Desktop enclosure spoofing (`EnclosureType = 0x03`), and native ARM64 patches for Galaxy Connect, Samsung Account, and Quick Share.

---

## Features

- **All-in-One Samsung Ecosystem & Dual-Boot Ready** Turn your Xiaomi Pad 5 (`nabu`) into any Galaxy Book on Windows 11 ARM64 and pair it with a dual-boot Samsung One UI 8.0 ROM on the same tablet
- **26 Galaxy Book Models** Choose from authentic hardware profiles (Galaxy Book6/5/4/3/2, Pro, Ultra, 360)
- **Persistent Samsung Account (`SysIdStub.dll`)** Freestanding ARM64 WinRT in-process server that keeps your device ID static across reboots (fixes `AUT_1302`)
- **Qualcomm Wi-Fi & BLE Support (`nabu`)** Patches Quick Share (`QSBLE.dll` + `WifiServiceHandler.dll`) and Galaxy Connect (`HasSupportedBluetooth` + `EnclosureType = 0x03`) for Snapdragon 860
- **Microsoft Store Update Lock** Freezes Store auto-updates (`AutoDownload = 2`) so patched ARM64 packages are not overwritten
- **Samsung MultiPoint Support** Connect Galaxy Buds to multiple devices seamlessly via Samsung Settings and Cloud Bluetooth Sync
- **Auto-Elevation** Automatically requests admin rights using `gsudo`, native `sudo`, or UAC
- **Diagnostic Logging** Automatic log generation for troubleshooting
- **Smart Package Selection** Choose from Core, Recommended, Recommended Plus, Full Experience, Everything, or custom package combinations
- **Package Manager** Install, downgrade, or uninstall entire profiles from existing installations with status tracking
- **Automated Startup** Registry spoof (`EnclosureType = 0x03`) and `FamilyMDE` keys run automatically on every boot
- **Multi Control Recovery Task** Restarts Samsung services at startup (+5 min) with a 1-minute grace period to improve Multi Control reconnection reliability
- **Differential Install** Skips already-installed packages for faster re-runs
- **Test Mode** Simulate installation without making changes to your system
- **Version Management** Update detection and migration support
- **Easy Uninstall** One-command removal with cleanup
- **Advanced Reset & Repair** Built-in tools to fix app issues, clear caches, and repair permissions
- **Nuke Mode** Optional destructive uninstall to wipe all Samsung app data

---

## Requirements

- **Device:** Xiaomi Pad 5 (`nabu`, Snapdragon 860)
- **OS:** Windows 11 ARM64 (standalone or dual-boot alongside Samsung One UI 8.0)
- **PowerShell:** Built-in Windows PowerShell 5.1 or PowerShell 7.0+
- **Required Files:** `Install-GalaxyBookNabu.ps1` and `AllInNabu.zip` (containing `SysIdStub.dll`) in the same folder
- **Privileges:** Administrator privileges
- **Network:** Active Internet connection
- **System & Store Updates:** Before running the installer, install all pending **Windows Update** patches and update all built-in apps in **Microsoft Store** (`Downloads -> Get updates`, especially **App Installer** / `winget`)

---

## Quick Start

> **Important Before Starting:** Make sure **Windows Update** and all **Microsoft Store** app updates (`Downloads -> Get updates`) are fully installed and **Developer Mode** (`Settings -> System -> For developers -> Developer Mode`) is enabled.

### Option A: One-Line Remote Execution (Recommended)

Place `AllInNabu.zip` in `C:\Toolbox\AllInNabu.zip` (or in your current working directory), open **PowerShell 7 (`pwsh`)** as Administrator, and run:

```powershell
irm https://raw.githubusercontent.com/pasnov-oleksandr/GalaxyBookNabu/main/Install-GalaxyBookNabu.ps1 | iex
```

### Option B: Local Execution

Download `Install-GalaxyBookNabu.ps1` and `AllInNabu.zip` into the same folder (e.g., `C:\Toolbox`), then run locally from PowerShell:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\Install-GalaxyBookNabu.ps1
```

The installer will automatically request administrator privileges if needed.

### Uninstall Options

When running the installer on an existing installation, you have granular uninstall options:

- **Reinstall (nuke + fresh install):** Completely removes everything (preserving BIOS config), then performs a clean installation
- **Uninstall everything:** Removes all Samsung apps, services, scheduled tasks, and configuration
  - **Nuke Mode:** Optionally delete ALL Samsung app data (caches, settings, databases) during uninstall
- **Uninstall apps only:** Removes all installed Samsung apps while keeping services and scheduled tasks
- **Uninstall services only:** Removes scheduled tasks and Samsung services while keeping apps installed

### Manual Install

1. Download `Install-GalaxyBookNabu.ps1` and `AllInNabu.zip` into the same directory
2. Run: `.\Install-GalaxyBookNabu.ps1`
3. Accept UAC prompt when requested
4. Follow the interactive installer

No need to manually "Run as Administrator" — the script handles elevation automatically!

### Uninstall

```powershell
.\Install-GalaxyBookNabu.ps1 -Uninstall
```

### Test Mode (No Changes Applied)

```powershell
.\Install-GalaxyBookNabu.ps1 -TestMode
```

Test mode simulates the entire installation without making any actual changes.

### Fully Autonomous Install (CI / Scripted)

Run the installer non-interactively with all options specified up front:

```powershell
.\Install-GalaxyBookNabu.ps1 -FullyAutonomous -AutonomousModel Book4Pro -AutonomousPackageProfile Recommended -AutonomousConfirmPackages:$true -AutonomousCreateAiSelectShortcut:$false -DebugOutput -LogDirectory "C:\GalaxyBook\Logs"
```

Notes:
- `-AutonomousPackageProfile` supports: `Core`, `Recommended`, `RecommendedPlus`, `Full`, `Everything`, `Custom`, `Skip`.
- For `Custom`, pass `-AutonomousPackageNames` with package names/IDs.
- `-AutonomousModel` accepts either an exact model code (for example `960XGL`) or a family/profile selection (for example `Book4Pro` or `Galaxy Book4 Pro`).
- `-AutonomousAction` supports: `Install` (default), `UpdateSettings`, `UpgradeSSE`, `UninstallAll`, `Cancel`.

---

## Reset & Repair Tools

The installer includes a comprehensive suite of tools to fix common issues with Samsung apps. Select **Reset/Repair Samsung Apps** from the main menu to access:

- **Diagnostics:** Checks installed packages, device data files, and databases
- **Soft Reset:** Clears app caches and temporary files (preserves login)
- **Hard Reset:** Clears caches, device data, and settings (requires re-login)
- **Clear Authentication:** Removes Samsung Account database and credentials
- **Repair Permissions:** Fixes ACLs on app folders
- **Re-register Apps:** Re-registers AppX manifests to fix launch issues
- **Factory Reset:** Completely wipes ALL Samsung data (credentials, devices, DBs, settings)

---

## Package Profiles

### Core Only

Essential packages for basic Samsung ecosystem functionality:

- Samsung Account
- Samsung Settings + Runtime
- Samsung Cloud
- Knox Matrix for Windows
- Galaxy Connect
- Samsung Intelligence Service
- Samsung Bluetooth Sync
- Galaxy Book Experience

### Recommended

Core packages + essential working Samsung apps on `nabu`:

- Quick Share
- Storage Share
- Multi Control
- Nearby Devices
- Samsung Notes
- AI Select
- Samsung Gallery
- Galaxy Buds
- Second Screen

### Recommended Plus

Recommended + additional working Samsung apps:

- Samsung Studio
- Samsung Studio for Gallery
- Samsung Screen Recorder
- Samsung Flow
- SmartThings
- Samsung Parental Controls
- Live Wallpaper
- Galaxy Book Smart Switch

### Full Experience

Recommended Plus + working phone integration:

- Samsung Phone

### Everything

All packages including non-functional / unsupported ones on `nabu`:

- ⚠️ Samsung Recovery
- ⚠️ Samsung Update
- ⚠️ Camera Share
- ⚠️ Samsung Pass
- ⚠️ Quick Search
- ⚠️ Samsung Device Care
- ⚠️ Samsung Find

### Custom Selection

Pick individual packages by category with detailed descriptions.

---

## Package Compatibility Matrix

**Legend:** `✅` Working · `⚠️` Partial · `❌` Not Working  
**Profiles:** `Core` · `Rec` (Recommended) · `Rec+` (Recommended Plus) · `Full` (Full Experience) · `All` (Everything) · `Legacy`  
**PFN Prefixes:** `*_3c1yjt4zspk6g` = `SAMSUNGELECTRONICSCO.LTD.` · `*_wyx1vj98g3asy` = `SAMSUNGELECTRONICSCoLtd.`

| # | Package | Store ID | Package Family (Short) | Profile | Status | Notes |
|---:|---|---|---|---|:-:|:---|
| 1 | Samsung Account | `9P98T77876KZ` | `SamsungAccount_3c1yjt4zspk6g` | Core | ✅ | Patched `SysIdStub.dll` (`C:\ProgramData\SamsungAccount_Nabu`) |
| 2 | Samsung Settings | `9P2TBWSHK6HJ` | `SamsungSettings1.5_3c1yjt4zspk6g` | Core | ✅ | Core UI works; x64 SSSE driver requires Test Mode |
| 3 | Settings Runtime | `9NL68DVFP841` | `SamsungSettingsRuntime_3c1yjt4zspk6g` | Core | ✅ | Required runtime for Samsung Settings |
| 4 | Samsung Cloud | `9NFWHCHM52HQ` | `SamsungCloudPlatformManag_3c1yjt4zspk6g` | Core | ✅ | Cloud storage & sync service |
| 5 | Knox Matrix | `9NJRV1DT8N79` | `KnoxMatrixforWindows_3c1yjt4zspk6g` | Core | ✅ | Samsung Knox security sync |
| 6 | Galaxy Connect | `9NGW9K44GQ5F` | `SamsungContinuityService_wyx1vj98g3asy` | Core | ✅ | Patched `HasSupportedBluetooth` + `EnclosureType=0x03` + `FamilyMDE` |
| 7 | Intelligence Service | `9NS0SHL4PQL9` | `SamsungIntelligenceVoiceS_3c1yjt4zspk6g` | Core | ✅ | Required for Galaxy AI & AI Select |
| 8 | Bluetooth Sync | `9NJNNJTTFL45` | `SamsungCloudBluetoothSync_3c1yjt4zspk6g` | Core | ✅ | Bluetooth device sync |
| 9 | Galaxy Book Exp. | `9P7QF37HPMGX` | `SamsungWelcome_3c1yjt4zspk6g` | Core | ✅ | Samsung app discovery catalog |
| 10 | Quick Share | `9PCTGDFXVZLJ` | `SamsungQuickShare_wyx1vj98g3asy` | Rec | ✅ | Patched `QSBLE.dll` + `WifiServiceHandler.dll` (`isQCDevice`) |
| 11 | Camera Share | `9NPCS7FN6VB9` | `16297BCCB59BC_wyx1vj98g3asy` | All | ❌ | Unsupported on nabu hardware |
| 12 | Storage Share | `9MVNW0XH7HS5` | `4438638898209_wyx1vj98g3asy` | Rec | ✅ | Enabled via Galaxy Connect Desktop mode (`0x03`) |
| 13 | Multi Control | `9N3L4FZ03Q99` | `MultiControl_wyx1vj98g3asy` | Rec | ✅ | Enabled via `FamilyMDE\Settings = 1` (HKLM & HKCU) |
| 14 | Nearby Devices | `9PHL04NJNT67` | `SamsungMyDevices_wyx1vj98g3asy` | Rec | ✅ | Manage & connect nearby Samsung devices |
| 15 | Samsung Notes | `9NBLGGH43VHV` | `SamsungNotes_wyx1vj98g3asy` | Rec | ✅ | Xiaomi Smart Pen pressure & Samsung Cloud sync |
| 16 | AI Select | `9PM11FHJQLZ4` | `SmartSelect_3c1yjt4zspk6g` | Rec | ✅ | Smart screenshot & OCR (`C:\GalaxyBook\AISelect.ps1`) |
| 17 | Samsung Gallery | `9NBLGGH4N9R9` | `PCGallery_3c1yjt4zspk6g` | Rec | ✅ | Photo & video gallery with cloud sync |
| 18 | Galaxy Buds | `9NHTLWTKFZNB` | `GalaxyBuds_3c1yjt4zspk6g` | Rec | ✅ | Galaxy Buds management & settings |
| 19 | Samsung Pass | `9MVWDZ5KX9LH` | `SamsungPass_3c1yjt4zspk6g` | All | ❌ | Requires genuine Samsung TPM/security hardware |
| 20 | Second Screen | `9PLTXW5DX5KB` | `SecondScreen_wyx1vj98g3asy` | Rec | ✅ | Wireless display extension via Galaxy Connect |
| 21 | Samsung Studio | `9P312B4TZFFH` | `SamsungStudio_3c1yjt4zspk6g` | Rec+ | ✅ | Photo & video editing suite |
| 22 | Studio for Gallery | `9NND8BT5WFC5` | `SamsungStudioForGallery_3c1yjt4zspk6g` | Rec+ | ✅ | Gallery-integrated editing tools |
| 23 | Screen Recorder | `9P5025MM7WDT` | `SamsungScreenRecording_3c1yjt4zspk6g` | Rec+ | ⚠️ | Shows "optimized for Galaxy Books" banner; works normally |
| 24 | Samsung Flow | `9NBLGGH5GB0M` | `SamsungFlux_wyx1vj98g3asy` | Rec+ | ✅ | Phone-tablet sync, Smart View, clipboard & notifications |
| 25 | SmartThings | `9N3ZBH5V7HX6` | `SmartThingsWindows_3c1yjt4zspk6g` | Rec+ | ✅ | SmartThings home control |
| 26 | Parental Controls | `9N5GWJTCZKGS` | `SamsungParentalControls_3c1yjt4zspk6g` | Rec+ | ✅ | Family & child device management |
| 27 | Live Wallpaper | `9N1G7F25FXCB` | `Sidia.LiveWallpaper` | Rec+ | ✅ | Animated Galaxy Book wallpapers |
| 28 | Smart Switch | `9PJ0J9KQWCLB` | `SmartSwitchforGalaxyBook_3c1yjt4zspk6g` | Rec+ | ✅ | Data backup & transfer |
| 29 | Device Care | `9NBLGGH4XDV0` | `SamsungPCCleaner_3c1yjt4zspk6g` | All | ❌ | Requires genuine Samsung hardware/drivers |
| 30 | Samsung Phone | `9MWJXXLCHBGK` | `SamsungPhone_3c1yjt4zspk6g` | Full | ✅ | Calls/SMS integration via Continuity |
| 31 | Samsung Find | `9MWD59CZJ1RN` | `SamsungFind_3c1yjt4zspk6g` | All | ❌ | Not supported on nabu |
| 32 | Quick Search | `9N092440192Z` | `QuickSearch_3c1yjt4zspk6g` | All | ❌ | Not supported on nabu |
| 33 | Samsung Recovery | `9NBFVH4X67LF` | `SamsungRecovery_3c1yjt4zspk6g` | All | ❌ | Requires genuine Samsung recovery partition |
| 34 | Samsung Update | `9NQ3HDB99VBF` | `SamsungUpdate_3c1yjt4zspk6g` | All | ❌ | Requires genuine Samsung BIOS/UEFI |
| 35 | Studio Plus (Legacy) | `9PLPF77D2R18` | `SamsungStudioPlus_3c1yjt4zspk6g` | Legacy | ❌ | Replaced by Samsung Studio (`9P312B4TZFFH`) |

## Dual-Boot Ecosystem: Windows 11 ARM64 + Samsung One UI 8.0

Xiaomi Pad 5 (`nabu`) allows you to run both **Windows 11 ARM64 (Galaxy Book mode)** and a **full Samsung One UI 8.0 Android ROM** on the same tablet via dual-boot:

- **One Device, Complete Samsung Experience:** Switch between a full Samsung One UI 8.0 tablet experience and a Windows 11 ARM64 Galaxy Book workstation without carrying two separate devices.
- **Unified Samsung Account & Cloud Sync:** Your Samsung Account (`SysIdStub.dll` on Windows + native Samsung Services on One UI 8.0) keeps **Samsung Notes**, **Samsung Gallery**, **Samsung Cloud**, **Samsung Flow**, and **Galaxy Buds** synchronized across both OS partitions and with your Galaxy phone.

---

## Wi-Fi & Bluetooth Compatibility (`nabu`)

Xiaomi Pad 5 (`nabu`) uses a **Qualcomm Snapdragon 860 (`WCN3990`)** Wi-Fi and Bluetooth controller instead of Intel AX/BE hardware. This fork enables full wireless functionality on `nabu`:

- **Samsung Continuity (Galaxy Connect):**
  - Patches `HasSupportedBluetooth` (`0x17 0x2A` → `ldc.i4.1; ret`) in `CommonsNet.dll` and `Commons.dll` so Qualcomm Bluetooth is accepted.
  - Sets `EnclosureType = 0x03` (Desktop mode) in `HKLM\HARDWARE\DESCRIPTION\System\BIOS` and `HKLM\SYSTEM\HardwareConfig\Current` so `DeviceSupportHelper.HasSupportedWiFi` bypasses the Intel/Qualcomm laptop whitelist.
  - Registers `FamilyMDE\Settings = 1` in both `HKLM` and `HKCU` under `Software\SAMSUNG\SamsungContinuityService\FamilyMDE` so Multi Control P2P stays connected.
- **Quick Share (`2.4.36.0` ARM64):**
  - Patches `QSBLE.dll` (`ShouldSupportExtendedPacket` → `false`) for Qualcomm BLE compatibility.
  - Patches `WifiServiceHandler.dll` (`QCWifiService::isQCDevice` → `mov w0, #1; ret`) so Quick Share enables Qualcomm Wi-Fi Direct.

---

## AI Select (Smart Select)

AI Select is Samsung's intelligent selection tool. The installer creates launcher scripts in `C:\GalaxyBook\` for easy hotkey binding.

### Launch URI

```text
shell:AppsFolder\SAMSUNGELECTRONICSCO.LTD.SmartSelect_3c1yjt4zspk6g!App
```

### Method 1: PowerToys URI (Recommended - Fastest)

1. Install PowerToys from Microsoft Store
2. Open PowerToys → Keyboard Manager
3. Remap a key (e.g., Right Alt → `Win+Ctrl+Alt+S`)
4. Remap a shortcut → `Win+Ctrl+Alt+S` → Open URI: `shell:AppsFolder\SAMSUNGELECTRONICSCO.LTD.SmartSelect_3c1yjt4zspk6g!App`

### Method 2: PowerToys Run Program

1. Install PowerToys
2. Keyboard Manager → Remap a shortcut
3. Set shortcut (e.g., `Ctrl+Shift+S`) → Action: Run Program
4. Program: `powershell.exe`
5. Args: `-WindowStyle Hidden -File "C:\GalaxyBook\AISelect.ps1"`

### Method 3: AutoHotkey (AHK)

```autohotkey
; AI Select launcher - save as AISelect.ahk
#Requires AutoHotkey v2.0
RAlt::Run "shell:AppsFolder\SAMSUNGELECTRONICSCO.LTD.SmartSelect_3c1yjt4zspk6g!App"
```

### Method 4: Desktop Shortcut (Standard)

1. Find `AI Select.lnk` on your Desktop (created by installer)
2. Right-click → Properties
3. Click in the "Shortcut key" field and press your desired key combination (e.g., `Ctrl+Alt+S`)
4. Click OK

---

## `config.plist` Generation

Generate configuration data only:

```powershell
.\Install-GalaxyBookNabu.ps1 -ConfigurationOnly -FullyAutonomous -AutonomousModel Book4Ultra -AutonomousCountryCode US
```

Write the generated values straight into an existing `config.plist`:

```powershell
.\Install-GalaxyBookNabu.ps1 -ConfigurationOnly -FullyAutonomous -AutonomousModel Book4Pro -AutonomousCountryCode US -ConfigurationPath "D:\EFI\OC\config.plist"
```

---

## How It Works

1. **Registry Spoof:** Modifies system registry (`BIOS`, `HardwareConfig\Current`, `SystemInformation`) to identify as a Samsung Galaxy Book profile with `EnclosureType = 0x03` (Desktop) and sets `FamilyMDE\Settings = 1`
2. **Startup Tasks:** Creates scheduled tasks (`GalaxyBookNabu` and `GalaxyBookNabu-MultiControlInit`) that run on every boot
3. **Core 3 Patched Apps (`AllInNabu.zip`):**
   - **[1/3] Galaxy Connect:** Installs `9NGW9K44GQ5F`, patches `HasSupportedBluetooth` in `CommonsNet.dll` and `Commons.dll`, and registers `C:\ProgramData\SamsungContinuity_Nabu`
   - **[2/3] Samsung Account:** Installs `9P98T77876KZ`, extracts `SysIdStub.dll` from `AllInNabu.zip`, patches `SAClient.dll`, registers the WinRT in-process server in `AppxManifest.xml`, and registers `C:\ProgramData\SamsungAccount_Nabu`
   - **[3/3] Quick Share:** Installs `9PCTGDFXVZLJ`, patches `QSBLE.dll` and `WifiServiceHandler.dll`, and registers `C:\ProgramData\SamsungQuickShare_Nabu`
4. **Store Update Freeze:** Sets `HKLM\SOFTWARE\Policies\Microsoft\WindowsStore\AutoDownload = 2`
5. **Package Installation:** Installs selected Samsung apps from Microsoft Store
6. **Configuration:** Sets up shortcuts and configuration files

---

## Available Models

| Model | Family | Gen |
| :--- | :--- | :---: |
| `944XJG` | Galaxy Book6 Pro | 2026 |
| `960XJG` | Galaxy Book6 Pro | 2026 |
| `960UJG` | Galaxy Book6 Ultra | 2026 |
| `960UJH` | Galaxy Book6 Ultra | 2026 |
| `760VJG` | Galaxy Book6 | 2026 |
| `960XHA` | Galaxy Book5 Pro | 2025 |
| `940XHA` | Galaxy Book5 Pro | 2025 |
| `960QHA` | Galaxy Book5 Pro 360 | 2025 |
| `750QHA` | Galaxy Book5 360 | 2025 |
| `960XGL` | Galaxy Book4 Ultra | 2024 |
| `960XGK` | Galaxy Book4 Pro | 2024 |
| `940XGK` | Galaxy Book4 Pro | 2024 |
| `960QGK` | Galaxy Book4 Pro 360 | 2024 |
| `750XGK` | Galaxy Book4 | 2024 |
| `750XGL` | Galaxy Book4 | 2024 |
| `750QGK` | Galaxy Book4 360 | 2024 |
| `960XFH` | Galaxy Book3 Ultra | 2023 |
| `960XFG` | Galaxy Book3 Pro | 2023 |
| `960QFG` | Galaxy Book3 Pro 360 | 2023 |
| `750XFG` | Galaxy Book3 | 2023 |
| `750XFH` | Galaxy Book3 | 2023 |
| `730QFG` | Galaxy Book3 360 | 2023 |
| `950XGK` | Galaxy Book2 Pro Special Edition | 2022 |
| `930XDB` | Galaxy Book Series | 2021 |
| `935QDC` | Galaxy Book Series | 2021 |
| `930SBE` | Notebook 9 Series | 2020 |

---

## Installation Guide

### Step-by-Step

1. **Run Installer as Administrator**
   ```powershell
   .\Install-GalaxyBookNabu.ps1
   ```
2. **Model Selection**
   - Choose a Galaxy Book generation and model profile
3. **File Setup**
   - Creates `%USERPROFILE%\.galaxy-book-nabu`
   - Generates `GalaxyBookSpoof.bat` (`EnclosureType = 0x03`) and `Init-SamsungMultiControlOnLogin.ps1`
4. **Startup Task Creation**
   - Creates scheduled task `GalaxyBookNabu` (runs `GalaxyBookSpoof.bat` as `SYSTEM` at startup)
   - Creates scheduled task `GalaxyBookNabu-MultiControlInit` (restarts Samsung services at startup +5 minutes)
5. **Core 3 Patched Packages (`AllInNabu.zip`)**
   - Deploys patched **Galaxy Connect**, **Samsung Account** (`SysIdStub.dll`), and **Quick Share** into `C:\ProgramData\*_Nabu`
   - Sets `AutoDownload = 2` in `HKLM\SOFTWARE\Policies\Microsoft\WindowsStore`
6. **Package Selection**
   - Choose installation profile (`Recommended`, `Recommended Plus`, `Full Experience`, `Everything`, or `Custom`)
7. **Apply Registry Spoof & `FamilyMDE`**
   - Applies SMBIOS keys and `FamilyMDE\Settings = 1` (`HKLM` + `HKCU`) immediately
8. **Reboot**
   - Restart your tablet, sign in to Samsung Account once, and verify persistence across reboots

---

## Building `SysIdStub.dll`

To compile `SysIdStub.dll` from source on Debian 12 / Ubuntu 24.04:

```bash
sudo apt update && sudo apt install -y clang lld

clang --target=aarch64-pc-windows-msvc -O2 -ffreestanding -fno-builtin \
    -c src/SysIdStub.c -o build/SysIdStub.o

lld-link /DLL /NOENTRY /MACHINE:ARM64 \
    /EXPORT:DllGetActivationFactory /EXPORT:DllCanUnloadNow \
    /OUT:release/SysIdStub.dll build/SysIdStub.o

zip -j AllInNabu.zip release/SysIdStub.dll
```

---

## Troubleshooting

### Samsung Account Signs Out After Reboot (`AUT_1302`)

- Check that Samsung Account is registered from `C:\ProgramData\SamsungAccount_Nabu`:
  ```powershell
  Get-AppxPackage -Name "*SamsungAccount*" | Select-Object Name, Version, InstallLocation, IsDevelopmentMode
  ```
- Verify Microsoft Store auto-updates are disabled (`AutoDownload = 2` under `HKLM\SOFTWARE\Policies\Microsoft\WindowsStore`).

### App Fails to Launch (`0x80040154 Class not registered`)

- Grant `ALL APPLICATION PACKAGES` (`*S-1-15-2-1`) read/execute permissions on the patched directories:
  ```powershell
  icacls.exe "C:\ProgramData\SamsungAccount_Nabu" /grant "*S-1-15-2-1:(OI)(CI)(RX)" /T
  icacls.exe "C:\ProgramData\SamsungContinuity_Nabu" /grant "*S-1-15-2-1:(OI)(CI)(RX)" /T
  icacls.exe "C:\ProgramData\SamsungQuickShare_Nabu" /grant "*S-1-15-2-1:(OI)(CI)(RX)" /T
  ```

### Multi Control Stops Working or Drops (`DISCONNECTED`)

- Check `FamilyMDE\Settings` in `HKCU` and `EnclosureType` in `BIOS`:
  ```powershell
  Get-ItemProperty "HKCU:\Software\SAMSUNG\SamsungContinuityService\FamilyMDE" -Name Settings
  Get-ItemProperty "HKLM:\HARDWARE\DESCRIPTION\System\BIOS" -Name EnclosureType
  ```
- Run recovery script manually:
  ```powershell
  powershell.exe -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.galaxy-book-nabu\Init-SamsungMultiControlOnLogin.ps1"
  ```

### Quick Share Not Working

- Ensure Wi-Fi and Bluetooth are enabled and `SamsungQuickShareService` is running (`Get-Service *QuickShare*`).

### Scheduled Task Not Running

- Check Task Scheduler for both `GalaxyBookNabu` and `GalaxyBookNabu-MultiControlInit`. Both must run as `SYSTEM` with highest privileges.

---

## FAQ

**Q: Is this safe to use?**
A: Yes, it only modifies volatile registry keys that reset on reboot and registers patched AppX packages in `C:\ProgramData\*_Nabu`.

**Q: Will this void my warranty?**
A: This doesn't modify firmware or partitions. It only changes Windows registry values and AppX registrations.

**Q: Why is `SysIdStub.dll` needed on Xiaomi Pad 5 (`nabu`)?**
A: On `nabu` WOA, `SystemIdentification.GetSystemIdForPublisher()` generates a new 32-byte `Source=Uefi` ID on every reboot, causing `AUT_1302` sign-out errors. `SysIdStub.dll` provides a static 32-byte ID across reboots.

**Q: Why is `EnclosureType` set to `0x03`?**
A: In `Commons.dll`, Samsung only enforces the Intel/Qualcomm Wi-Fi whitelist when `EnclosureType` reports as a laptop (`0x0A` / `0x1F`). Setting `0x03` (Desktop) bypasses the Wi-Fi whitelist on `nabu`.

**Q: Can I dual-boot Samsung One UI 8.0 alongside Windows 11 ARM64 on `nabu`?**
A: Yes. Xiaomi Pad 5 (`nabu`) supports dual-booting a full Samsung One UI 8.0 Android ROM alongside Windows 11 ARM64, letting you use both Samsung tablet and Galaxy Book features on a single device.

---

## Uninstallation

### Complete Removal

```powershell
.\Install-GalaxyBookNabu.ps1 -Uninstall
```

### Manual Cleanup

```powershell
Unregister-ScheduledTask -TaskName "GalaxyBookNabu" -Confirm:$false -ErrorAction SilentlyContinue
Unregister-ScheduledTask -TaskName "GalaxyBookNabu-MultiControlInit" -Confirm:$false -ErrorAction SilentlyContinue
Remove-Item "$env:USERPROFILE\.galaxy-book-nabu" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "C:\ProgramData\SamsungAccount_Nabu", "C:\ProgramData\SamsungContinuity_Nabu", "C:\ProgramData\SamsungQuickShare_Nabu" -Recurse -Force -ErrorAction SilentlyContinue
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsStore" -Name "AutoDownload" -ErrorAction SilentlyContinue
Restart-Computer
```

---

## Privacy & Security

- **No telemetry:** Script doesn't send any data
- **Local only:** All operations are local to your tablet
- **Open source:** Full code is available for review
- **Reversible:** Complete uninstall available

---

## License

This project is licensed under the MIT License — see the `LICENSE` file for details.

## Credits

- Original script by `@obrobrio2000`
- Enhanced and maintained by `@Bananz0`
- Desktop Enclosure (`0x03`) & `FamilyMDE` fix from PR #100 / `Chiragsd13/GalaxyConnectEnabler`
- Bluetooth device removal by `@m-a-x-s-e-e-l-i-g` / `@RS-DU34` (`powerBTremover`)
- Xiaomi Pad 5 (`nabu`) ARM64 port by `@pasnov-oleksandr` (`SysIdStub.dll`, Qualcomm `QSBLE`/`WifiServiceHandler` ARM64 patches, CIL `HasSupportedBluetooth` patch)

## Disclaimer

- This tool is for educational and personal use only
- Not affiliated with or endorsed by Samsung Electronics or Xiaomi
- Use at your own risk
- Samsung may update their apps to detect or block this method

> **Prerequisite (Windows 11 ARM64 / Xiaomi Pad 5 `nabu`):** Enable **Developer Mode** (`Settings -> System -> For developers -> Developer Mode`) before running `Install-GalaxyBookNabu.ps1` so local ARM64 AppX manifests (`AllInNabu.zip`) can be registered.

# Fujitsu Esprimo Q958 Mini Hackintosh OpenCore

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.7-cyan.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) ![MacOS](https://img.shields.io/badge/macOS-14.7.5_to_26.3b-purple.svg) [![release](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases)

<img src="https://github.com/user-attachments/assets/3fc086f2-1be3-4a90-83f9-efdc702d51d2" width="100%%" />

---
**TABLE of CONTENTS**

- [About](#about)
- [System Specs](#system-specs)
- [What works?](#what-works)
	- [Notable Features](#notable-features)
- [BIOS Settings](#bios-settings)
- [EFI Folder Content](#efi-folder-content)
- [Deployment](#deployment)
	- [Prepare EFI](#prepare-efi)
	- [Disable CFG Lock](#disable-cfg-lock)
	- [macOS Installation](#macos-installation)
- [Post-Install](#post-install)
	- [Enable Intel WiFi in macOS Sequoia/Tahoe](#enable-intel-wifi-in-macos-sequoiatahoe)
	- [Modify Power Management Settings](#modify-power-management-settings)
	- [Disable Gatekeeper (optional)](#disable-gatekeeper-optional)
	- [Strengthen Security (optional)](#strengthen-security-optional)
	- [Enable brightness control for external displays (optional)](#enable-brightness-control-for-external-displays-optional)
	- [Add Eject button for optical drive to Menu Bar (optional)](#add-eject-button-for-optical-drive-to-menu-bar-optional)
- [Geekbench 5 Benchmarking Results](#geekbench-5-benchmarking-results)
	- [CPU](#cpu)
	- [iGPU](#igpu)
- [Maintenance](#maintenance)
	- [Adding kext URLs to OCAT](#adding-kext-urls-to-ocat)
- [Credits](#credits)

---

## About
OpenCore EFI folder for running macOS Sonoma, Sequoia and even Tahoe on the Fujitsu Esprimo Q958 Small Form Factor PC. As far as I am aware, this is the first OpenCore EFI for this system. I am using it for office/streaming purposes. It's a quite capable and powerful mini-pc. It has a true 300-series chipset, the system can be maxed-out with with an i9-9900T (8 Cores 16 Threads) and 64 Gigs of RAM.

## System Specs

**Component**   | Description 
---------------:|:--------------------------------|
**Model**       | [Fujitsu Esprimo Q958](https://www.fujitsu.com/id/products/computing/pc/ap/desktops/esprimo-q958/)
**Formfactor**  | Small Form Factor (Mini-PC)
**Chipset**     | [Intel Q370](https://www.intel.com/content/www/us/en/products/sku/133282/intel-q370-chipset/specifications.html)
**CPU**         | 9th Gen Intel® Core™ [i7-9700T](https://www.intel.com/content/www/us/en/products/sku/191048/intel-core-i79700t-processor-12m-cache-up-to-4-30-ghz/specifications.html) (Coffee Lake) 
**GPU**         | Intel UHD Graphics 630 
**RAM**         | 32 GB DDR4 (2666 MHz)
**Storage**     | WD Blue SN550 NVMe SSD       
**NIC**         | Intel® I219-LM (1 Gbit)
**WiFi/BT**     | [Intel® Wireless-AC 9260](https://www.intel.de/content/www/de/de/products/sku/99445/intel-wirelessac-9260/specifications.html) 160MHz <ul> <li> WiFi Firmware: `iwm-9260-46` <li> BT Firmware: <ul><li>`ibt-18-16-1.sfi` <li>`ibt-18-16-1.ddc`
**Sound**       | Realtek ALC671 (Layout-ID: 16)
**I/O**         | Serial Port (DE-9, disabled)
**BIOS**        | [R1.38.0](https://support.ts.fujitsu.com/IndexDownload.asp?lng=de&OpenTab=) (08/29/22024)
**SMBIOS**      | `iMac19,1` 
**Support Files** | [Downloads for Esprimo Q958](https://support.ts.fujitsu.com/IndexDownload.asp?lng=en&OpenTab=)

## What works?

- [x] OS:
  - [x] macOS Sonoma+
  - [x] Windows 10+
- [x] iGPU
- [x] SATA drive
- [x] NVME drive
- [x] USB port mapping (USB 2, USB 3.1, USB C, Bluetooth)
- [x] Ethernet
- [x] Audio (Line-out, Headphone, Mic, Integrated Speaker)
- [x] Intel Wi-Fi
- [x] Bluetooth
- [x] Sleep and Wake

> [!IMPORTANT]
> 
> This EFI uses `AirportItlwm.kext` for WLAN. It supports Handoff, Universal Clipboard, Location Services and Instant Hotspot support but iServices won't work unless root patches are applied in Post-Install with OCLP-Mod. 

### Notable Features

- [x] Hibernation fully working (Modes 3 and 25)
- [x] All 3 Graphics ports are working (`con0` = DVI, `con1` = DP1 (bottom), `con2` = DP2 (Top))
- [x] USB Port Mapping via ACPI to maximize compatibility
- [x] 3D Globe in Apple Maps.
- [x] Optimized EFI folder size by using slimmed kexts (11 MB in total instead of the default 54 MB)

## BIOS Settings

<details>
<summary><b>Click to show BIOS settings</b></summary><br>

Begin by loading "Optimized Defaults" (under Save & Exit &rarr; "Restore Defaults"). Next, adjust the following settings for booting macOS:

- **Advanced** [TAB]
	- **CPU Configuration**
		- **Package C State Limit**: 
			- [**CPU Default**] &rarr; Causes the fan to ramp up much later at around at around 60° C. Set to `CPU Default` for a more silent operation. Otherwise the fan goes ham once the screensaver kicks in.
	- **Drive Configuration**
		- OnBoard SATA Configuration
			- SATA Mode: **AHCI** 
	- **Trusted Computing**
		- TPM Support: **Disabled**
	- **Super IO Configuration**
		- Serial Port 1 Configuration
			- Serial Port: **Disabled**
	- **Network Stack Configuration**
		- Network Stack: **Disabled**
	- **Graphic Configuration**
		- Primary Display: Internal Graphics
		- Internal Graphic: Enabled
			- DVMT Shared Memory Size: 64 MB
			- DVM Total Graphics Memory Size: 256 MB (or MAX)
	- **System Management**
		- **Fan Control**: 
			- [**Auto**]: Less aggressive fan activity
- **Security** [TAB] 
    - Secure Boot Configuration:
        - Secure Boot Control: Disabled
- **Boot** [TAB]
	- New UEFI OS Option Policy: Place Last
	- Boot Option Priorities:
		- Boot Option #1: OpenCore
		- Windows Boot Manager 
</details>

## EFI Folder Content

<details>
<summary><strong>Click to show</strong></summary><br>


```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── DMAR.aml
    │   ├── SSDT-EC.aml
    │   ├── SSDT-MCHC.aml
    │   ├── SSDT-OSDW.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-PMC.aml
    │   ├── SSDT-RTCAWAC
    │   ├── SSDT-SBUS.aml
    │   ├── SSDT-USBX.aml
    │   └── SSDT-XHUB.aml
    ├── Drivers
    │   ├── HfsPlus.efi
    │   ├── OpenCanopy.efi
    │   ├── OpenRuntime.efi
    │   └── ResetNvramEntry.efi
    ├── Kexts
    │   ├── AdvancedMap.kext (macOS 12+)
    │   ├── AirportItlwm_Sequoia.kext
    │   ├── AirportItlwm_Sonoma.kext
    │   ├── AMFIPass.kext (macOS 12+)
    │   ├── AppleALC.kext
    │   ├── BlueToolFixup.kext (macOS 12+)
    │   ├── CPUFriend.kext
    │   ├── CPUFriendDataProvider.kext
    │   ├── IntelBluetoothFirmware.kext
    │   ├── IntelBTPatcher.kext
    │   ├── IntelMausiEthernet.kext
    │   ├── IO80211FamilyLegacy.kext (macOS 14+)
    │   ├── IOSkywalkFamily.kext (macOS 14+)
    │   ├── itlwm.kext
    │   ├── Lilu.kext
    │   ├── NVMeFix.kext
    │   ├── RestrictEvents.kext (macOS 11+)
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── VirtualSMC.kext
    │   └── WhateverGreen.kext
    ├── OpenCore.efi
    ├── Resources (NOTE: shows sub-folders only, no files)
    │   ├── Font
    │   └── Image
    │       └── Acidanthera
    │       │   └── GoldenGate
    │       └── Label
    └── config.plist
```
</details>

## Deployment

### Prepare EFI
- Download the latest OC EFI folder from the [Releases](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases) section
- Extract it
- Open the `config.plist` with [OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases) 
- Go to `Misc/Tools` and set enable the `CFGLock` tool.
- Next, go to `PlatformInfo/generic`. Press "Generate" to create Serials, MLB etc. (or use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- Save the `config.plist`
- Copy the EFI folder to a FAT32 formatted USB flash drive and reboot
- Press <kbd>F12</kbd> to enter the BIOS Boot Menu
- Select the USB Flash Drive and press <kbd>Enter</kbd> to run OpenCore

Continue with the next section, "Disable CFG Lock"

> [!NOTE]
>
> When using macOS Sequoia or older, disable/delete the `unfairvgva=4` boot-arg if you want to use AppleTV+ or Apple Music.

### Disable CFG Lock
- In the OC Bootmenu, press space bar to show hidden tools
- Select `CFGLock.efi` and press <kbd>Enter</kbd> to run it
- Follow the on-screen Instructions to disable CFGLock (set it to `0`)
- Quit the tool
- Boot into macOS
- Mount EFI partition and open your `config.plist`
- Disable the `CFGLock` Tool (`Misc/Tools`)
- Save your config and reboot

> [!IMPORTANT]
>
> Disabling CFG Lock this way is not a persistent. Once you reset the BIOS back to defaults, you have to run `CFGLock.efi` again to disable it – otherwise macOS won't boot without the `AppleXcpmCfgLock` Quirk.

### macOS Installation
* Put the EFI folder at the root of a FAT32-formatted USB flash drive. You can boot OpenCore from it
* To create a macOS USB installer, follow Dortania’s [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)

## Post-Install
This section contains post-install-measures to enable features, work around issues and some optional settings.

### Enable Intel WiFi in macOS Sequoia/Tahoe

In order for Wi-Fi to work in macOS Sequoia/Tahoe, you have to apply root patches with either OCLP Mod. Make sure to connect the system via LAN so OCLP-Mod can download additionally required ressources.

- [Download the latest release of OCLP-Mod](https://github.com/laobamac/OCLP-Mod/releases)
- Install and run OCLP-Mod
- Click the top right button:<br> <img width="600" height="331" alt="patch01" src="https://github.com/user-attachments/assets/19dc7610-829c-4bd5-9e99-a0938331b50e" />
- It should say "Intel" somewhere in the Chines text of the Patches that will be applied:<br><img width="603" height="332" alt="oclpmod_intel01" src="https://github.com/user-attachments/assets/f49ecc37-e644-4e67-851b-f54fce640935" />
- Click the first button from the top to start root patching.
- Additional files required for patching will be downloadded automatically:<br><img width="415" height="288" alt="oclpmod_intel02" src="https://github.com/user-attachments/assets/391d5440-d3f4-4629-9dbb-e13fe511cbdd" />
- Once that's done, patching will commence:<br> ![intel_spoof06](https://github.com/user-attachments/assets/ced653f7-0807-4aef-82cb-eabf35b08884)
- Reboot the system
- Reset NVRAM
- Boot into macOS Sequoia/Tahoe
- WiFi should work now, so should Audio (applies to macOS Tahoe only)

>[!IMPORTANT]
>
> Once root patches are applied, the security seal of the system volume will be broken. And once it is broken, the complete macOS version will be downloaded every time an update for macOS is available. The workaround would be to revert root patches *before* installing updates and then use LAN to to download and install incremental updates. But there's a chance that applying incremental updates will fail. In this case, the full installer will be downloaded on the next attempt.

### Modify Power Management Settings
Open Terminal and enter the following commands, to adjust Power Management. If you don't want to use Hibernation, use `hibernatemode 3` instead:

```
# Enable hibernatemode 25 (sleep to disk, power off RAM)

sudo pmset -a hibernatemode 25 

# Enable standby (required for hibernation to actually work)

sudo pmset -a standby 1

# Enable autopoweroff (transition to hibernation)

sudo pmset -a autopoweroff 1

# Set autopoweroff delay to 5 minutes (300 seconds)

sudo pmset -a autopoweroffdelay 300

# Set standby delays (time before entering standby/hibernation)

sudo pmset -a standbydelayhigh 600    # 10 minutes when battery > 50%
sudo pmset -a standbydelaylow 600     # 10 minutes when battery < 50%

# Disable wake-causing features

sudo pmset -a powernap 0              # Disable Power Nap (prevents background wake)
sudo pmset -a tcpkeepalive 0          # Prevent network from waking system
sudo pmset -a proximitywake 0         # Disable wake when iPhone/iPad nearby
sudo pmset -a ttyskeepawake 0         # Prevent remote login sessions from preventing sleep
sudo pmset -a womp 0                  # Disable wake-on-magic-packet (wake-on-LAN)
```

Test sleep and wake by entering `pmset sleepnow`. Wait 30 seconds and move the mouse or press a key on the keyboard. The system should wake and the screen(s) should wake when using HibernateMode 3 or 25.

> [!NOTE]
>
> For more configuration options, follow my [hibernation configuration guide](https://github.com/5T33Z0/OC-Little-Translated/tree/main/Content/04_Fixing_Sleep_and_Wake_Issues/Changing_Hibernation_Modes) properly.

### Disable Gatekeeper (optional)
I disable Gatekeeper on all my systems by default, since it blocks running scripts from github etc. To do so, enter `sudo spctl --master-disable` in Terminal. Disabling Gatekeeper in macOS Sequoia and Tahoe requires [additional steps](https://github.com/5T33Z0/OCLP4Hackintosh/tree/main/Guides/Disable_Gatekeeper.md).

###  Strengthen Security (optional)
In `config.plist`, navigate to `Misc/Security/SecureBootModel` and change it to: `Default`. But you may have to disable it when updating macOS, otherwise the installer might crash. Ever since the release of macOS Sonoma 14.4, it does not longer work correctly during install.

### Enable brightness control for external displays (optional)
There's a new app called [**MonitorControl**](https://github.com/MonitorControl/MonitorControl) which allows controlling the brightness and contrast of attached external displays right from the menu bar and/or keyboard.

### Add Eject button for optical drive to Menu Bar (optional)
macOS locks the optical drive sometimes so that you can't open it by pressing the physical eject button – even if no media is present. If you have an Apple Keyboard, you can simply press the <kbd>Eject</kbd> key to open the drive. 

But if you don't, you can add an Eject button to the menu bar: 

- Open Finder and navigate to: `System/Library/CoreServices/Menu Extras`
- Double-click on `Eject.menu`. This adds the Eject button icon to the Menu Bar: <br>![Eject](https://github.com/user-attachments/assets/9ed29614-d4e8-48f6-a2a1-93b48a6d36da)

## Geekbench 5 Benchmarking Results

### CPU

**macOS**:

![gb5_sonoma](https://github.com/user-attachments/assets/88c6bd9b-4439-4b85-98e2-81dcdf675da9)

**Test results**: [https://browser.geekbench.com/v5/cpu/23512366](https://browser.geekbench.com/v5/cpu/23512366)

**Windows 10**:

<img width="854" height="334" alt="Win10_GB5" src="https://github.com/user-attachments/assets/2d3c1e89-ca49-452c-8130-dcecab4b2461" />

**Test results**: [https://browser.geekbench.com/v5/cpu/24103178](https://browser.geekbench.com/v5/cpu/24103178)

### iGPU

**macOS**:

Metal | OpenCL | RPS Control
:----:|:------:|:------------:
[**4941**](https://browser.geekbench.com/v5/compute/6868138) | [**5341**](https://browser.geekbench.com/v5/compute/6868141) | **ON**
[4776](https://browser.geekbench.com/v5/compute/6868144) | [4995](https://browser.geekbench.com/v5/compute/6868143) | OFF

I also tested force-loading Apple's GUC firmware ([4858 points in the Metal test](https://browser.geekbench.com/v5/compute/6872229/claim?key=253757)), but that's no usable option for me since it causes Firefox Tabs to crash. It has no impact on Safari, though.

> [!TIP]
>
> - If iGPU performance is sufficient for your needs and you want a nearly silent operation of the system, disable the `rps-control` property by changing it to `00000000`
> - If you need higher iGPU performance, enable `rps-control` by changing the value to `01000000`. But this will increase temps which results in more fan activity.

**Windows 10** 

Vulkan | OpenCL | 
:----:|:------:|
[**6657**](https://browser.geekbench.com/v5/compute/6925530) | [6380](https://browser.geekbench.com/v5/compute/6925529) 

## Maintenance

### Adding kext URLs to OCAT

OCAT does not contain all the links to kext repos used in my EFI folder. So in order to fetch updates for them, you need to add them to `KextURL.txt`.

**Instructions**

1. Open Finder
2. Press <kbd>CMD</kbd>+<kbd>Shift</kbd>+<kbd>G</kbd>
3. Paste in this path and hit <kbd>Enter</kbd>: 
	```
	~/.config/OCAuxiliaryTools/
	```
4. Open `KextUrl.txt` with TextEdit
5. Add the following entries:
	```text
	AdvancedMap.kext | https://github.com/notjosh/AdvancedMap
	AMFIPass.kext | https://github.com/bluppus20/AMFIPass
	IntelMausiEthernet.kext | https://github.com/Mieze/IntelMausiEthernet
	```
6. Save the file

The next time, you check for kext updates in OCAT (enable the `Dev` option), the color of the squares in front of the kexts above will no longer be grey (not found) but either red (outdated) or green (up to date). All the kexts coming from the OCLP repo can't be fetched automatically, so they will remain grey.

## Credits
- Acidanthera for the [**OpenCore**](https://github.com/acidanthera/OpenCorePkg) Bootloader
- Dortania for [**OpenCore Legacy Patcher**](https://github.com/dortania/OpenCore-Legacy-Patcher)
- lzhoang2801 for [**OpCore Simplify**](https://github.com/lzhoang2801/OpCore-Simplify)
- ic005k for [**OC Auxiliary Tools**](https://github.com/ic005k/OCAuxiliaryTools)
- CorpNewt for [**SSDTTime**](https://github.com/corpnewt/SSDTTime), [**MMIODevirt**](https://github.com/corpnewt/MmioDevirt), [**ProperTree**](https://github.com/corpnewt/ProperTree) and [**MountEFI**](https://github.com/corpnewt/MountEFI)
- benbaker76 for [**Hackintool**](https://github.com/benbaker76/Hackintool) (and all users who contributed framebuffer patches for it because it was a pita to get the Display Ports working)
- Unknown for [**USBToolBox**](https://github.com/USBToolBox/tool)
- Brumbaer for `CFGLock.efi`

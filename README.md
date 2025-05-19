# Fujitsu Esprimo Q958 Mini Hackintosh OpenCore

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.5-cyan.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) ![MacOS](https://img.shields.io/badge/macOS-14.7.5_to_15.5-purple.svg) [![release](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases)

<img src="https://github.com/user-attachments/assets/3fc086f2-1be3-4a90-83f9-efdc702d51d2" width="100%%" />

---
**TABLE of CONTENTS**

- [About](#about)
- [System Specs](#system-specs)
- [What works?](#what-works)
	- [Notable Features](#notable-features)
- [Issues](#issues)
- [BIOS Settings](#bios-settings)
- [Deployment](#deployment)
- [Post-Install](#post-install)
	- [Applying root patches with OCLP](#applying-root-patches-with-oclp)
		- [1. Download OCLP](#1-download-oclp)
		- [2. Apply root patches with OCLP (macOS Sequoia only)](#2-apply-root-patches-with-oclp-macos-sequoia-only)
		- [3. Reboot and enjoy!](#3-reboot-and-enjoy)
	- [Disable Gatekeeper (optional)](#disable-gatekeeper-optional)
	- [Fixing Sleep issues](#fixing-sleep-issues)
	- [Enable brightness control for external displays](#enable-brightness-control-for-external-displays)
	- [Disable CFG Lock (optional)](#disable-cfg-lock-optional)
- [Geekbench 5 Results](#geekbench-5-results)
- [Credits](#credits)

---

## About
OpenCore EFI folder for running macOS Sonoma or newer on the Fujitsu Esprimo Q958 Small Form Factor PC. As far as I am aware this is the first OpenCore EFI for this system.

## System Specs

**Component**   | Description 
----------------|:--------------------------------|
**Model**       | [Fujitsu Esprimo Q958](https://www.fujitsu.com/id/products/computing/pc/ap/desktops/esprimo-q958/)
**Formfactor**  | Small Form Factor (Mini-PC)
**Chipset**     | [Intel Q370](https://www.intel.com/content/www/us/en/products/sku/133282/intel-q370-chipset/specifications.html)
**CPU**         | 9th Gen Intel® Core™ [i7-9700T](https://www.intel.com/content/www/us/en/products/sku/191048/intel-core-i79700t-processor-12m-cache-up-to-4-30-ghz/specifications.html) (Coffee Lake) 
**GPU**         | Intel UHD Graphics 630 
**RAM**         | 32 GB DDR4 (2666 MHz)
**Storage**     | WD Blue SN550 NVMe SSD       
**NIC**         | Intel® I219-LM (1 Gbit)
**WiFi/BT**     | [Intel® Wireless-AC 9260](https://www.intel.de/content/www/de/de/products/sku/99445/intel-wirelessac-9260/specifications.html) 160MHz <ul> <li> WiFi Firmware: `iwm-9260-46` <li> BT Formware: `ibt-18-16-1.sfi`, `ibt-18-16-1.ddc`
**Sound**       | Realtek ALC671 (Layout-ID 16)
BIOS Revision   | R1.38.0 (08/29/22024)
**SMBIOS**      | `iMac19,1` 

## What works?

- [x] OS:
  - [x] macOS Sonoma+
  - [x] Windows 
- [x] iGPU
- [x] SATA drive
- [x] NVME drive
- [x] USB Portmapping (USB 2, USB 3.1, USB C, Bluetooth )
- [x] Ethernet
- [x] Audio (Line-out, Headphone, Mic, Integrated Speaker)
- [x] WLAN
- [x] Bluetooth

> [!IMPORTANT]
> 
> This EFI uses `AirportItlwm.kext` for WLAN. It supports Handoff, Universal Clipboard, Location Services and Instant Hotspot support but iServices won't work unless root patches are applied in Post-Install with OpenCore Legacy Patcher (OCLP)

### Notable Features

- [x] All 3 Graphics ports working (`con0` = DVI, `con1` = DP1 (bottom), `con3` = DP2 (Top))
- [x] 3D Globe in Apple Maps
- [x] Optimized EFI folder size by using slimmed kexts (11 MB in total instead of the default 54 MB)
- [x] Added MMIO Whitelist entries

## Issues
- [ ] Another Mini-PC with a Black-Screen-on-Wake issue. Needs investigation. Workaround: Disable Display sleep.
- [x] When trying to boot Windows 10 via OpenCore, I got Kernel Panics &rarr; Switched to [OC No ACPI Mod](https://github.com/wjz304/OpenCore_NO_ACPI_Build) to workaround the issue.

## BIOS Settings

Start by loading "Optiimized Defaults" (under Save & Exit &rarr; "Restore Defaults") and then continue adjusting the following settings:

- **Advanced** [TAB]
	- Drive Configuration
		- OnBoard SATA Configuration
			- SATA Mode: AHCI 
- **CPU Configuration**
	- Active Processor Cores: All
	- Intel Virtualization Technology: Enable if you want to use virtualization in Microsoft Windows, otherwise disable it
	- VT-D: same
- **Trusted Computing**
	- TPM Support: Disabled
- **USB COnfiguration**
	- USB 3.1 Gen 2 Speed: Enabled
	- USB Port Security
		- USB Port Control: Enable all ports
		- USB Device Control: Enable all devices
- **System Management**
	- Fan Control: Auto or Enhanced (better pperdormance, more fan activity)   
- **Super IO Configuration**
	- Serial Port 1 Configuration
		- Serial Port: Disabled
- **AMT Configuration**
	- Intel ANT: Disabled
- **Network Stack Configuration**
	- Network Stack: Enabled
	- IPv4 PXE Support: Disabled      
	- IPv6 PXE Support: Disabled
- **Graphic Configuration**
	- Primary Display: Internal Graphics
	- Internal Graphic: Enabled
		- DVMT Shared Memory Size: 64 MB
		- DVM Total Graphics Memory Size: Max
- **Power** [TAB]
	- Power Failure Recovery: Disabled
	- USB/PS2 Power: Set as needed
	- USB Type-C Connector Power Limit: Set as needed
	- External Power Buttob Control: Enabled
	- LAN: Disabled
- **Boot** [TAB]
	- New UEFI OS Option Policy: Place Last
	- Boot Optiob Priorities:
		- Boot Option #1: OpenCore
		- Windows Boot Manager 

## Deployment
- Download the latest OC EFI folder from the [Releases](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases) section
- Extract it
- Download, install and run [OpenCore Auxiliary Tools](https://github.com/ic005k/OCAuxiliaryTools)
- From the menubar, select Edit > OpenCore DEV:<br>![Bildschirmfoto 2025-05-06 um 06 03 58](https://github.com/user-attachments/assets/92354797-b3b9-43d0-8e38-05acda93c76d)
- Next, Open the Sync window and change the source for OpenCore to "OpenCore_No_ACPI_Build" and click on "Get OpenCore" :<br> ![Bildschirmfoto 2025-05-06 um 06 04 18](https://github.com/user-attachments/assets/3fac6505-6866-4e84-a728-359906ea62e6)
- Mount your EFI Partition and open the `confi.plist`
- In the `PlatformInfo/Generic` section, select `iMac19,1` and click on "Generate"
- Save the `config.plist` 
- Copy the EFI folder to a FAT32 formatted USB flash drive
- Boot macOS from the USB flash drive via the BIOS Boot Menu (F12)
- If the folder works then copy it to your internal disk and adjust the boot order in BIOS
- Enjoy

## Post-Install

### Applying root patches with OCLP (macOS Sequoia only)
In order for Wi-Fi to work in macOS Sequoia, you have to apply root patches with OpenCore Legacy Patcher.

#### 1. Download OCLP
- [Download the latest release of OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) _before_ booting into macOS Sequoia

#### 2. Apply root patches with OCLP (macOS Sequoia only)

- Run OCLP
- Click on "Apply Root Patch" button
- "Networking: Modern WiFi" should be available:<br>![intel_spoof05](https://github.com/user-attachments/assets/8b072d05-93f5-4151-b6e1-1d8e0c6c555e)
- Click "Start Root Patching"
- It will install the necessary Frameworks required for `AirportItlwm` to work:<br> ![intel_spoof06](https://github.com/user-attachments/assets/ced653f7-0807-4aef-82cb-eabf35b08884)

#### 3. Reboot and enjoy!

- Reboot the system
- Perform an NVRAM reset
- Boot into macOS
- WiFi should work now

> [!IMPORTANT]
> 
> Once root patches are applied, the security seal of the volume will be broken. And once it is broken, the complete macOS version will be downloaded every time an OS update is available. The workaround would be to revert root patches before installing updates and then use LAN to to download and install incremental updates.

### Disable Gatekeeper (optional)
I disable Gatekeeper on my systems because it is annoying and wants to stop you from running scripts from github etc. To do so, enter `sudo spctl --master-disable` in Terminal. Disabling Gatekeeper in macOS Sequoia requires a few more [steps](https://github.com/5T33Z0/OC-Little-Translated/blob/main/14_OCLP_Wintel/Guides/Disable_Gatekeeper.md).

### Fixing Sleep issues

In order to prevent the most common problems with sleep, we will set it to `hibernatemode 0` (Suspend to RAM). This is not relevant for now, since the blackscreen-on-wake issue is not resolved.

Open Terminal and enter:

```bash
sudo pmset -a hibernatemode 0
```

If you have issues with sleep, run the following commands in Terminal:

```bash
sudo pmset hibernatemode 0
sudo rm /var/vm/sleepimage
sudo touch /var/vm/sleepimage
sudo chflags uchg /var/vm/sleepimage
```

For more info follow my my guide to [configure hibernation](https://github.com/5T33Z0/OC-Little-Translated/tree/main/04_Fixing_Sleep_and_Wake_Issues/Changing_Hibernation_Modes#enabling-hibernation-on-hackintosh-systems)

> [!NOTE]
>
> This is irrelevant for now, since the blackscreen-on-wake issue is not resolved yet.

### Enable brightness control for external displays

There's a new app called [**MonitorControl**](https://github.com/MonitorControl/MonitorControl) which allows controlling the brightness and contrast of attached external displays right from the menu bar.

### Disable CFG Lock (optional)
- From Bootmenu, select `CFGLock.efi` and press <kbd>Enter</kbd> to run it
- Follow the on-screen Instrutions to disable CFGLock (set it to `0`)
- Quit the tool
- Boot macOS
- Mount EFI partition and open your `config.plist`
- Disable Kernel Quirk `AppleXcpmCfgLock`
- Save your config an reboot

## Geekbench 5 Results

### CPU

![gb5_sonoma](https://github.com/user-attachments/assets/88c6bd9b-4439-4b85-98e2-81dcdf675da9)

**Full result**: [https://browser.geekbench.com/v5/cpu/23512366](https://browser.geekbench.com/v5/cpu/23512366)

### iGPU

Metal | OpenCL | RPS Control
:----:|:------:|:------------:
[**4941**](https://browser.geekbench.com/v5/compute/6868138)| [**5341**](https://browser.geekbench.com/v5/compute/6868141) | **ON**
[4776](https://browser.geekbench.com/v5/compute/6868144) | [4995](https://browser.geekbench.com/v5/compute/6868143) | OFF

## Credits
- Acidanthera for the [**OpenCore**](https://github.com/acidanthera/OpenCorePkg) Bootloader
- Dortania for [**OpenCore Legacy Patcher**](https://github.com/dortania/OpenCore-Legacy-Patcher)
- lzhoang2801 for [**OpCore Simplify**](https://github.com/lzhoang2801/OpCore-Simplify)
- ic005k for [**OC Auxiliary Tools**](https://github.com/ic005k/OCAuxiliaryTools)
- CorpNewt for [**SSDTTime**](https://github.com/corpnewt/SSDTTime), [**MMIODevirt**](https://github.com/corpnewt/MmioDevirt), [**ProperTree**](https://github.com/corpnewt/ProperTree) and [**MountEFI**](https://github.com/corpnewt/MountEFI)
- benbaker76 for [**Hackintool**](https://github.com/benbaker76/Hackintool) (and all users who contributed framebuffer patches for it because it was a pita to get the Display Ports working)
- Unknown for [**USBToolBox**](https://github.com/USBToolBox/tool)
- Brumbaer for `CFGLock.efi`

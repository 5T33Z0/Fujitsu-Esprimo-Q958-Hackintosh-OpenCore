# Fujitsu Esprimo Q958 Mini Hackintosh OpenCore

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.6-cyan.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) ![MacOS](https://img.shields.io/badge/macOS-14.7.5_to_26_beta_7-purple.svg) [![release](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases)

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
	- [Disable Gatekeeper (optional)](#disable-gatekeeper-optional)
	- [Fixing On-Board Audio with OCLP Mod (macOS Tahoe beta 2+)](#fixing-on-board-audio-with-oclp-mod-macos-tahoe-beta-2)
	- [Enable Intel WiFi (macOS Sequoia)](#enable-intel-wifi-macos-sequoia)
	- [Enabling audio in macOS Tahoe](#enabling-audio-in-macos-tahoe)
	- [Strengthen Security (optional)](#strengthen-security-optional)
	- [Fixing Sleep issues](#fixing-sleep-issues)
	- [Enable brightness control for external displays](#enable-brightness-control-for-external-displays)
	- [Disable CFG Lock (optional)](#disable-cfg-lock-optional)
	- [Add Eject button for optical drive to Menu Bar](#add-eject-button-for-optical-drive-to-menu-bar)
- [Geekbench 5 Results](#geekbench-5-results)
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
> This EFI uses `AirportItlwm.kext` for WLAN. It supports Handoff, Universal Clipboard, Location Services and Instant Hotspot support but iServices won't work unless root patches are applied in Post-Install with OpenCore Legacy Patcher (OCLP). In macOS Tahoe it uses `itlwm.kext` since no root patches exist yet.

### Notable Features

- [x] All 3 Graphics ports are working (`con0` = DVI, `con1` = DP1 (bottom), `con3` = DP2 (Top))
- [x] 3D Globe in Apple Maps.
- [x] Optimized EFI folder size by using slimmed kexts (11 MB in total instead of the default 54 MB)
- [x] Added MMIO Whitelist entries.

## Issues
- [ ] Another Small Form Factor PC with a Black-Screen-on-Wake issue. No known fixes yet. **Workaround**: Disable `displaysleep`, so the system simply won't enter sleep.

## BIOS Settings

<details>
<summary><b>Click to show BIOS settings</b></summary><br>

Begin by loading "Optimized Defaults" (under Save & Exit &rarr; "Restore Defaults"). Next, adjust the following settings for booting macOS:

- **Advanced** [TAB]
	- **CPU Configuration**
		- **Package C State Limit**: 
			- [**CPU Default**] = Causes the fan to ramp up much later at around at around 60° C. Use for a a lot more silent operation.
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

## Deployment
- Download the latest OC EFI folder from the [Releases](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases) section
- Extract it
- Open the `config.plist`, add Serials, MLB etc. with [OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases) (in Dev Mode) or [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and save it
- Copy the EFI folder to a FAT32 formatted USB flash drive
- Boot macOS from the USB flash drive via the BIOS Boot Menu (F12)
- If the folder works then copy it to your internal disk and adjust the boot order in BIOS
- Enjoy

> [!NOTE]
>
> If you are using macOS Sequoia and older, disable/delete the `unfairvgva7` boot-arg if you want to use AppleTV+ or Apple Music.

## Post-Install

### Disable Gatekeeper (optional)
I disable Gatekeeper on my systems by default, since it blocks running scripts from github etc. To do so, enter `sudo spctl --master-disable` in Terminal. Disabling Gatekeeper in macOS Sequoia requires [additional steps](https://github.com/5T33Z0/OCLP4Hackintosh/tree/main/Guides/Disable_Gatekeeper.md).

### Fixing On-Board Audio with OCLP Mod (macOS Tahoe beta 2+)
Apple deleted the `AppleHDA` component required for using analog on-board audio since macOS 26 beta 2. Since there's no official OCLP version available for macOS Tahoe yet, we are going to use [**OCLP Mod**](https://github.com/laobamac/OCLP-Mod/) to apply root patches – which will also install AppleHDA, thereby re-enabling audio. (&rarr; [Instructions](https://github.com/5T33Z0/OCLP4Hackintosh/blob/main/Enable_Features/Audio_Tahoe.md#instructions))

### Enable Intel WiFi (macOS Sequoia)
In order for Wi-Fi to work in macOS Sequoia, you have to apply root patches with OpenCore Legacy Patcher.

- [Download the latest release of OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) _before_ booting into macOS Sequoia
- Run OCLP
- Click on "Apply Root Patch" button
- "Networking: Modern WiFi" should be available:<br>![intel_spoof05](https://github.com/user-attachments/assets/8b072d05-93f5-4151-b6e1-1d8e0c6c555e)
- Click "Start Root Patching"
- It will install the necessary Frameworks required for `AirportItlwm` to work:<br> ![intel_spoof06](https://github.com/user-attachments/assets/ced653f7-0807-4aef-82cb-eabf35b08884)
- Reboot the system
- Reset NVRAM
- Boot into macOS
- WiFi should work now

> [!IMPORTANT]
> 
> Once root patches are applied, the security seal of the system volume will be broken. And once it is broken, the complete macOS version will be downloaded every time an OS update is available. The workaround would be to revert root patches *before* installing updates and then use LAN to to download and install incremental updates. But there's a chance that applying incremental updates will fail. In this case, the full installer will be downloaded on the next attempt.

### Enabling audio in macOS Tahoe

`AppleHDA` was removed from macOS 26 beta 2 onward, so analog on-board audio does not work out of the box &rarr; [Fix with OCLP Mod](https://github.com/5T33Z0/OCLP4Hackintosh/blob/main/Enable_Features/Audio_Tahoe.md).

### Enable Intel WiFi (macOS Tahoe)
- Since AirportItlwm no longer works in macOS Tahoe, Itlwm is used instead
- So you need to use the [Heliport App](https://github.com/OpenIntelWireless/HeliPort) to connect to WiFi Access Points

###  Strengthen Security (optional)
In config.plist, navigate to Misc/Security/SecureBootModel and change it to: `Default`. But you may have to disable it when updating macOS, otherwise the installer might crash. Ever since the release of macOS Sonoma 14.4 it does not longer work correctly during install.

### Fixing Sleep issues

In order to prevent the most common issues with sleep, we will set it to `hibernatemode 0` (Suspend to RAM), write protect the sleep image using Terminal:

```bash
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo touch /var/vm/sleepimage
sudo chflags uchg /var/vm/sleepimage
```

Next, we disable `displaysleep` and `powernap` to workaround the black-screen-on-wake issue. And since this Mini-PC does not have a motion sensor, we also disable proximitywake: 

```bash 
sudo pmset displaysleep 0
sudo pmset powernap 0
sudo pmset proximitywake 0
```

For more info follow my my guide to [configure hibernation](https://github.com/5T33Z0/OC-Little-Translated/tree/main/04_Fixing_Sleep_and_Wake_Issues/Changing_Hibernation_Modes#enabling-hibernation-on-hackintosh-systems)

> [!NOTE]
>
> This is irrelevant for now, since the blackscreen-on-wake issue is not resolved yet.

### Enable brightness control for external displays

There's a new app called [**MonitorControl**](https://github.com/MonitorControl/MonitorControl) which allows controlling the brightness and contrast of attached external displays right from the menu bar and/or keyboard.

### Disable CFG Lock (optional)
- From Bootmenu, select `CFGLock.efi` and press <kbd>Enter</kbd> to run it
- Follow the on-screen Instrutions to disable CFGLock (set it to `0`)
- Quit the tool
- Boot macOS
- Mount EFI partition and open your `config.plist`
- Disable Kernel Quirk `AppleXcpmCfgLock`
- Save your config an reboot

> [!NOTE]
>
> Disabling CFG Lock in this ways is not permanent. Once you reset the BIOS back to defaults, you have to run `CFGLock.efi` again to disable it!

### Add Eject button for optical drive to Menu Bar
macOS locks the optical drive sometimes so that you can't open it by pressing the physical eject button – even if no media is present. If you have an Apple Keyboard, you can simply press the <kbd>Eject</kbd> key to open the drive. 

But if you don't, you can add an Eject button to the menu bar: 

- Open Finder and navigate to: `System/Library/CoreServices/Menu Extras`
- Double-click on `Eject.menu`. This adds the Eject button icon to the Menu Bar: <br>![Eject](https://github.com/user-attachments/assets/9ed29614-d4e8-48f6-a2a1-93b48a6d36da)

## Geekbench 5 Results

### CPU

![gb5_sonoma](https://github.com/user-attachments/assets/88c6bd9b-4439-4b85-98e2-81dcdf675da9)

**Full result**: [https://browser.geekbench.com/v5/cpu/23512366](https://browser.geekbench.com/v5/cpu/23512366)

### iGPU

Metal | OpenCL | RPS Control
:----:|:------:|:------------:
[**4941**](https://browser.geekbench.com/v5/compute/6868138)| [**5341**](https://browser.geekbench.com/v5/compute/6868141) | **ON**
[4776](https://browser.geekbench.com/v5/compute/6868144) | [4995](https://browser.geekbench.com/v5/compute/6868143) | OFF

I also tested force-loading Apple's GUC firmware ([4858 points in the Metal test](https://browser.geekbench.com/v5/compute/6872229/claim?key=253757)), but that's no usable option for me since it causes Firefox Tabs to crash. It has no impact on Safari, though.

> [!TIP]
>
> - If iGPU performance is sufficient for your needs and you want a nearly silent operation of the system, disable the `rps-control` property by changing it to `00000000`
> - If you need higher iGPU performance, enable `rps-control` by changing the value to `01000000`. But this will increase temps which results in more fan activity.

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
	AdvancedMap.kext | https://github.com/narcyzzo/AdvancedMap
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

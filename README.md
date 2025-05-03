:construction: Work in Progress

# Fujitsu Esprimo Q958 Mini Hackintosh OpenCore

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.5-cyan.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) ![MacOS](https://img.shields.io/badge/macOS-14.7.5-purple.svg) [![release](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases)

![Fujitsu_Esprimo_Q958_front](https://github.com/user-attachments/assets/3fc086f2-1be3-4a90-83f9-efdc702d51d2)

## About
OpenCore EFI folder for running macOS Sonoma or newer on the Fujits Esprimo Q958 Small Form Factor PC. As far as I am aware this is the first OpenCore EFI for the Esprimo Q958.

## System Specs

**Compenent**   | Description 
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
**SMBIOS**      | `iMac19,1` 

## What works?

### Supported macOS

- [x] macOS Sonoma. Might work with Sequoia with some minor config adjustments.

### Hardware

- [x] iGPU (2 Display Ports)
- [x] SATA drive
- [x] USB Portmapping (USB 2, USB 3.1, USB C). May need further refinement.
- [x] Ethernet
- [x] Audio (Line-out, Headphone out, Mic, integrated speaker)
- [x] WLAN
- [x] Bluetooth

> [!IMPORTANT]
> 
> This EFI uses `AirportItlwm.kext` for WLAN. It supports Handoff, Universal Clipboard, Location Services and Instant Hotspot support but iServices won't work unless root patches are applied in Post-Install with OpenCore Legacy Patcher (OCLP)

## Issues
- [ ] Another Mini-PC with a Black-Screen-on-Wake issue. Needs invesigation. Workaround: Disable Display sleep.
- [x] ~~When trying to boot Windows 10 via OpenCore I get a Kernel Panic. Further Investigation required~~ &rarr; Switched to OC No ACPI Mod.

## BIOS Settings

todo…

## Deployment
- Download the latest OC EFI folder from the [Releases](https://github.com/5T33Z0/Fujitsu-Esprimo-Q958-Hackintosh-OpenCore/releases) section
- Extract it
- Open the `config.plist` with [OCAT](https://github.com/ic005k/OCAuxiliaryTools)
- In the `PlatformInfo/Generic` section, press the "Generate" button next to the "System Product Name" dropdown menu te generate serial numbers, etc.
- Copy EFI to a FAT32 formatted USB flash drive
- Boot macOS from the USB flash drive via the BIOS Boot Menu (F12)
- If the folder works then copy it to your internal disk and adjust the boot order in BIOS
- Enjoy

## Post-Install

todo…

### Disable CFG Lock (optional)
- From Bootmenu, select `CFGLock.efi` and press <kbd>Enter</kbd> to run it
- Follow the on-screen Instrutions to disable CFGLock (set it to `0`)
- Quit the tool
- Boot macOS
- Mount EFI partition and open your `config.plist`
- Disable Kernel Quirk `AppleXcpmCfgLock`
- Save your config an reboot

## Geeekbench 5 Results

![gb5_sonoma](https://github.com/user-attachments/assets/88c6bd9b-4439-4b85-98e2-81dcdf675da9)

**Full reseult**: [https://browser.geekbench.com/v5/cpu/23512366](https://browser.geekbench.com/v5/cpu/23512366)

## Credits
- Acidanthera for the [**OpenCore**](https://github.com/acidanthera/OpenCorePkg) Bootloader
- Dortania for [**OpenCore Legacy Patcher**](https://github.com/dortania/OpenCore-Legacy-Patcher)
- lzhoang2801 for [**OpCore Simplify**](https://github.com/lzhoang2801/OpCore-Simplify)
- CorpNewt for [**SSDTTime**](https://github.com/corpnewt/SSDTTime), [**MMIODevirt**](https://github.com/corpnewt/MmioDevirt), [**ProperTree**](https://github.com/corpnewt/ProperTree) and [**MountEFI**](https://github.com/corpnewt/MountEFI)
- benbaker76 for [**Hackintool**](https://github.com/benbaker76/Hackintool) (and all users who contributed framebuffer patches for it because it was a pita to get the Display Ports working)
- Unknown for [**USBToolBox**](https://github.com/USBToolBox/tool)
- Brumbaer for `CFGLock.efi`

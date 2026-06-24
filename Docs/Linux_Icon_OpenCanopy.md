# Making OpenCanopy Display the Correct Icon for Your Linux Installation

## About

By default, OpenCore displays a generic hard drive icon for Linux installations in the Boot Picker:

<img width="794" height="343" alt="Generic_Icon" src="https://github.com/user-attachments/assets/1fcfcabc-068b-4a9c-93c3-e2f0f6e26528" />

This can be customized so that OpenCore displays a distribution-specific icon instead. In this example, Fedora is used:

<img width="737" height="305" alt="OC_Bootpicker_00" src="https://github.com/user-attachments/assets/43d00ce8-4549-4743-afb4-6ba7aa8ce1a2" />

## Instructions

* Mount the EFI System Partition (ESP) using OCAT.
* Navigate to `EFI/OC`.
* Press <kbd>CMD</kbd> + <kbd>SHIFT</kbd> + <kbd>.</kbd> to show hidden files.
* Copy an existing `.contentFlavour` file from another boot entry, or create a new file named `.contentFlavour`.
* Paste the `.contentFlavour` file into the directory containing your Linux distribution's bootloader files. In this example, this is the `fedora` directory:<br> <img width="914" height="332" alt="Finder_01" src="https://github.com/user-attachments/assets/59ebf433-d9e1-4db3-bba0-201cc698dcf8" />
* Right-click the `.contentFlavour` file and open it with TextEdit.
* Enter the icon name followed by the operating system type. For Fedora, the file should contain:<br><img width="662" height="103" alt="contentflavor" src="https://github.com/user-attachments/assets/3b6cc3f6-f090-4b01-a749-64f1d57f7251" />
* Save the file.
* Reboot and verify that the correct icon is displayed.

### Format

The `.contentFlavour` file uses the following format:

```text
<IconName>:<OSType>
```

**Examples**:

```text
Fedora:Linux
Ubuntu:Linux
Arch:Linux
```

> [!NOTE]
> 
> The `.contentFlavour` file must be located in the same directory as the EFI bootloader files for the distribution.

## Supported Linux Icons

Since my `config.plist` is already configured to use content flavour files, you only need to ensure that the desired icon exists in:

```text
/EFI/OC/Resources/Image/HJebbour/GoldenGateExt
```

and that its filename is referenced correctly in `.contentFlavour`.

Currently, GoldenGateExt includes icons for approximately 30 Linux distributions and Linux-based operating systems:

* Arch Linux (`Arch.icns`)
* CachyOS (`CachyOS.icns`)
* CentOS (`CentOS.icns`)
* Debian (`Debian.icns`)
* Deepin (`Deepin.icns`)
* elementary OS (`elementaryOS.icns`)
* EndeavourOS (`EndeavourOS.icns`)
* Endless OS (`Endless.icns`)
* Fedora (`Fedora.icns`)
* Generic Linux (`Linux.icns`)
* Generic External Linux (`ExtLinux.icns`)
* Gentoo (`Gentoo.icns`)
* Kali Linux (`Kali.icns`)
* KDE neon (`KDEneon.icns`)
* Kubuntu (`Kubuntu.icns`)
* Lubuntu (`Lubuntu.icns`)
* Mageia (`Mageia.icns`)
* Manjaro (`Manjaro.icns`)
* Linux Mint (`Mint.icns`)
* MX Linux (`MX.icns`)
* openSUSE (`openSUSE.icns`)
* Pop!_OS (`PopOS.icns`)
* Rocky Linux (`Rocky.icns`)
* Red Hat Enterprise Linux (`RHEL.icns`)
* Solus (`Solus.icns`)
* Ubuntu (`Ubuntu.icns`)
* Ubuntu Cinnamon (`UbuntuCinnamon.icns`)
* Ubuntu DDE (`UbuntuDDE.icns`)
* Ubuntu Kylin (`UbuntuKylin.icns`)
* Ubuntu MATE (`UbuntuMATE.icns`)
* Void Linux (`Void.icns`)
* Xubuntu (`Xubuntu.icns`)
* Zorin OS (`Zorin.icns`)

## Missing Icons

If an icon for your distribution is not available, you can either create a custom icon or use the icon of a closely related distribution. For example, Aurora OS is Fedora-based, so the Fedora icon works well as a substitute.

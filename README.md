# Dell Vostro 3888 Hackintosh Guide
This guide wil guide you for making your own EFI folder for the Dell Vostro 3888.
 
 --------------------------------------------------------------------------------------------
### Desktop Specs
<img src="https://microless.com/cdn/product_description/6437630_1613377018.jpg" alt="Dell Vostro 3888" align="right">

- [x] <b>Model</b>: Dell Vostro 3888
- [x] <b>CPU</b>: 10th Gen Intel® Core™ i5-10400 (6-Core, 12MB Cache, 2.9GHz to 4.3GHz)
- [x] <b>Chipset</b>: Intel® B460
- [x] <b>GPU</b>: Integerated Intel UHD 630
- [x] <b>RAM</b>: 8GB DDR4 @ 2666MHz (upgradable to 64GB)
- [x] <b>HDD</b>: 1TB SATA @ 7200 rpm (GUID Partition Table)
- [x] <b>Audio</b>: Realtek ALC3246/256
- [x] <b>Wifi</b>: Intel(R) Wireless-AC 3165
- [x] <b>Ethernet</b>: Realtek RTL8111 Gigabit Ethernet
 
# What Works:
Wifi
Bluetooh
Ethernet
CD-ROM
All USB Ports
Front Audio Jack **[Microphone not tested yet]**
Back Audio Jack
HDMI
Sleep

# What Doesn't Work:
> Need to Recheck

# Not Tested:
VGA Port
SD Card Reader

-------------------------------------------------------
**How to make your own EFI?**
First of all, here is the list of the kexts & tools that are needed:
 - **Kext:**
>[Lilu](https://github.com/acidanthera/lilu/releases)
[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) [GPU Patches]
[VirtualSMC](https://github.com/acidanthera/virtualsmc/releases)
[AppleALC](https://github.com/acidanthera/AppleALC) [for audio]
[Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) [Wifi Card]
[IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) [Bluetooh Card]
[RealtekRTL8111](https://github.com/acidanthera/OpenCorePkg/releases) [Ethernet Port]

- **Tools:**
>[Latest OpenCore Release](https://github.com/acidanthera/OpenCorePkg/releases/) [Bootloader]
>[macrecovery.py](https://github.com/acidanthera/OpenCorePkg/releases) [Creating macOS installer]
>[SSDTTime](https://github.com/corpnewt/SSDTTime) [Creating SSDTs]
>[ProperTree](https://github.com/corpnewt/ProperTree) [Universal plist editor]
>[GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) [Generating SMBIOS data]
>[Sample/config.plist](https://github.com/acidanthera/OpenCorePkg/releases) [Sample config.plist]
--------------------------------------------------------------------------------------------
### Creating the installer
You may follow the OpenCore Dortania guide for making the installer on:
[Windows](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)
[macOS](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#downloading-macos-modern-os)
[Linux](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html)

-----
### Adding Files
After creating your installer, we will work now on creating your own EFI folder.
First, download & extract the OpenCore-0.6.8-RELEASE.zip. Access the X64 folder, then copy the EFI folder to your USB EFI partition.

Now, you'll notice that it comes with a bunch of files in the Drivers and Tools folders, most of which we don't want:

-   **Keep the following from Drivers**(if applicable):

|Driver | Status | Description |
|--|--|--|
| OpenRuntime.efi | Required | Required for proper operation |
| OpenHfsPlus.efi | Required | Needed for seeing HFS volumes(ie. macOS Installers and Recovery partitions/images). |
|OpenUsbKbDxe.efi  | Optional | Required for non-UEFI systems(pre-2012) |
|OpenPartitionDxe.efi | Optional | Required to boot macOS 10.7-10.9 recovery |

-   **Keep the following from Tools:**

|Tool | Status | Description |
|--|--|--|
|OpenShell.efi  | Optional | Recommended for easier debugging |

---
We may now start adding our kexts & drivers.
Download all the kexts linked above. Here is the list of kexts which we need
|Kext  | File |
|--|--|
|Lilu  | Lilu.kext |
|WhateverGreen  | WhateverGreen.kext |
|VirtualSMC | VirtualSMC.kext, SMCSuperIO.kext, SMCProcessor.kext & SMCDellSesnors.kext |
|AppleALC  | AppleALC.kext |
|itlwm  | airportitlwm.kext [Big Sur Version]|
|IntelBluetoothFirmware| IntelBluetoothFirmware.kext & IntelBluetoothInjector.kext|
|Realtek RTL8111|RealtekRTL8111.kext|
This is what your Kexts folder might look like:
![Kext Folder](https://github.com/HassanElDessouki/DV3888-Hackintosh/blob/main/pics/T92RmjoODm.jpg?raw=true)

## SSDTs
We will need to create the following SSDT files:
> SSDT-PLUG
> 
> SSDT-EC-USBX
> 
> SSDT-AWAC
> 
> SSDT-RHUB
> 

We use the SSDTTime tool to create these SSDT files. Run the SSDTTime.bat as Administrater. You will see the following screen:![SSDTTime Example](https://github.com/HassanElDessouki/DV3888-Hackintosh/blob/main/pics/3Pyp4GrAVt.jpg?raw=true)
First we need to run the 8th selection [Dump DSDT], then we will need to run the FixHPET, FakeEC, PluginType, AWAC & finally USB-Reset.
Your SSDTTime/results folder might look something similar to this:
![enter image description here](https://github.com/HassanElDessouki/DV3888-Hackintosh/blob/main/pics/VnUkiabvup.jpg?raw=true)


Copy only the following files to the EFI/OC/ACPI.
>SSDT-AWAC.aml
>
>SSDT-EC.aml
>
>SSDT-HPET.aml
>
>SSDT-PLUG.aml
>
>SSDT-USB-Reset.aml
>

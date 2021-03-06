---
title: BitLocker drive encryption in Windows 10 for OEMs
description: OEMs can configure hardware to support Windows 10 automatic device encryption.
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
---

# BitLocker drive encryption in Windows 10 for OEMs

BitLocker drive encryption provides offline data and operating system protection by ensuring that the drive is not tampered with the operating system is offline. BitLocker drive encryption uses a TPM, either discrete or firmware, that supports the Static Root of Trust Measurement as defined by the [Trusted Computing Group](https://trustedcomputinggroup.org/). 

## BitLocker drive encryption hardware requirements
BitLocker drive encryption uses a system partition separate from the Windows partition. The BitLocker system partition must meet the following requirements.
- The BitLocker system partition is configured as the active partition.
- The BitLocker system partition must not be encrypted.
- The BitLocker system partition must have at least 250 MB of free space, above and beyond any space used by required files. This additional system partition can be used to host Windows Recovery Environment (RE) and OEM tools (provided by the OEM), so long as the partition still meets the 250 MB free space requirement. 

For more information see [System.Client.SystemPartition](https://docs.microsoft.com/en-us/windows-hardware/design/compatibility/systems#systemclientsystempartition), and [Hard Drives and Partitions](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/hard-drives-and-partitions).

## BitLocker automatic device encryption
BitLocker automatic device encryption uses BitLocker drive encryption technology to automatically encrypt internal drives after the user completes the Out Of Box Experience (OOBE) on InstantGo or HSIT compliant hardware.  

**Note:** BitLocker automatic device encryption is enabled only after users sign in with a **Microsoft Account** or an **Azure Active Directory** account. BitLocker automatic device encryption is not enabled with local accounts, in which case BitLocker can be manually using the BitLocker Control Panel.

## BitLocker automatic device encryption hardware requirements
BitLocker automatic device encryption is enabled when:
- The device contains a **TPM** (Trusted Platform Module), either TPM 1.2 or TPM 2.0. 
- **UEFI Secure Boot** is enabled. See [Secure boot](OEM-secure-boot.md) for more information.
- **Platform Secure Boot** is enabled
- **Direct memory access (DMA)** protections is enabled

The following tests must pass before Windows 10 will enable Automatic BitLocker device encryption. If you want to create hardware that supports this capability, you must verify that your device passes these tests. 

1. **TPM**: Device must include a TPM with PCR 7 support.  See [System.Fundamentals.TPM20.TPM20](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#systemfundamentalstpm20tpm20).
2. **Secure boot**: UEFI Secure Boot is enabled. See [System.Fundamentals.Firmware.UEFISecureBoot](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#systemfundamentalsfirmwareuefisecureboot).
3. **InstantGo (AOAC)** requirements or **HSTI** validation. This requirement is met by one of the following:
    - InstantGo (AOAC) requirements are implemented. These include [requirements for UEFI secure boot](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#systemfundamentalsfirmwarecsuefisecurebootconnectedstandby) and protection from unauthorized DMA.
    - Starting with Windows 10, version, 1703, this requirement can be met through [HSTI test](https://msdn.microsoft.com/library/windows/hardware/mt712332.aspx):
        1.	Platform Secure Boot self-test (or additional self-tests as configured in the registry) must be reported by HSTI as implemented and passed.
        2.	Excluding Thunderbolt, HSTI must report no non-allowed DMA busses.
        3.	If Thunderbolt is present, HSTI must report that Thunderbolt is configured securely (security level must be SL1 – “User Authorization” or higher).
4. You must have 250MB of free space on top of everything you need to boot (and recover Windows, if you put WinRE on the system partition). For more information, see [System.Client.SystemPartition](https://docs.microsoft.com/en-us/windows-hardware/design/compatibility/systems#systemclientsystempartition).

When the requirements as listed above are met, System Information indicates the system supports BitLocker automatic device encryption. This functionality is available in Windows 10, version 1703 or after. Here's how to check System Information. 

1.	Click **Start**, and type **System information**
2.	Right-click **System Information** app and click **Open as Administrator**. Allow the app to make changes to your device by clicking **Yes**. Some devices might require elevated permissions to view the encryption settings.
3.	In **System Summary**, see **Device Encryption Support**.  The value will state if the device is encrypted, or if not, reasons why it is disabled. 

## Un-allowed DMA capable bus/device(s) detected 

This System Information status in Device Encryption Support means Windows detected at least one potential external DMA capable bus or device that may expose a DMA threat.
 
To resolve this issue, contact the IHV(s) to determine if this device has no external DMA ports. If confirmed by the IHVs that the bus or device only has internal DMA, then the OEM can add this to the allowed list.   
  
To add a bus or device to the allowed list, navigate to registry key: 

**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\DmaSecurity\\AllowedBuses** 
 
Add string (REG_SZ) name/value pairs for each flagged DMA capable bus that is determined to be safe: 

- Key: *device friendly name* /*description*  
- Value: PCI\VEN\_*ID*&DEV\_*ID*.  
 
Ensure the IDs match the output from the HLK test. For example, if you have a safe device with a friendly name of “Contoso PCI Express Root Port”, vendor ID 1022 and Device ID 157C, you would create the following Registry entry as REG_SZ data type:  

**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\DmaSecurity\\AllowedBuses!Contoso PCI Express Root Port**

Where the value = "PCI\\VEN\_1022&DEV\_157C" 

## Disable BitLocker automatic device encryption
OEMs can choose to disable device encryption and instead implement their own encryption technology on a device. To disable BitLocker automatic device encryption, you can use an Unattend file and set [PreventDeviceEncryption](https://msdn.microsoft.com/windows/hardware/commercialize/customize/desktop/unattend/microsoft-windows-securestartup-filterdriver-preventdeviceencryption) to True. 
Alternately, you can update this registry key:
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\BitLocker**
Value: **PreventDeviceEncryption** equal to True (1).

## <span id="related_topics"></span>Related topics

[Windows 10 S security features and requirements for OEMs](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-10s-security)
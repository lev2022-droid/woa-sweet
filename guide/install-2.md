<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 running on sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Installing Windows

### Prerequisites
- Completed [Partitioning your device](install-1.md)
- The phone's Windows (`winnt`) and ESP partitions visible from the PC
-  [UEFI (soon) ](https://)
-  [Drivers](https://github.com/lev2022-droid/woa-sweet/releases/tag/Drivers) 

### Notes
> [!WARNING]  
> `dism /apply-image` erases everything on the `winnt` partition. Make sure you pick the right disk/letters!
> 
> Double-check the image index before applying — you want Windows 11 **Pro**.
>
> Do NOT unplug the phone during apply. The process takes 10–30 minutes depending on USB speed.


##### Booting the UEFI with mass storage
```cmd
adb reboot bootloader
fastboot boot boot-sweet.img
```
> In the SimpleInit menu (USB mouse!), enable **USB Mass Storage**, then plug the phone into the PC.

##### Assigning drive letters
> Open an **elevated** CMD and run diskpart
```cmd
diskpart
list disk
select disk <phone disk number>
list partition
select partition <winnt>
assign letter=X
select partition <esp>
assign letter=Y
exit
```

##### Applying Windows
> Find your index first (you want Windows 11 Pro)
```cmd
dism /get-imageinfo /imagefile:<ISO>\sources\install.esd
```
```cmd
dism /apply-image /imagefile:<ISO>\sources\install.esd /index:6 /ApplyDir:X:\
bcdboot X:\Windows /s Y: /f UEFI
```

##### Installing all drivers (offline)
> One command — installs every package in `sweet_drivers_all\` and enables testsigning
```cmd
D:\path\to\sweet_drivers_all\INSTALL-ALL.cmd X Y
```
> `X` = the winnt letter, `Y` = the ESP letter. The script runs `dism /add-driver /recurse /ForceUnsigned` and `bcdedit /store Y:\EFI\Microsoft\Boot\BCD /set testsigning on` for you.

##### Rebooting into Windows
```cmd
shutdown /r /t 0     (on the phone, or just unplug and power on)
```
> First boot takes a few minutes (OOBE). **A USB mouse is required** — the touchscreen driver does not exist yet.

#### What works right after boot
- Tray battery icon appears immediately; the real percentage binds ~1 minute later
- Wi-Fi connects automatically (if not: `pnputil /add-driver WLAN-standalone\x3n_wlan.inf /install` in Windows)
- See [Project status](status.md) for the full checklist

## Dual boot

- **Windows → Android**: reboot with Volume Down held (bootloader), then
```cmd
fastboot flash boot boot_backup.img
fastboot reboot
```
- **Android → Windows**: install the [WOA Helper](https://github.com/n00b69/woa-helper) app (root) → "Quickboot to Windows", or every time `fastboot boot boot-sweet.img` from the PC.

If something went wrong — see [Troubleshooting](troubleshooting.md).

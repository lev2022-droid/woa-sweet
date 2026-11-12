<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 running on sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Installing Windows

### Prerequisites
- Completed [Partitioning your device](install-1.md)
- The phone's Windows (`winnt`) and ESP partitions visible from the PC
-  [UEFI]([https://](https://github.com/lev2022-droid/woa-sweet/releases/tag/UEFI))
-  [Drivers](https://github.com/lev2022-droid/woa-sweet/releases/tag/Drivers) 

### Notes
> [!WARNING]  
> `dism /apply-image` erases everything on the `winnt` partition. Make sure you pick the right disk/letters!
> 
> Double-check the image index before applying — you want Windows 11 **Pro**.
>
> Do NOT unplug the phone during apply. The process takes 10–30 minutes depending on USB speed.

> [!IMPORTANT]
> The second argument of `INSTALL-ALL.cmd` (the ESP letter) enables `testsigning` in the phone's BCD — the battery fuel-gauge driver needs it. Without it you get a tray icon but a frozen percentage.

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
```
```cmd
list disk
```
```cmd
select disk <phone disk number>
```
```cmd
list partition
```
```cmd
select partition <winnt>
```
```cmd
assign letter=X
```
```cmd
select partition <esp>
```
```cmd
assign letter=Y
```
```cmd
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
> One command — installs every package in `sweet_drivers` and enables testsigning
```cmd
dism /Image:letter phone:\ /Add-Driver /Driver:"the path to the drivers folder" /Recurse

```

### Recommended BCD tweaks

> Run these against the phone's **offline** BCD from the PC (ESP mounted as `Y:`), or from Windows on the phone by dropping the `/store Y:\EFI\Microsoft\Boot\BCD` part. Each command is separate on purpose — you decide which ones you need.

##### Test mode — allow test-signed drivers
```cmd
bcdedit /store Y:\EFI\Microsoft\Boot\BCD /set testsigning on
```

##### Disable integrity checks — allow unsigned kernel drivers
```cmd
bcdedit /store Y:\EFI\Microsoft\Boot\BCD /set nointegritychecks on
```

##### Disable Windows Recovery — no automatic-repair loops
```cmd
bcdedit /store Y:\EFI\Microsoft\Boot\BCD /set recoveryenabled no
```

##### Rebooting into Windows

> First boot takes a few minutes (OOBE). **A USB mouse is required** — the touchscreen driver does not exist yet.

#### What works right after boot
- Tray battery icon appears immediately; the real percentage binds ~1 minute later
- Wi-Fi connects automatically (if not: `pnputil /add-driver WLAN-standalone\x3n_wlan.inf /install` in Windows)

## Dual boot

- **Windows → Android**: reboot with Volume Down held (bootloader), then
```cmd
fastboot flash boot boot_backup.img
fastboot reboot
```
- **Android → Windows**: install the [WOA Helper](https://github.com/n00b69/woa-helper) app (root) → "Quickboot to Windows", or every time `fastboot boot boot-sweet.img` from the PC.

If something went wrong — see [Troubleshooting](troubleshooting.md).

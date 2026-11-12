<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Installation instructions

### What you need

1. **Unlocked bootloader**
2. [Platform-tools](https://developer.android.com/tools/releases/platform-tools).
3. **USB-C OTG hub + USB mouse** — the touchscreen does not work in Windows or UEFI yet.
4. A Windows 11 ARM64 ISO / install.wim / install.esd .
5. The package: `boot-sweet.img`, `sweet_drivers_all\`.

### Step 1 — Backups (do this first!)

Boot to Android, enable USB debugging, then:

```
adb shell su -c "dd if=/dev/block/by-name/boot of=/sdcard/boot_backup.img"

adb shell su -c "dd if=/dev/block/by-name/dtbo of=/sdcard/dtbo_backup.img"

adb pull /sdcard/boot_backup.img

adb pull /sdcard/dtbo_backup.img

```

Keep these two files — they are your way back to Android. First tests always run via `fastboot boot` (RAM only, nothing is flashed).

### Step 2 —  test the UEFI

```
fastboot boot boot-sweet.img   
```

Expected: the SimpleInit graphical menu on the AMOLED screen. Control it with the USB mouse.

- Menu visible → go to Step 3.
- Black screen / hang → reboot (Android is still intact) and try `boot-surya.img` as a fallback.

### Step 3 — Dump DTBO (for brightness/touch bring-up later)

In Android (root):

```
adb shell su -c "dd if=/dev/block/by-name/dtbo of=/sdcard/dtbo.img"
adb pull /sdcard/dtbo.img
```

Keep this file — the panel config and touch IRQ live in it.

### Step 4 — Repartition (wipes Android data!)

Via TWRP for sweet, or from the UEFI mass-storage mode on the PC:

```
sgdisk --resize-table 64 /dev/block/sda
parted /dev/block/sda
  print                          ; find the userdata partition number
  rm <userdata#>
  mkpart userdata ext4 11.7GB 66GB      ; Android ~54 GB
  mkpart esp fat32 66GB 66.3GB          ; ESP 300 MB
  mkpart winnt ntfs 66.3GB 100%         ; Windows, the rest (~200 GB)
  set <esp#> esp on
  quit
```

### Step 5 — Apply Windows and drivers

1. `fastboot boot boot-sweet.img`
2. In the UEFI (mouse!), enable **USB Mass Storage**.
3. On the PC, assign drive letters in diskpart: `X:` = winnt (big partition), `Y:` = esp (300 MB FAT32).
4. From an elevated prompt:

```
dism /apply-image /ImageFile:<ISO>\sources\install.esd /index:6 /ApplyDir:X:\
   (check the index with dism /get-imageinfo; you want Windows 11 Pro)

bcdboot X:\Windows /s Y: /f UEFI

D:\path\to\sweet_drivers_all\INSTALL-ALL.cmd X Y
```

`INSTALL-ALL.cmd X Y` installs all driver packages offline (`/ForceUnsigned`) **and enables testsigning in the phone's BCD** — required for the battery driver. Pass the ESP letter as the second argument.

### Step 6 — Reboot into Windows

Reboot the phone. Windows now boots from the phone's internal storage.

- Tray battery icon appears immediately; the real percentage binds ~1 minute after boot.
- Wi-Fi should connect automatically. If not: `pnputil /add-driver WLAN-standalone\x3n_wlan.inf /install`.

### Step 7 — Dual boot

- **Windows → Android**: reboot with Volume Down held (bootloader), then `fastboot flash boot boot_backup.img` — or use the WOA Helper app shortcut (root).
- 
- **Android → Windows**: WOA Helper app → "Quickboot to Windows" (root), or `fastboot boot boot-sweet.img` from the PC.

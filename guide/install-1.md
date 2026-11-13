<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 running on sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro/10 Pro Max

## Partitioning your device

### Prerequisites
- A brain (most important of all)
- An **unlocked bootloader** 
- Latest MIUI / Android installed with **root**
- [TWRP for sweet](https://dl.twrp.me/sweet/) (download in a browser )
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- A **USB-C OTG hub + USB mouse** (no touchscreen in UEFI/Windows yet)
- The package: `boot-sweet.img`, `sweet_drivers\`, `WLAN-standalone\` and a **Windows 11 ARM64 ISO** (23H2 tested)

### Notes
> [!WARNING]  
> All your data will be erased! Backup now if needed.
> 
> `boot-sweet.img` is **booted**, not flashed — never run `fastboot flash boot boot-sweet.img`. Use `fastboot boot` only. To restore Android afterwards you flash back your own `boot_backup.img`.
> 
> Do not run the same command twice.
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woasweet).
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

> [!IMPORTANT]
> Make sure you go through this whole tutorial with the provided `boot-sweet.img` (UEFI v2, SSDT-power build, md5 `4d589d06`). Do not mix it with UEFIs from other threads — the battery table ships inside this image.

##### Backing up important files
> Boot to Android (root required), open a CMD window in the platform-tools folder, then run
```cmd
adb shell su -c "dd if=/dev/block/by-name/boot of=/sdcard/boot_backup.img"
adb shell su -c "dd if=/dev/block/by-name/dtbo of=/sdcard/dtbo_backup.img"

adb pull /sdcard/boot_backup.img
adb pull /sdcard/dtbo_backup.img
```
> Keep both images on your PC — they are your way back to Android.

> [!warning]
> **IF YOU PROCEED WITHOUT BACKING UP `boot` AND `dtbo`, YOU ARE ON YOUR OWN IF YOU MESS UP**

##### Testing the UEFI (nothing is flashed)
> Reboot to the bootloader and boot our UEFI from RAM
```cmd
adb reboot bootloader
fastboot boot boot-sweet.img
```
> Expected: the SimpleInit graphical menu on the AMOLED screen. Control it with the USB mouse.
>
> Black screen / hang → hold power ~10s, Android is intact. Try again or ask in the chat.
>
> Partitioning below is done from TWRP.

### Partitioning guide
> Your phone has 128GB or 256GB of storage. This guide keeps ~54GB for Android and gives the rest to Windows, as an example.

##### Extending the partition limit
> Boot TWRP (`fastboot boot twrp.img` from the bootloader), open its Terminal, then run
```cmd
sgdisk --resize-table 64 /dev/block/sda
```

##### Opening parted
```cmd
parted /dev/block/sda
```

##### Printing the current table partition:
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

##### Deleting userdata
> Replace $ with the number of the userdata partition
>
> If it asks you if you are okay with data loss, type yes
```cmd
rm $
```

##### Recreating userdata (Android keeps ~54GB)
> 11.7GB is where the stock userdata begins (check your `print` output) and 66GB is where Android ends
```cmd
mkpart userdata ext4 11.7GB 66GB
```

##### Creating ESP partition
> 66GB is the **End** of **userdata** in this example and 66.3GB is the end of the ESP partition we will be creating, which will be 300MB in size
```cmd
mkpart esp fat32 66GB 66.3GB
```

> Replace "$" with your ESP partition number, usually the one right after userdata
```cmd
set $ esp on
```

##### Creating Windows partition
> 100% means "until the end of storage" — ~200GB on the 256GB model
```cmd
mkpart WINSWEET ntfs 66.3GB 100%
```

##### Exit parted
```cmd
quit
```
> Ignore "Information: You may need to update /etc/fstab."

##### Fixing the GPT backup
> CRITICAL — without this, Windows setup may wipe the partition table.
>
> Note: TWRP for sweet has **no** `fixgpt` command — do it directly with sgdisk (this is exactly what fixgpt does: move the backup GPT header to the true end of the disk)
```cmd
sgdisk -e /dev/block/sda
```

##### Formatting data
Format all data in TWRP, or Android will not boot.
( Go to Wipe > Format data > type yes )

#### Check if Android still starts
Just restart the phone, and see if Android still works


## [Next step: Installing Windows](/guide/install-2.md)

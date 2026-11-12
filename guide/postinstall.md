<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Optional post-install stuff

### Touch driver (when released)

The Goodix GT9896 digitizer driver is in development (UMDF HID over I2C7, Berlin protocol). Until it ships, use a USB mouse.

### Brightness

Requires a DTBO dump (`Step 3` of the installation) to extract the panel config, then an ACPI `_BCM` method in the UEFI tables. Bring your `dtbo.img` to the chat.

### Bypass Microsoft account requirement

`sweet_drivers_all\BypassNRO\` contains the package to skip the network requirement during OOBE — install it before going online, or press `Shift+F10` and run `oobe\bypassnro.cmd`.

### Keep Android and Windows in sync

- Install the [WOA Helper](https://github.com/n00b69/woa-helper) app on Android (root) for one-tap switching.
- Never let Windows Update flash the `boot` partition — it can't, but if you use third-party tools, remember the phone boots Android from `boot_backup.img` after a Windows session only if you flashed it back.
- Recharge often: charging runs in trickle mode (~0.005 W) under Windows. Long sessions need the cable.

<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Troubleshooting

### Black screen in UEFI

- Reboot (holding power ~10s) — Android is still intact, `fastboot boot` never flashed anything.
- Try `boot-surya.img` (Poco X3 NFC fallback, same SoC).
- Persistent hang → ask in the Telegram/Discord chat, UART debug is the last resort.

### Windows BSOD

Photograph the STOP code. Most common causes:

- `ACPI_BIOS_ERROR` → use the fallback `boot-surya.img` tables and report it in the chat.
- Driver crash → note the failing driver name on the BSOD, disable that package and reinstall.

### Battery percentage stuck / no percentage

The tray icon comes from the static ACPI battery and always works. The real percentage needs the PMFG driver, which needs test signing:

```
bcdedit /set testsigning on      ; in Windows on the phone, elevated
shutdown /r /t 0
pnputil /add-driver sweet_drivers_all\PMFG\sweet_pmfg.inf /install
```

If `INSTALL-ALL.cmd X Y` was used with the ESP letter, testsigning was already enabled — check `bcdedit /enum | findstr testsigning`.

### Wi-Fi gone after sleep

Reboot. Known quirk of the WCN stack on SM7150.

### Windows partition not visible in UEFI mass storage

You most likely forgot `fixgpt` after repartitioning. Redo Step 4 (repartition is destructive — back up first).

### Roll back to Android

```
fastboot flash boot boot_backup.img      ; stock boot image
fastboot flash dtbo dtbo_backup.img      ; stock dtbo
fastboot reboot
```

The Windows partitions do not bother Android — it boots and works normally with them present. To reclaim the space, delete the `esp` and `winnt` partitions in TWRP/parted and recreate a single userdata partition.

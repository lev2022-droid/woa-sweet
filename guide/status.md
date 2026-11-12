<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Project Status

The project is in beta. The phone boots Windows 11 ARM64 from the Renegade-based UEFI (v2, with the SSDT-power battery table). Expect driver updates (touch, loudspeakers, GPU) in the near future.

#### Features

- [x] Audio (earpiece codec — loudspeakers need the AW882xx amp driver)
- [ ] Microphone
- [ ] Jack 3.5 Audio
- [x] Battery status (real fuel-gauge %, needs testsigning — enabled by the installer)
- [x] Battery tray icon (power source, works immediately)
- [x] Bluetooth
- [ ] Brightness
- [ ] Camera
- [x] Cellular (calls / SMS / mobile data)
- [ ] Charging (trickle ~0.005 W only — keep the cable connected)
- [x] Display (AMOLED, framebuffer)
- [ ] GPU (Adreno 618 — framebuffer only, no acceleration)
- [ ] Touchscreen (Goodix GT9896 driver in development — use a USB mouse)
- [x] Vibration (AW8624 haptics)
- [x] UFS (256 GB)
- [x] USB (mass storage / MTP)
- [x] Wi-Fi (byte-verified package, connects automatically)

#### Sensors
- [ ] Accelerometer
- [ ] Fingerprint
- [ ] GPS
- [ ] Gyroscope
- [ ] Light sensor
- [ ] Magnetometer
- [ ] NFC
- [ ] Proximity

#### Security

- [ ] Security Processor
- [ ] TPM (Software Backed)

#### Dual boot

- [x] Android → Windows (WOA Helper app / `fastboot boot boot-sweet.img`)
- [x] Windows → Android (reboot to bootloader → flash back `boot_backup.img`)

#### What you need

- Unlocked bootloader (Mi Unlock, ~168 h wait — wipes all data)
- USB-C OTG hub + USB mouse (**required** — no touchscreen driver yet)
- A Windows 11 ARM64 ISO (23H2 tested)
- The package: `boot-sweet.img`, `sweet_drivers_all\`, `WLAN-standalone\`

## Notes

- The tray battery icon appears immediately (static ACPI battery from the SSDT); the real percentage binds about a minute after boot.
- If the percentage freezes at one number, `testsigning` is off — see [troubleshooting](troubleshooting.md#battery-percentage-stuck--no-percentage).
- Charging runs in trickle mode under Windows: long sessions need the cable.
- No Qualcomm GPU node exists in any public ACPI table for this SoC, so display runs on the framebuffer. The DX driver package ships in the pack for the day it becomes usable.

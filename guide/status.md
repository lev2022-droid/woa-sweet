<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Project Status

The project is in beta. The phone boots Windows 11 ARM64 from the Renegade-based UEFI (v2, with the SSDT-power battery table). Expect driver updates (touch, loudspeakers, GPU) in the near future.

#### Features

- [x] Audio (earpiece codec — loudspeakers need the AW882xx amp driver)
- [ ] Microphone
- [ ] Jack 3.5 Audio
- [x] Battery status
- [x] Bluetooth
- [ ] Brightness
- [ ] Camera
- [x] Cellular
- [ ] Charging (trickle ~0.005 W only — keep the cable connected)
- [x] Display
- [ ] GPU
- [ ] Touchscreen (Goodix GT9896 driver in development — use a USB mouse)
- [x] Vibration
- [x] UFS
- [x] USB
- [x] Wi-Fi

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



## Notes

- If the percentage freezes at one number, `testsigning` is off — see [troubleshooting](troubleshooting.md#battery-percentage-stuck--no-percentage).
- Charging runs in trickle mode under Windows: long sessions need the cable.
- No Qualcomm GPU node exists in any public ACPI table for this SoC, so display runs on the framebuffer. The DX driver package ships in the pack for the day it becomes usable.

<img align="right" src="../media/Redmi-Note-10-Pro.png" width="350" alt="Windows 11 Running On sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

## Project Status

The project is in **beta**. The phone boots Windows 11 ARM64 from the Renegade-based UEFI (v2), Wi-Fi  / Bluetooth / battery percentage work out of the box. Expect driver updates (touch, loudspeakers, GPU) in the near future.

#### Features

- [x] Display 
- [x] Battery status 
- [x] UFS 
- [x] USB 
- [x] Wi-Fi 
- [x] Bluetooth 
- [ ] Cellular
- [ ] SMS
- [ ] Calls
- [x] Vibration 
- [ ] Touchscreen (Goodix GT9896 — driver in development, use a USB mouse)
- [ ] GPU acceleration (framebuffer only for now)
- [ ] Loudspeakers (AW882xx amplifier has no driver yet; the earpiece codec works)
- [ ] Brightness control (needs DTBO dump → ACPI _BCM)
- [ ] Charging (trickle ~0.005 W only — keep the cable connected)
- [ ] Camera
- [ ] Microphone
- [ ] NFC
- [ ] 3.5 mm jack

#### Sensors

- [ ] Accelerometer
- [ ] Fingerprint
- [ ] GPS
- [ ] Gyroscope
- [ ] Light sensor
- [ ] Magnetometer
- [ ] Proximity

#### Security

- [ ] Security Processor
- [ ] TPM (Software Backed)


## Notes

- **Touch**: the digitizer (Goodix GT9896 on I2C7) has no driver yet — **a USB mouse through a USB-C OTG hub is required** for setup and UEFI.
- **GPU**: no Qualcomm GPU node exists in any public ACPI table for this SoC, so display runs on the framebuffer. The DX driver package ships in the pack for the day it becomes usable.

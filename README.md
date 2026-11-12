<img align="right" src="media/Redmi-Note-10-Pro.jpg" width="350" alt="Windows 11 running on sweet">

# Running Windows on the Xiaomi Redmi Note 10 Pro

[![Telegram](https://img.shields.io/badge/chat-telegram-brightgreen.svg?logo=telegram&style=flat-square)](https://t.me/woasweet)



## ⚠️ Warning

We're not responsible for bricked devices, missing recovery partitions, dead xiaomi factoryline ~~workers~~ cowboys, dead pmics, dead ram, dead sim cards, dead display ics, dead cpus, any xiaomi/redmi shenanigans, exploding batteries, dead cats or dogs, nuclear wars or you getting fired because you forgot to boot back in to android for the alarm.

This project is in an early stage. Your bootloader **must be unlocked** , and installing Windows **wipes your data**. Everything is tested through RAM-booting (`fastboot boot`) first — nothing is flashed until you decide to.

**IF YOU AREN'T COMFORTABLE MODDING YOUR PHONE OR ITS PARTITION TABLE OR YOU ARE PARANOID OF BRICKING YOUR DEVICE CLICK AWAY NOW!!! YOU HAVE BEEN WARNED, YOU ARE ON YOUR OWN IF YOU BRICK YOUR DEVICE!!! AGAIN! YOU HAVE BEEN WARNED!!!**


## Get Started

- [Project status](guide/status.md)

- [Installation instructions](guide/install-1.md)

- [Troubleshooting](guide/troubleshooting.md)


## Miscellaneous

- [Optional post-install stuff](guide/postinstall.md)

- [Roll back to Android](guide/troubleshooting.md#roll-back-to-android)


## What is this?

A curated package to boot **Windows 11 ARM64** on the **Redmi Note 10 Pro (sweet, SM7150)**:

| Piece | What it is |
|---|---|
| `boot-sweet.img` | UEFI firmware v2 — SimpleInit GUI, USB mass-storage, **SSDT-power (battery devices)** |
| `sweet_drivers_all\` | 27 signed driver packages + PMFG fuel-gauge battery driver |
| `WLAN-standalone\` | Wi-Fi package separately (byte-identical to the proven miatoll one) |

Built on the [Renegade Project](https://github.com/edk2-porting/edk2-msm) edk2-msm stack (surya base, community SM8180→SM7150 port) with a custom SSDT-power table grafted from the proven miatoll (Project-Aloha) ACPI.


## Contributors

[<img alt="Renegade Project" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/63859504?s=200&v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/edk2-porting)
[<img alt="Project Aloha" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/101461018?s=200&v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/Project-Aloha)
[<img alt="map220v" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/14368485?v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/map220v)
[<img alt="MollySophia" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/20746884?v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/MollySophia)
[<img alt="n00b69" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/83274506?v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/n00b69)
[<img alt="gus33000" src="https://images.weserv.nl/?url=https://avatars.githubusercontent.com/u/3755345?v=4&w=45&fit=cover&mask=circle&maxage=7d" />](https://github.com/gus33000)
[<img alt="Renegade Discord" src="https://images.weserv.nl/?url=https://cdn.discordapp.com/icons/736563593058713690/68f67bfddf4390b11effc99917b16338.webp?size=256&w=45&fit=cover&mask=circle&maxage=7d" />](https://discord.gg/XXBWfag)

# HakiOS on the Samsung Galaxy A32 4G

An experimental iOS 16.5 port for the Samsung Galaxy A32 4G, developed from [Luminous418’s original Galaxy A32 port](https://github.com/Luminous418/hakios-a32).

This repository documents later work on the **SM-A325M** variant: booting iOS, bringing up hardware, improving rendering, and investigating the remaining compatibility problems. It is a research project, **not a stable ROM or a ready-to-flash release**.

## Credits

**Luminous418 created the original HakiOS Galaxy A32 device port.** That work provided the foundation for this project and must retain its original attribution and license notices.

The later SM-A325M experiments were carried out by **Mauri with Codex**, building on Luminous418’s device work and the broader HakiOS project.

If you reuse these sources, please credit the original authors and preserve the included MIT and APSL 2.0 notices.

## Device and software

| Component | Tested configuration |
| --- | --- |
| Phone | Samsung Galaxy A32 4G, SM-A325M |
| SoC | MediaTek Helio G80 family |
| GPU | Mali-G52 |
| iOS version | iOS 16.5 |
| Kernel | HakiOS adaptation of XNU |
| Graphics path | Mali/Panfrost with OpenGL ES |
| Android boot | Chainboot arrangement using the device’s own boot image |

The upstream project targets the **SM-A325F**. Although the SM-A325M is closely related, model and firmware differences matter. Nothing in this repository should be assumed compatible with the Galaxy A32 5G or another Samsung device.

## What we achieved

On one SM-A325M, iOS 16.5 reached the lock screen and SpringBoard, and the interface accepted touch input. We were able to open and use parts of the system rather than stopping at a boot logo.

The following worked in at least some tests:

- iOS boot and SpringBoard
- Touch input
- Screen brightness control
- Flashlight
- Wi‑Fi association and Internet access
- A persistent iOS data volume
- An Android app that requested a one-time iOS boot
- An iOS app that returned to Android in earlier tests

The persistent data volume was configured at approximately **24 GiB** for the test device. That number describes our particular partition layout; it is not a safe partitioning recipe for other phones.

## Current limitations

This port is still experimental. A successful boot does not mean that every service or app works.

| Area | Current state |
| --- | --- |
| Rendering | GPU rendering works through Mali/Panfrost and OpenGL ES, but visual artifacts remain. |
| Performance | The interface is usable in places but still noticeably slow. A 90 Hz panel setting does not mean iOS renders at 90 FPS. |
| Metal | A complete, working Metal backend was not established in these tests. |
| Keyboard | Keys at the right edge can be clipped. The cause was investigated but not fixed. |
| Audio | Not confirmed working. |
| Bluetooth | Not confirmed working. |
| Camera | Not confirmed working. |
| Cellular/RIL | Calls, SMS, and mobile data were not integrated or verified. |
| Apps and jailbreak | Some apps were unstable. Sileo launched, but installing packages and using tweaks were not fully verified. |
| Return to Android | The app and five-finger gesture worked in earlier sessions but both failed in the last long-session test. |

The final long-session test also exposed a boot configuration error: iOS had accidentally been set as the default boot target. Reflashing a corrected boot image restored Android as the default. A forced restart left the iOS data volume marked dirty. **That final return failure has not been fixed or retested.**

## Why there is no universal flashable ZIP

The tested setup depended on files specific to the owner and the exact phone:

- An iOS 16.5 IPSW obtained by the tester
- The phone’s matching Samsung stock boot image and firmware
- A device-specific boot image
- An iOS system image and prepared partitions
- A compatible recovery and an unlocked bootloader

A boot image from one phone is not a safe general release for every A32. Flashing it without the matching system image and partition layout would not install a working iOS system.

This repository must not distribute Apple’s iOS binaries, Samsung firmware, another person’s stock boot image, credentials, or user data. Anyone developing from these sources needs to supply and build their own inputs and verify the resulting images for their device.

**Do not flash the source ZIP in recovery.** It is source code, not an installer.

## Installation and development

Start with [Luminous418’s upstream repository](https://github.com/Luminous418/hakios-a32) and its installation documentation. That guide describes the original baseline, not a verified one-command installation of our later SM-A325M experiments.

Before attempting an independent build:

1. Confirm the phone’s exact model and firmware revision.
2. Save and verify a restorable copy of its stock boot image.
3. Confirm that its bootloader can be unlocked and that recovery works.
4. Build the iOS system and device boot image from your own required inputs.
5. Inspect the partition layout on the actual phone before making any changes.
6. Check that **Android is the default boot target** and iOS starts only through a one-time request.
7. Verify image hashes and keep a tested recovery path.

Creating iOS partitions can modify the partition table and erase Android user data. Do not copy partition offsets or images from another phone.

## Research priorities

The next areas to investigate are:

1. Correct the remaining black regions, clipped elements, and other rendering defects.
2. Find and remove the largest frame-time costs.
3. Fix the keyboard’s right-edge layout and verify that visual positions match touch targets.
4. Make the return-to-Android paths reliable, including orderly storage shutdown.
5. Bring up and validate audio, Bluetooth, camera, and cellular services.
6. Explore a practical Metal implementation or translation layer for the Mali GPU.

These are goals, not promises of working features.

## Contributing

Reproducible findings are especially valuable. Please include the exact phone model, firmware build, what changed, how you tested it, and what the device actually displayed or logged. Keep personal data, Wi‑Fi credentials, device identifiers, firmware dumps, and copyrighted system files out of issues and pull requests.

When contributing work derived from the original device port, retain credit to **Luminous418**. Files derived from XNU must keep their applicable APSL 2.0 notices; do not relicense them as MIT.

## License and attribution

This project contains components under different licenses. Preserve the original **MIT** and **Apple Public Source License 2.0 (APSL 2.0)** files and notices from the relevant sources. Check each file’s origin before redistributing or relicensing it.

HakiOS is an independent research effort. It is not affiliated with or endorsed by Apple or Samsung. Apple, iOS, Samsung, Galaxy, and other product names belong to their respective owners.

# HR+ iOS 27 patch

An experimental iOS 27 compatibility patch for **one specific HR+ V007 firmware profile**, with an Android installer and a separate firmware download.

Initial pairing and music playback in both earbuds were reported working on **one pair with an iPhone 17 running iOS 27.0**. This is an unofficial community modification, not an Apple, HR or BullSuper release.

## Downloads

Choose **one** option. The APK already includes the firmware; you do not need both downloads.

| Option | Download | What it contains |
|---|---|---|
| Android installer — recommended | [HRplus-iOS27-English.apk](apk/HRplus-iOS27-English.apk?raw=true) | The modified BullSuper-based app, English update controls and both patched firmware files. |
| Firmware only — advanced users | [HRplus-V007-iOS27-firmware-v1.zip](firmware/HRplus-V007-iOS27-firmware-v1.zip?raw=true) | Left/right FOTA `.bin` files, instructions and SHA-256 checksums. No app. |

Download the APK using GitHub's **Download raw file** button if your browser opens its file page. File fingerprints are in [SHA256SUMS.txt](SHA256SUMS.txt).

## Exact compatibility

The tested device reports:

```text
A5G_BY_G23_M05_ST_007_20260122_L
```

It was sold as an AirPods Pro 2 V5.3 HR+ replica with an Airoha AB1562AE chip. **The chip name, sales name or a short “V7” label is not enough to establish compatibility.** Other boards, firmware profiles and revisions have not been validated. This is not a TigerBuilder firmware conversion.

The installer checks the full profile received from the connected device before enabling the update. If verification fails, stop; do not bypass the check or rename a firmware file to force acceptance.

## Install using Android

1. Download and install **HRplus-iOS27-English.apk** on an Android phone or tablet. Allow installation from that source if Android asks. The installed app is called **HR+ iOS27 Test** and is separate from official BullSuper.
2. Close BullSuper, FlyCC and other earbud apps. Disconnect the earbuds from other devices, pair them in Android Bluetooth settings, then open the app and select them using **+** on **My earbuds**.
3. Keep **both earbuds outside the charging case**, with **more than 20% battery each**, near the Android device.
4. Open **Update → Earbud firmware → Verify patch**. This checks the connected profile and bundled files; it does **not** flash firmware.
5. When verification succeeds, select **Install patch**, read the confirmation and confirm. **Cancel** closes the dialog without starting an update.
6. Keep the app open and the earbuds out of the case until the update **and restart** finish. Reaching 100% transfer is not the final success state. Do not start a second update while waiting for restart.
7. After success, disconnect from Android and test pairing and music on the iPhone.

**Flashing is experimental and can make an incompatible device unusable; recovery is not guaranteed.** The app uses the existing Airoha transfer flow and retains its battery/integrity checks, but these checks do not guarantee compatibility with every unit.

## What changed?

The firmware modification changes **18 bytes within a 37-byte validation block**. When the expected value of one negotiation byte is zero, it additionally accepts `0x80`. The command check, another parameter check and all nonzero configured expectations remain strict.

The firmware version stays **V7 / V007**. That is intentional: do not reflash just because the version number has not changed. A later official firmware installation may overwrite this modification; its compatibility has not been tested.

The app provides the bundled patch, checks the exact profile and file hashes, and adds English, readable update controls. Its original Airoha transfer implementation is retained.

- [Technical patch explanation](docs/PATCH.md)
- [What was tested](docs/VALIDATION.md)
- [Firmware-only instructions and individual files](firmware/README.md)
- [Credits and third-party components](CREDITS.md)

## Scope of validation

Validation is limited to the initial iPhone 17 / iOS 27.0 pairing and music test on one pair. Calls, microphone behavior, ANC/transparency, repeated reconnections, battery life, other devices and other OS versions were not individually validated. No universal compatibility or seamless switching claim is made.

The final English APK was installed and its navigation, file verification and confirmation/cancel controls were checked. Its firmware and transfer checks match the earlier successful flashing build; the final English APK was **not used for an additional flash test**.

## Reporting results

Include the full firmware profile, phone model, iOS version, Android version, and the exact stage/error shown by the installer. Do not post private pairing keys or unredacted diagnostic logs.

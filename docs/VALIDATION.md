# Validation

## Hardware test — 25 October 2026

One HR+ pair reporting `A5G_BY_G23_M05_ST_007_20260122_L` was updated with the earlier test installer. The Airoha SDK reported transfer completion, restart, reconnection and **Status Succeed**. The owner then reported successful initial pairing and music in both earbuds on an **iPhone 17 running iOS 27.0**.

This is one owner's initial result on one pair. Calls, microphone behavior, ANC/transparency, repeated reconnections and battery life were not separately validated. It does not establish support for other hardware revisions or OS versions.

## Final English APK

`HRplus-iOS27-English.apk` was installed on the test Android tablet. Navigation, **Verify patch**, the **Install patch** confirmation, **Cancel**, and back navigation were checked. The positive confirmation action was not pressed for an additional firmware transfer.

The embedded firmware is byte-for-byte identical to the successful test. File hashing, identity gating and transfer-start methods were compared against the tested build and remained unchanged. The final APK's v1/v2/v3 signatures and ZIP alignment were verified at build time.

APK SHA-256:

```text
d7d00da97aa09e61bea2d7402f1fd5b5e159fb7f24573d1a807f6803a7cdb49f
```

## Offline firmware verification

- **199,680 cases** passed in a bounded interpreter for the actual Xtensa validation-block instructions.
- Original/default inputs remain accepted; the additional `0x80` value is accepted only for the zero expected value.
- Other configured expectations, command checks and byte-4 checks retain their behavior.
- The patched block restores the connection pointer before entering the operational reply path.
- Package/section hashes, decompression and unchanged bytes outside the patch were checked.

The bounded interpreter is not a full firmware or Bluetooth emulator. Offline checks alone do not prove device compatibility or long-term reliability.

## Release packaging — 27 October 2026

The published APK and firmware are copies of the existing checked artifacts; packaging does not change their executable content. `release-manifest.json` records their hashes and the verification performed when assembling this publication. The APK's bundled left/right images are compared directly to the separate firmware downloads.

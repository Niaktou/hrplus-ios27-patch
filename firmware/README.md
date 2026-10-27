# Firmware only — HR+ V007 iOS 27 patch v1

This option is for users who already have a compatible dual-earbud Airoha FOTA workflow. For the checked app-based installation path, use the [Android APK](../apk/HRplus-iOS27-English.apk?raw=true).

**Exact tested profile:** `A5G_BY_G23_M05_ST_007_20260122_L`.

Do not select a package based only on the AB1562AE chip name, an HR+ sales name or a short V7 label. Other profiles are not validated. Flashing an incompatible image can make the earbuds unusable; recovery is not guaranteed.

## Files

- [Firmware ZIP](HRplus-V007-iOS27-firmware-v1.zip?raw=true) — both files, these instructions and checksums.
- [Left FOTA package](HRplus_V007_iOS27_candidate_v1_left.bin?raw=true).
- [Right FOTA package](HRplus_V007_iOS27_candidate_v1_right.bin?raw=true).

The two files are intentionally byte-for-byte identical, as were the original packages supplied for this exact profile. Each is 1,054,473 bytes and has SHA-256:

```text
e33a563c45a5a7bad6b4d957c8db68ab209e1711ddabbb12943b93132da1a5a2
```

## Using the files

1. Confirm the complete device profile and verify the file hashes before opening an update workflow.
2. Use only an updater and dual-earbud procedure already established as compatible with this HR+ profile. Assign the named left/right files to their corresponding sides where the tool requires separate inputs.
3. Keep both earbuds outside the case, charged above 20%, and near Android throughout transfer and restart. Wait for the updater's final success state.

These files are **not an APK** and cannot be installed by tapping them. Their availability does not mean official BullSuper, FlyCC or another updater supports importing them; no such import workflow is validated here. The bundled Android installer is the provided route with profile and file checks.

The displayed version stays V7/V007. If the patch is already installed, there is no need to flash it again. A later official firmware installation may overwrite the patch.

See the repository's `docs/PATCH.md` for the exact code change and `docs/VALIDATION.md` for test scope.

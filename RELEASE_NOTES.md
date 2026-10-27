# HR+ V007 iOS 27 patch v1 — experimental

Two download options:

- **HRplus-iOS27-English.apk** — Android installer with both firmware files included. Recommended for the documented installation path.
- **HRplus-V007-iOS27-firmware-v1.zip** — firmware only, with left/right `.bin` files, instructions and checksums.

**Exact tested profile:** `A5G_BY_G23_M05_ST_007_20260122_L`. Matching a chip name or a V7 label alone is not sufficient.

The patch changes 18 bytes in a parameter-validation block to additionally accept `0x80` when the expected value is zero. It preserves the other validation checks, firmware version and the rest of the decompressed payload. The app adds exact-profile/file checks and English update controls around the existing Airoha transfer flow.

**Test result:** initial pairing and music in both earbuds reported working on one pair with iPhone 17 / iOS 27.0. The final English APK's UI and bundled firmware were checked; the successful flashing test used the earlier installer with the same firmware and transfer logic. Other revisions, features and OS versions are not validated.

Keep both earbuds outside the case and charged above 20%. Use **Verify patch** before **Install patch**, and wait for restart and final success. Flashing is experimental and can make an incompatible device unusable. The firmware still displays V7/V007 after installation.

See the repository README for installation, `docs/PATCH.md` for the exact modification and `SHA256SUMS.txt` for fingerprints.

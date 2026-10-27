# Technical explanation

## Input and scope

This patch is specific to `A5G_BY_G23_M05_ST_007_20260122_L` and the reviewed HR+ V007 input packages. The original left/right packages were identical, despite their side-specific filenames.

- Original package size: **1,055,066 bytes** each.
- Original package SHA-256: `1dea0bbd7abe2f5fdaeac962d02ec06f5ca9195300ee2961353269c1adebd8d6`.
- Patched package size: **1,054,473 bytes** each.
- Patched package SHA-256: `e33a563c45a5a7bad6b4d957c8db68ab209e1711ddabbb12943b93132da1a5a2`.

The two patched packages are also identical. Both side-specific files are provided for dual-earbud FOTA tools. Package sizes differ because the modified payload is recompressed, not because firmware sections were removed.

## Firmware change

Analysis of the public [PairBridge project](https://github.com/MattiaIppoliti/pairbridge) provided a reference negotiation check. Its S305 patch could not be copied directly into HR+: the exact instruction sequence was absent and the HR+ validation logic differed.

The corresponding HR+ function was located at decompressed payload offset `0x6b01c`. The replacement block spans `[0x6b0d0, 0x6b0f5)` — **37 bytes**, of which **18 differ**. These are decompressed-payload offsets, not offsets in the compressed `.bin` download.

For the zero-based six-byte handler input, the relevant original rule was:

```text
input[1] == expected_command
AND input[4] == expected_byte4
AND input[5] == expected_byte5
```

The patched rule is:

```text
input[1] == expected_command
AND input[4] == expected_byte4
AND (
    (expected_byte5 == 0 AND input[5] IN {0x00, 0x80})
    OR
    (expected_byte5 != 0 AND input[5] == expected_byte5)
)
```

Default expectations are command `0x0a`, byte 4 `0x00`, byte 5 `0x00`. This is a narrowly scoped parameter-compatibility change. It does not disable Bluetooth authentication, encryption or pairing-key checks.

Reference inputs from PairBridge:

| Input | Original HR+ check | Patched HR+ check |
|---|---|---|
| `f6 0a 1f 01 00 00` | Accept | Accept |
| `f6 0a f6 76 00 80` | Reject | Accept |

These are public reference inputs, not packets captured from the tested owner's iPhone.

Original block:

```text
a86182040192040437983ca79939c851b20405c79b310c0ab1ccff38410c0c81895de00800
```

Replacement block:

```text
a86182040192040437983ca79939c851b20405562c00b0b064c79b2b3841c600003df03df0
```

The existing success-path debug log was removed to fit the extra comparison into the same block. Connection-pointer restoration and the operational reply path remain. No code cave was used and no section was moved. Bytes outside this block in the decompressed payload are unchanged; the version string and the other three sections are preserved.

The container was rebuilt with its package and section SHA-256 integrity fields updated, then decompressed and compared again. These hashes are integrity checks, **not manufacturer signatures**.

## Android installer change

The APK is an adapted BullSuper app with application ID `com.hzm.contro.gearphone.hrpatch`, installed separately from the official app. It includes:

- Both patched firmware files as bundled assets.
- A full-version check bound to the connected Bluetooth device, expiring after ten minutes.
- File size and SHA-256 checks before preparation and again before transfer.
- An OTA connection check and explicit install confirmation.
- A guard against starting another transfer in the same running session.
- English labels, visible confirmation buttons and clearer status messages.

The existing Airoha transfer implementation and its battery/integrity checks are retained. The final APK is signed with a **test/debug certificate**, not a manufacturer's certificate. It retains third-party app components; this work is not a complete security or privacy audit of those components.

The firmware-only files have no app-side identity guard on their own. A third-party updater must handle compatibility and the correct dual-earbud FOTA process; merely selecting a `.bin` file does not establish that it is safe to install.

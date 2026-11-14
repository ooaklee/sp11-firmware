# Redistribution licence notes

## Microsoft Surface firmware payloads

Touch, pen, Type Cover, SAM, retimer, UEFI, NFC, and NVMe payloads in this repository
originate from Microsoft's publicly distributed Surface firmware/driver archive
(`SurfacePro11_ARM_Win11_26100_26.041.12746.0`). They are redistributed as firmware
payloads only, without the accompanying Windows drivers (`.sys`/`.dll`), installers,
or catalogs. This is the same class of redistribution that the `linux-firmware`
project performs for vendor-supplied firmware: the firmware is licensed, not sold,
and is provided "AS IS" for use with the corresponding hardware. No source code for
these blobs exists, and they may not be decompiled, modified, or reverse engineered.

## Qualcomm (QCT) firmware

`firmware/26100_26.041.12746.0/camera-icp/*` contains Qualcomm-sourced
firmware, redistributed under the Qualcomm firmware terms used by `linux-firmware`
(the complete terms are included verbatim in
[`LICENSES/LICENSE.qcom`](LICENSE.qcom), copied from
[linux-firmware](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/LICENSES/LICENSE.qcom),
as those terms require redistributors of binary materials to include the
agreement text).
The camera ICP files mirror files already published in linux-firmware under
`qcom/x1e80100/camss/`. The modem OTA package (`mbbfwupdate/update.zip`, also
Qualcomm-sourced) is not stored in Git; it is attached to the release
`sp11-firmware-v1` as `sp11-firmware-modem-ota.zip` and is subject to the same
Qualcomm firmware terms.

## Excluded material

Windows driver binaries (`.sys`, `.dll`), installers (`.exe`, `.msi`), and security
catalogs (`.cat`) are excluded on purpose: they are covered by the Microsoft Windows
EULA, are licensed rather than sold, and are out of scope for this kit. Cleanroom
register tables documented from hardware behaviour live in the kernel repository's
`evidence/` directory, not here.

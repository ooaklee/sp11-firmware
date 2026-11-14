# sp11-firmware

Redistributable firmware payload kit for the **Surface Pro 11 (ARM / Snapdragon X
Elite)**, extracted from Microsoft's official Windows 11 26100 driver/firmware archive
(`SurfacePro11_ARM_Win11_26100_26.041.12746.0`).

## Purpose

The SP11 ships as a Windows machine: originally nothing on this hardware worked on
Windows without these firmware packages, and the same is true on Linux. Our project
ports Linux drivers to the SP11; those drivers need the underlying device firmware
payloads (touch, pen, Type Cover, SAM, retimers, NFC, camera, modem) at runtime, the
same way the kernel consumes blobs from `linux-firmware`.

This repository is a **self-compilation / self-install kit** for SP11 Linux users: it
contains only the firmware payloads, their checksums, and documentation, so users can
build their own firmware tree or drop files into an existing `linux-firmware`
installation without downloading the multi-gigabyte Windows archive.

## What is NOT included (and why)

- **No `.sys` or `.dll` drivers, installers, or `.cat` catalogs.** Microsoft's Windows
  EULA covers the driver binaries separately; they are licensed, not sold, and are not
  redistributable in the same class as firmware payloads. They are also useless on
  Linux.
- **No register tables or driver logic** reverse-engineered from those binaries.
  Cleanroom-documented register tables and hardware evidence live in the kernel
  repository's `evidence/` directory, not here.
- **Nothing was decompiled, patched, or modified.** All files are byte-identical
  copies (see `SHA256SUMS` in each `firmware/<version>/<category>/` directory).

## Layout

Firmware is organised **per Microsoft release** under
`firmware/<WindowsBuild>_<MSIVersion>/` so future versions can be added without
touching earlier ones (see
[`firmware/26100_26.041.12746.0/README.md`](firmware/26100_26.041.12746.0/README.md)
for the source links and iteration recipe for that release):

```
firmware/
  26100_26.041.12746.0/     ← current release (Win11 26100, MSI 26.041.12746.0)
    README.md   official Microsoft source links + how to add the next version
    touch/      Touch G6 firmware (0C80 + 0C83 variants), payload + offer files
    pen/        Slim Pen (0C0F) BLE / MCU / haptics payloads + offers
    typecover/  Type Cover V7 CFU images + offers
    sam/        Surface Aggregator Module firmware
    retimer/    USB4/TB retimer 0 and 1 firmware
    uefi/       UEFI capsule update
    nfc/        NXP PN560 NFC firmware
    camera-icp/ Qualcomm camera ICP firmware (mbn + elf)
    nvme/       Optional NVMe firmware payloads (one per drive family)
LICENSES/       Redistribution notes
MANIFEST.md     Per-file source folder, licence basis, Linux consumer mapping
```

## Install guidance

### linux-firmware drop-in

Copy payloads into your firmware tree following the mapping in `MANIFEST.md`, e.g.:

```sh
git clone https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git
cp firmware/26100_26.041.12746.0/camera-icp/CAMERA_ICP.mbn    linux-firmware/qcom/x1e80100/camss/
cp firmware/26100_26.041.12746.0/touch/*.payload.bin          linux-firmware/surface/
cp firmware/26100_26.041.12746.0/pen/*.payload.bin            linux-firmware/surface/
cp firmware/26100_26.041.12746.0/typecover/*.cfu              linux-firmware/surface/
cp firmware/26100_26.041.12746.0/sam/*.bin                    linux-firmware/surface/
cp firmware/26100_26.041.12746.0/retimer/*.bin                linux-firmware/surface/
cp firmware/26100_26.041.12746.0/nfc/NXPPN560FW.DAT           linux-firmware/nxp/
```

Then rebuild the `linux-firmware` package (or run `./copy-firmware.sh` into
`/lib/firmware`).

### Custom paths

For drivers we port ourselves that request firmware from non-standard paths, place
files directly, e.g.:

```sh
sudo mkdir -p /lib/firmware/surface
sudo cp firmware/26100_26.041.12746.0/touch/*.payload.bin /lib/firmware/surface/
```

The kernel logs will name the exact `request_firmware()` path each driver expects;
match it.

### UEFI capsule

`firmware/26100_26.041.12746.0/uefi/Surface_UEFI_175.222.235.bin` is a **firmware capsule**, not a runtime
blob. If you need a UEFI update, prefer letting the machine receive it via Windows or
a Microsoft-signed channel first; flashing a capsule on Linux requires `fwupd` support
or manual `CapsuleUpdate` boot flow, and a failure can brick the device. Do not treat
this file like the others.

## Licensing

Firmware payloads are redistributed under the vendor firmware redistribution terms
(Microsoft Surface firmware terms; Qualcomm QCT terms for camera/modem files) — the
same class of terms under which `linux-firmware` ships vendor blobs. See `LICENSES/`.

## Provenance

- Source archive: `SurfacePro11_ARM_Win11_26100_26.041.12746.0/SurfaceUpdate`
  (208 folders, Windows 11 26100, driver/firmware bundle version 26.041.12746.0)
- Official Microsoft download page (always serves the latest release — as of
  2026-11-14 it serves `SurfacePro11_ARM_Win11_26100_26.094.41119.0.msi`, not
  the version extracted here):
  <https://www.microsoft.com/en-us/download/details.aspx?id=106119>
- Official per-release listing: Surface Pro 11th Edition update history —
  <https://support.microsoft.com/en-us/surface/updates/surface-pro-11th-edition-update-history>
- Microsoft no longer serves the 26.041.12746.0 MSI directly. Third-party
  mirrors (e.g. Softpedia) list it, but these are **unverified third-party
  mirrors** — payloads in this repo come from the original official Microsoft
  archive and are pinned by `SHA256SUMS`; do not treat mirror copies as
  verified. See
  [`firmware/26100_26.041.12746.0/README.md`](firmware/26100_26.041.12746.0/README.md).
- Redistribution assessment: fleet agent A5 (firmware payloads only; verified
  `.sys`/`.dll` excluded)
- Verification: per-directory `SHA256SUMS`; verify with `shasum -a 256 -c SHA256SUMS`.

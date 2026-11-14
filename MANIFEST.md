# SP11 Firmware Manifest

> **Modem note:** the Windows archive's `mbbfwupdate/update.zip` (~120 MB) is a
> full Windows-side 5G modem OTA package (a 73 MB `NON-HLOS.ubi` flash image plus
> an embedded Linux rescue rootfs with Qualcomm kernel modules). It is not a
> runtime-loadable Linux firmware payload, exceeds GitHub's file-size limit, and
> Linux loads modem firmware as separate `.mbn` files (already shipped in
> linux-firmware for X1E devices). It is therefore intentionally **not** included.

Source: `SurfacePro11_ARM_Win11_26100_26.041.12746.0` (Windows 11 26100 driver/firmware
archive for Surface Pro 11 ARM, MSI version 26.041.12746.0). Files are byte-identical
copies of the firmware payloads in that archive. Only firmware payloads are included —
never `.sys`/`.dll` drivers, installers, or catalogs.

Licence basis: redistribution of firmware payloads under the Microsoft Surface firmware
redistribution terms, the same class of terms under which linux-firmware ships vendor
blobs ("firmware is licensed, not sold"). Qualcomm-sourced files fall under the Qualcomm
firmware redistribution terms used by linux-firmware (`LICENSES/LICENSE.qcom`, included in this repo).

| Category / file | Source folder | Licence | Intended Linux consumer |
|---|---|---|---|
| `firmware/26100_26.041.12746.0/touch/SurfaceTouchG6_FW_MSHW0484.payload.bin` (+ `.offer.bin`) | `touchg6fwupdateextnpackage0c80` | MS firmware | `linux-firmware/surface/` — touch screen (ipts/surface touch G6, device 0C80) |
| `firmware/26100_26.041.12746.0/touch/SurfaceTouchG6_FW_MSHW0485.payload.bin` (+ `.offer.bin`) | `touchg6fwupdateextnpackage0c83` | MS firmware | `linux-firmware/surface/` — touch screen (0C83 variant) |
| `firmware/26100_26.041.12746.0/pen/SurfacePen0C0F_{BLE-HI,BLE-LO,HAPTIC,MCU-B0}.payload.bin` (+ `.offer.bin`) | `pen0c0ffirmwareupdate` | MS firmware | `linux-firmware/surface/` — Slim Pen 2/3 (HID 0C0F) controller, BLE and haptics |
| `firmware/26100_26.041.12746.0/typecover/SurfaceTypeCoverV7_{FPRA,FPRB,KIP,KIP2,KIP3_APP1/APP2,TP,WLC,WLC2}.cfu` (+ `.offer.bin`) | `typecoverv7fwupdate` | MS firmware | `linux-firmware/surface/` — Type Cover CFU firmware (touchpad, fingerprint, keypad, wireless charger) |
| `firmware/26100_26.041.12746.0/sam/SurfaceSAM_27.201.139.bin` | `SAM` | MS firmware | `linux-firmware/surface/` — Surface Aggregator Module (SAM/SMF; consumed by `surface_aggregator`) |
| `firmware/26100_26.041.12746.0/retimer/SurfaceRetimer0_128.3.6.0.bin` | `retimer0` | MS firmware | `linux-firmware/surface/` — USB4/TB retimer 0 |
| `firmware/26100_26.041.12746.0/retimer/SurfaceRetimer1_128.3.6.0.bin` | `retimer1` | MS firmware | `linux-firmware/surface/` — USB4/TB retimer 1 |
| `firmware/26100_26.041.12746.0/uefi/Surface_UEFI_175.222.235.bin` | `uefi` | MS firmware | UEFI capsule (update via `fwupd`/capsule, not a runtime driver; do NOT flash blindly) |
| `firmware/26100_26.041.12746.0/nfc/NXPPN560FW.DAT` | `nfcclient` | NXP/MS firmware | `linux-firmware/nxp/` — PN560 NFC controller |
| `firmware/26100_26.041.12746.0/camera-icp/CAMERA_ICP.mbn` | `qccamisp` | QCT (Qualcomm) | `linux-firmware/qcom/x1e80100/camss/` — camera ICP firmware; mirrors existing linux-firmware `qcom/x1e80100/` camss files |
| `firmware/26100_26.041.12746.0/camera-icp/CAMERA_ICP_AAAAAA.elf` | `qccamisp` | QCT (Qualcomm) | `linux-firmware/qcom/x1e80100/camss/` — camera ICP ELF (same content class as CAMERA_ICP.mbn) |
| `firmware/26100_26.041.12746.0/modem/update.zip` | `mbbfwupdate` | QCT (Qualcomm) | Modem (WWAN) full image bundle; consumed by qcom MHI/mbim stack after extraction. Large (~120 MB) |
| `firmware/26100_26.041.12746.0/modem/FwVersions.txt` | `mbbfwupdate` | MS (text metadata) | Version metadata for `update.zip` |
| `firmware/26100_26.041.12746.0/nvme/HFS256GEJ3X108N-SKhynix_51005C30.ebin` | `storagefwupdatehfs256gej3x108n` | SK hynix/MS firmware | Optional NVMe FW update payload (256G model; identical bytes across 256G/512G/1T) |
| `firmware/26100_26.041.12746.0/nvme/ANMS0103.sig` | `storagefwupdatekbg50zns256g` | Kioxia/MS firmware | Optional NVMe FW payload (KBG50ZNS; identical across capacities) |
| `firmware/26100_26.041.12746.0/nvme/HXC75M0Q_noFormat.bin` | `storagefwupdatemz9l4256hcjq00bmv` | Samsung/MS firmware | Optional NVMe FW payload (MZ9L4; identical across capacities) |
| `firmware/26100_26.041.12746.0/nvme/73107024.fluf` | `storagefwupdatesddptqd256g1124` | Sandisk/MS firmware | Optional NVMe FW payload (SDDPTQD; identical across capacities) |

Storage (NVMe) note: only one file per drive model family is shipped because the
capacity variants in the archive are byte-identical (verified by hash). Total NVMe
payload kept small (~7.5 MB) per scope.

Each category directory carries its own `SHA256SUMS` covering the files in that
directory.

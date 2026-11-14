# Firmware 26100_26.041.12746.0 (Windows 11 26100, MSI 26.041.12746.0)

This directory holds the SP11 firmware payload kit extracted from Microsoft's
Windows 11 26100 driver/firmware archive for Surface Pro 11 (ARM / Snapdragon X
Elite), MSI version **26.041.12746.0**
(`SurfacePro11_ARM_Win11_26100_26.041.12746.0.msi`).

## Official source

Microsoft publishes the Surface Pro 11th Edition (Snapdragon) driver & firmware
package on the Microsoft Download Center:

- **Current download page (always latest version):**
  <https://www.microsoft.com/download/details.aspx?id=106119>
- Direct MSI for the version in this directory (if still hosted by Microsoft):
  <https://download.microsoft.com/download/92821b24-203a-4ce5-9a3a-5df3160a2fe3/SurfacePro11_ARM_Win11_26100_26.041.12746.0.msi>

> **Note:** Microsoft replaces older MSIs with the newest release on this
> page — it now serves `SurfacePro11_ARM_Win11_26100_26.094.41119.0.msi`
> (verified 2026-11-14); the June 2026 driver/firmware release is listed on
> Microsoft's official
> [Surface Pro 11th Edition update history](https://support.microsoft.com/en-us/surface/updates/surface-pro-11th-edition-update-history).
> If the direct link above 404s, use the download page link, which always
> serves the latest version.

> **Historical copies of the MSI from this directory (26.041.12746.0):**
> Third-party mirrors such as
> [Softpedia](https://drivers.softpedia.com/get/Tablets/Microsoft/Microsoft-Surface-Pro-11th-Edition-Tablet-Firmware-Driver-June-2026.shtml)
> and [fwatlas](https://fwatlas.com/microsoft/surface-pro-11th-edition-with-snapdragon/26-041-12746-0-windows-11-26100)
> list the older MSI. These are **unverified third-party mirrors**: Microsoft
> no longer serves this version directly, so any copy obtained there cannot be
> checked against an official Microsoft source. Only the payloads in this
> repository (with their `SHA256SUMS`, extracted from the original official
> Microsoft archive) are considered verified; treat mirror-downloaded MSIs as
> untrusted and re-verify extracted payloads against the checksums here before
> use.

## What is here

Byte-identical firmware payloads only (see each `SHA256SUMS`); no `.sys`/`.dll`
drivers, installers, or catalogs. Camera-ICP and NVMe payloads are also in Git
here (`camera-icp/`, `nvme/`); the **modem OTA package is not in Git** — it is
shipped only as a repo release asset (`sp11-firmware-modem-ota.zip`, release
`sp11-firmware-v1`) due to file-size limits.

## Versioning / iteration

Each Microsoft driver/firmware release gets its own directory:

```
firmware/<WindowsBuild>_<MSIVersion>/     e.g. 26100_26.041.12746.0
  README.md          this file (source links, verification, status)
  touch/  pen/  typecover/  sam/  retimer/  uefi/  nfc/  camera-icp/ ...
  SHA256SUMS         per-directory checksums
```

To add a new version:

1. Download the new `.msi` from the official download page above.
2. Create `firmware/<build>_<msiversion>/` mirroring this layout.
3. Copy firmware payloads only (never drivers/installers), byte-identical.
4. Generate `SHA256SUMS` per category directory
   (`sha256sum * > SHA256SUMS` inside each directory).
5. Update the top-level `MANIFEST.md` and `README.md` with the new version and
   any payload changes.
6. Keep this README as the template; update the direct MSI link and date.

## Licensing / provenance

Firmware payloads are redistributed under the vendor firmware redistribution terms
("firmware is licensed, not sold", provided AS-IS for use with the corresponding
hardware): the Microsoft Surface firmware terms embedded in the source MSI's
end-user licence terms, and the Qualcomm firmware redistribution terms used by
linux-firmware (`LICENSES/LICENSE.qcom`, included in this repo) for Qualcomm-sourced files. The redistribution
basis is the same as linux-firmware's shipping of vendor blobs; no source code
exists for these blobs, and they may not be decompiled, modified, or reverse
engineered. See `LICENSES/REDISTRIBUTION.md` at the repo root and the top-level
`MANIFEST.md` for the per-file licence table. If your use case requires more
certainty than this analogy provides, obtain your own copy from the official
Microsoft download link above — that is the safest provenance.

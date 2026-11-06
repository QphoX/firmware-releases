# QphoX firmware releases

Published firmware for QphoX devices. The QphoX control software uses this
repository to check for and install firmware updates.

This repository is maintained by automated release tooling. Please do not
edit it by hand.

## Layout

One folder per product, and one GitHub Release per firmware version:

```
<product>/latest.json      newest stable firmware for that product
```

| Product | Latest stable firmware |
|---------|------------------------|
| `labphox` | [`labphox/latest.json`](labphox/latest.json) (created with the first release) |

Release tags are `<product>-v<version>`, for example `labphox-v5`.
Tags with a suffix, such as `labphox-v6-rc1`, are pre-releases. A pre-release
never appears in `latest.json`.

Each release contains:

| File | Content |
|------|---------|
| `<Image>.bin` | Raw firmware image |
| `<Image>.hex` | Same firmware as Intel HEX (contains its own load addresses) |
| `SHA256SUMS` | SHA-256 checksums of the `.bin` and `.hex` |
| `BUILD_INFO.txt` | Version, build type, source commit, toolchain |

## `latest.json`

```json
{
  "schema": 1,
  "product": "labphox",
  "sw_version": 5,
  "tag": "labphox-v5",
  "release_url": "https://github.com/QphoX/firmware-releases/releases/tag/labphox-v5",
  "published_at": "2026-01-01T12:00:00Z",
  "build_type": "Release",
  "files": {
    "bin": {
      "name": "Labphox.bin",
      "url": "https://github.com/QphoX/firmware-releases/releases/download/labphox-v5/Labphox.bin",
      "sha256": "<64 hex characters>",
      "size": 123456
    },
    "hex": {
      "name": "Labphox.hex",
      "url": "https://github.com/QphoX/firmware-releases/releases/download/labphox-v5/Labphox.hex",
      "sha256": "<64 hex characters>",
      "size": 345678
    }
  }
}
```

(The values above are an example, not a real release.)

| Field | Meaning |
|-------|---------|
| `schema` | Format version of this file. Clients should refuse values they do not know. |
| `product` | Product id, equal to the folder name. |
| `sw_version` | Firmware version as an integer. It is the same number the device reports. |
| `tag` | Release tag in this repository. |
| `release_url` | Release page. |
| `published_at` | Publication time, UTC, ISO 8601. |
| `build_type` | Build configuration. |
| `files.<kind>.url` | Direct download link. |
| `files.<kind>.sha256` | SHA-256 of the file, lowercase hex. |
| `files.<kind>.size` | File size in bytes. |

## How clients update a device

1. Download
   `https://raw.githubusercontent.com/QphoX/firmware-releases/main/<product>/latest.json`.
   This URL is cached, so a new release can take a few minutes to appear.
2. If `sw_version` is greater than the version the device reports, download
   `files.bin.url`.
3. Check that the file's size and SHA-256 match `latest.json`. If they don't,
   discard the file.
4. Flash the device.

`latest.json` never moves to a lower `sw_version`.

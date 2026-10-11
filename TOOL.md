# Download tool (fixed build)

`manifest_fixed_windows_amd64.zip` contains `manifest.exe`, a build of [meszmate/manifest](https://github.com/meszmate/manifest) v0.1.0 with one bug fixed.

## Why this build

The original v0.1.0 assumes every chunk on Epic's CDN is zlib-compressed. Epic stores some chunks uncompressed, and on those the original fails with:

```
decompress: zlib init: zlib: invalid header
```

About a quarter of the chunks in Dauntless 1.14.7 are stored that way, so the original tool can never finish that build. This build reads each chunk's header and handles both kinds. Nothing else is changed.

## What is in the zip

| File | What it is |
|---|---|
| `manifest.exe` | The fixed tool, Windows 64-bit |
| `plain-chunk-fix.patch` | The exact source change, also viewable [here](plain-chunk-fix.patch) |
| `LICENSE.txt` | The original project's MIT licence |
| `README.txt` | Short notes |

## Checksums (SHA-256)

```
bc508a630942fd99fd2b787a5778874b03e648af49b67001977aa0a0c2aed235  manifest_fixed_windows_amd64.zip
e7ad5835e927ffb0020f7b0b05cc2bdef025ce94f31984d89ec99df340f6c204  manifest.exe
```

On Windows: `certutil -hashfile manifest_fixed_windows_amd64.zip SHA256`

## Building it yourself

If you would rather not run a prebuilt program, clone the original project, apply `plain-chunk-fix.patch`, and build with Go:

```
git clone https://github.com/meszmate/manifest
cd manifest
git apply plain-chunk-fix.patch
go build -o manifest.exe ./cmd/manifest
```

Windows may show a SmartScreen warning for the prebuilt file because it is unsigned. The original release is unsigned too.

# How to download a build

You need the tool and one command. No Epic login is involved.

## 1. Get the tool

Download **[manifest_fixed_windows_amd64.zip](https://github.com/addison2k/DauntlessManifestArchive/raw/main/manifest_fixed_windows_amd64.zip)** from this repo and unzip it.

This is [meszmate/manifest](https://github.com/meszmate/manifest) v0.1.0 with one bug fixed. **Do not use the original v0.1.0 release:** it fails on about a quarter of the chunks in some builds with `zlib: invalid header` and can never finish. Details, the source patch and checksums are in [TOOL.md](TOOL.md).

Open Command Prompt in the unzipped folder: open the folder in File Explorer, click the address bar, type `cmd` and press Enter.

```
manifest --version
```

## 2. Download a build (quick way)

The tool can read a manifest straight from this repo, so there is nothing to save first. Copy the command for the version you want, change the `--output` folder, and run it as one line.

**Disk space:** you need about twice the installed size free on the output drive while it runs. The tool stores all the chunks first, then builds the game files from them, then deletes the chunks. Once it finishes, the folder shrinks to the installed size.

**1.6.2** (11.56 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2k/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.6.2.271281.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.6.2"
```

**1.12.0** (15.29 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2k/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.12.0.392819.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.12.0"
```

**1.14.7** (15.39 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2k/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.14.7.647472.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.14.7"
```

**2.1.1** (14.45 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2k/DauntlessManifestArchive/main/manifests/Windows/Dauntless_2.1.1.682875.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_2.1.1"
```

If the download is interrupted, run the same command again. Chunks already downloaded are skipped. Game files only appear in the folder at the very end, so an empty-looking folder during the download is normal.

A finished download ends with a summary that says `failed: 0`.

## 3. Verify

Use the same manifest address and your output folder:

```
manifest verify "https://raw.githubusercontent.com/addison2k/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.6.2.271281.manifest" --path "D:\Dauntless_1.6.2"
```

## Using a manifest file instead

If you have a `.manifest` file on disk, for example one that is not in this repo yet, put its path where the web address goes:

```
manifest download "C:\path\to\file.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless"
```

To see which build a manifest is:

```
manifest info "C:\path\to\file.manifest"
```

**Saving a manifest from this repo:** open the file's page on GitHub and use the download button at the top right of the file view. Do not right-click a link and choose "Save link as". That saves a web page, and the tool will reject it with a `bad magic` error.

## The base URL

It is the same for every Dauntless build:

```
https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
```

## If it is slow

Speed depends on your connection and on Epic's servers. If it crawls, you can stop with `Ctrl+C` and try a different host or more parallel downloads. Keep the same `--output` folder so progress carries over.

Other hosts (same path, same files):

```
https://egdownload.fastly-edge.com/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
https://egs-cloudfront-chunks.epicgamescdn.com/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
```

More parallel downloads: add `--concurrency 32`.

## If chunks fail

**`zlib: invalid header` errors, or the same number of chunks failing on every run:** you are using the original tool. Get the fixed build from step 1 and run the same command with the same `--output` folder. Nothing you already downloaded is lost.

A `403` or `404` on chunks means Epic has removed that build's data. Please open an issue saying which version, so the README can be updated.

The `--dry-run` flag does **not** contact Epic. It only counts files. A real download is the only full test.

## Good to know

- The game is online-only. These files are for preservation, datamining and private server projects. A stock client will not get past the title screen.
- One manifest is one build. There is no way to get a different version from it.
- Epic could remove these files at any time. If a build matters to you, keep your downloaded copy.

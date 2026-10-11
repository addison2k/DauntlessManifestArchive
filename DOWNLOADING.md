# How to download a build

You need the tool and one command. No Epic login is involved.

## 1. Get the tool

Download the Windows build of [meszmate/manifest](https://github.com/meszmate/manifest/releases) and unzip it. Open Command Prompt in that folder.

```
cd /d "C:\path\to\manifest_0.1.0_windows_amd64"
manifest --help
```

## 2. Download a build (quick way)

The tool can read a manifest straight from this repo, so there is nothing to save first. Copy the command for the version you want, change the `--output` folder, and run it as one line.

Make sure the output drive has room for the installed size listed in the [README](README.md).

**1.6.2** (11.56 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2kpuzzle-source/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.6.2.271281.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.6.2"
```

**1.14.7** (15.39 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2kpuzzle-source/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.14.7.647472.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.14.7"
```

**2.1.1** (14.45 GB installed)

```
manifest download "https://raw.githubusercontent.com/addison2kpuzzle-source/DauntlessManifestArchive/main/manifests/Windows/Dauntless_2.1.1.682875.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_2.1.1"
```

If the download is interrupted, run the same command again. Chunks already downloaded are skipped.

## 3. Verify

Use the same manifest address and your output folder:

```
manifest verify "https://raw.githubusercontent.com/addison2kpuzzle-source/DauntlessManifestArchive/main/manifests/Windows/Dauntless_1.6.2.271281.manifest" --path "D:\Dauntless_1.6.2"
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

Old builds are not cached near you, so 5 to 8 MB/s is normal. You can stop with `Ctrl+C` and try a different host or more parallel downloads. Keep the same `--output` folder so progress carries over.

Other hosts (same path, same files):

```
https://egdownload.fastly-edge.com/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
https://egs-cloudfront-chunks.epicgamescdn.com/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
```

More parallel downloads: add `--concurrency 32`.

## If chunks fail

A `403` or `404` on chunks means Epic has removed that build's data. Please open an issue saying which version, so the README can be updated.

The `--dry-run` flag does **not** contact Epic. It only counts files. A real download is the only full test.

## Good to know

- The game is online-only. These files are for preservation, datamining and private server projects. A stock client will not get past the title screen.
- One manifest is one build. There is no way to get a different version from it.
- Epic could remove these files at any time. If a build matters to you, keep your downloaded copy.

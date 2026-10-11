# How to download a build

You need three things: the tool, a manifest from this repo, and Dauntless's base URL. No Epic login is involved.

## 1. Get the tool

Download the Windows build of [meszmate/manifest](https://github.com/meszmate/manifest/releases) and unzip it. Open Command Prompt in that folder.

```
cd /d "C:\path\to\manifest_0.1.0_windows_amd64"
manifest --help
```

## 2. Get a manifest

Download the `.manifest` for the version you want from the table in the [README](README.md).

To confirm what a manifest is:

```
manifest info "C:\path\to\Dauntless_1.6.2.271281.manifest"
```

## 3. Download

The base URL is the same for every Dauntless build:

```
https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default
```

Full command (one line):

```
manifest download "C:\path\to\Dauntless_1.6.2.271281.manifest" --base-url "https://epicgames-download1.akamaized.net/Builds/Org/o-krlzxj88qrtb69fredeuaf887bl5az/Jackal/default" --output "D:\Dauntless_1.6.2"
```

Make sure the output drive has room for the installed size listed in the README, plus a little extra.

If the download is interrupted, run the same command again. Chunks already downloaded are skipped.

## 4. Verify

```
manifest verify "C:\path\to\Dauntless_1.6.2.271281.manifest" --path "D:\Dauntless_1.6.2"
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

To spot-check before a full download, the `--dry-run` flag does **not** contact Epic. It only counts files. A real download is the only full test.

## Good to know

- The game is online-only. These files are for preservation, datamining and private server projects. A stock client will not get past the title screen.
- One manifest is one build. There is no way to get a different version from it.
- Epic could remove these files at any time. If a build matters to you, keep your downloaded copy.

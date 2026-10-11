# How to find and submit a manifest

If you ever had Dauntless installed on PC, you may have a manifest that exists nowhere else. Checking takes about two minutes.

## Is it safe to share?

Yes. A manifest only describes the game build: file names, sizes, checksums and chunk IDs. It contains no Epic account, email, login token, Windows username or folder paths. It is the same data for everyone who installed that version.

Do **not** send `.item` files from `C:\ProgramData\Epic\EpicGamesLauncher\Data\Manifests`. Those are a different thing, they include your install paths, and they are not needed.

## Where to look

### 1. Any Dauntless folder that came from Epic

Every Epic install has a hidden `.egstore` folder holding that version's manifest. This includes old installs, backups, and copies on other drives. If you stopped updating at some point, the manifest is for the version you stopped on.

Search a folder:

```
dir /a /s /b "D:\path\to\Dauntless\*.manifest"
```

Search whole drives (one line, edit the drive letters):

```
for %d in (C D E F G) do @dir /a /s /b "%d:\*.manifest" 2>nul | findstr /i "egstore"
```

Epic manifests have a 32-character name, like `B647F1DC4AF91373FD12C2BCCE018EDE.manifest`.

### 2. Legendary or Heroic

These launchers keep a copy of the manifest for every version they installed. Look in:

```
%USERPROFILE%\.config\legendary\manifests
%APPDATA%\heroic\legendaryConfig\legendary\manifests
```

Dauntless files there start with `Jackal`.

### 3. A copied game folder with no .egstore

If you only have the game files and no `.egstore`, there is no manifest to recover. A manifest cannot be rebuilt from game files. `Version.txt` in the game folder will still tell you which build you have, which is useful to report.

## Check what you found

With [meszmate/manifest](https://github.com/meszmate/manifest/releases):

```
manifest info "path\to\file.manifest"
```

- **App Name** should be `Jackal` (or `JackalProdTest` on the final builds). Anything else is a different game.
- **Build Version** is the version, for example `1.12.0.392819`.

No tool? Send the file anyway and it will be checked.

## How to submit

Pick whichever is easiest:

- **Pull request.** Add the file to `manifests/Windows/` named `Dauntless_<build version>.manifest`, for example `Dauntless_1.12.0.392819.manifest`.
- **Issue.** Open a "Submit a manifest" issue and attach the file. GitHub does not accept `.manifest` attachments, so zip it first.
- **Direct message.** See the Contact section in the [README](README.md).

Please do not open the file in a text editor and re-save it. It is binary and that corrupts it. Zipping is fine.

Duplicates of versions already archived are still useful: they confirm the file is genuine.

## What happens next

Each submission is parsed, its version confirmed, a few chunks are tested against Epic's CDN, and its SHA-1 is recorded. Then it is added to the table. You are credited by whatever name you like, or not at all if you prefer.

## What not to submit

- Game files, paks, executables or archives of a game folder
- `.item` files
- Anything that is not a manifest

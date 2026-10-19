# Dauntless Manifest Archive

An archive of Epic Games manifests for old PC builds of **Dauntless**, so the builds can still be fetched from Epic's own servers.

Dauntless shut down in May 2025, but Epic's CDN still serves the chunks for old builds. The only thing you need to download one is that build's **manifest**: a small file (about 1 MB) that lists which chunks make up the build. Epic only hands out the manifest for the current build, so every older one has to come from someone who saved it. This repo collects them.

**This repo contains manifests and a small download tool. No game files are hosted here.**

- [How to download a build](DOWNLOADING.md) (uses the [fixed tool](TOOL.md) in this repo)
- [How to find and submit a manifest](CONTRIBUTING.md)

## Most wanted

| Version | Why |
|---|---|
| **1.4.4** | Target build of the dauntless-revived private server project |
| **2.1.0.681846** | First Awakening build |
| Anything from 2019 or 2020 | Nothing from before Reforged (1.5.0, December 2020) is archived yet |

If you ever installed Dauntless through Epic, Legendary or Heroic, you may have one of these without knowing it. [Checking takes two minutes](CONTRIBUTING.md).

## Archived builds (Windows)

| Version | Build | Files | Download | Installed | Manifest | SHA-1 |
|---|---|---|---|---|---|---|
| 1.5.4 | 259586 | 442 | 11.59 GB | 12.09 GB | [download](manifests/Windows/Dauntless_1.5.4.259586.manifest) | `18680f431b1bed1258c283aae8a2244ddef1fdb8` |
| 1.6.2 | 271281 | 444 | 11.08 GB | 11.56 GB | [download](manifests/Windows/Dauntless_1.6.2.271281.manifest) | `1d98a586c34a69d3ea8d26bcd1e4dcfd3113e751` |
| 1.12.0 | 392819 | 609 | 14.74 GB | 15.29 GB | [download](manifests/Windows/Dauntless_1.12.0.392819.manifest) | `46b4097c070afc29ed783e77a68ffa936b6a3504` |
| 1.14.5 | 582369 | 611 | 14.90 GB | 15.40 GB | [download](manifests/Windows/Dauntless_1.14.5.582369.manifest) | `c5ff78582b97e0f67a9cf79d7f6842cfd9049007` |
| 1.14.6 | 615145 | 611 | 14.89 GB | 15.40 GB | [download](manifests/Windows/Dauntless_1.14.6.615145.manifest) | `db712f4f6d8adf8be4fa957047b8914dc58bb3e2` |
| 1.14.7 | 647472 | 611 | 14.89 GB | 15.39 GB | [download](manifests/Windows/Dauntless_1.14.7.647472.manifest) | `aa70748e4c0a776a991f2bac10ad342adaa31476` |
| 2.1.1 | 682875 | 832 | 13.86 GB | 14.45 GB | [download](manifests/Windows/Dauntless_2.1.1.682875.manifest) | `2406ba6b70410692acbad2dbbcd9090e0349576e` |

1.14.7 was downloaded in full from Epic's CDN on October 11, 2026. 1.14.5 was spot-checked on October 19, 2026. The other builds were spot-checked or added the same week. Epic can remove old chunks at any time, so a manifest here is not a guarantee.

## Wanted builds

Dauntless was on the Epic Games Store from May 21, 2019 until the shutdown, so every build in that window had an Epic manifest. Rows marked **wanted** are placeholders. Dates and version numbers below are from public patch notes and may be incomplete, so corrections are welcome.

### By release line

| Line | Era | Status |
|---|---|---|
| 0.8.x | May to June 2019, Epic launch (0.8.1, 0.8.2, 0.8.2A, 0.8.3 known) | wanted |
| 0.9.x | Mid 2019 | wanted |
| 1.0.x | From September 2019, Aether Unbound | wanted |
| 1.1.x | From December 2019 | wanted |
| 1.2.x | 2020 | wanted |
| 1.3.x | 2020 | wanted |
| 1.4.x | 2020, last line before Reforged | wanted, **1.4.4 most of all** |
| 1.5.x | From December 3, 2020, Reforged | **1.5.4 archived**, others wanted |
| 1.6.x | 2021 | **1.6.2 archived**, others wanted |
| 1.7.x | 2021 | wanted |
| 1.8.x | From October 14, 2021 | wanted |
| 1.9.x | 2021 to 2022 | wanted |
| 1.10.x | 2022 | wanted |
| 1.11.x | 2022, through 1.11.5 in September | wanted |
| 1.12.x | October to December 2022 | **1.12.0 archived**, others wanted |
| 1.13.x | January to April 2023 | wanted |
| 1.14.x | April 2023 to April 2024 | **1.14.5, 1.14.6 and 1.14.7 archived**, others wanted |
| 2.1.x | December 2024, Awakening | **2.1.1 archived**, 2.1.0 wanted |

### Known point releases, 1.12 onward

| Version | Released | Status |
|---|---|---|
| 1.12.0 (build 392819) | October 13, 2022 | **archived** |
| 1.12.1 | 2022 | wanted |
| 1.12.2 | November 9, 2022 | wanted |
| 1.12.3 | December 1, 2022 | wanted |
| 1.12.4 | December 15, 2022 | wanted |
| 1.13.0 | January 19, 2023 | wanted |
| 1.13.1 | February 1, 2023 | wanted |
| 1.13.2 | February 16, 2023 | wanted |
| 1.13.3 | March 9, 2023 | wanted |
| 1.13.4 | March 23, 2023 | wanted |
| 1.13.5 | April 5, 2023 | wanted |
| 1.14.0 | April 27, 2023 | wanted |
| 1.14.1 | May 18, 2023 | wanted |
| 1.14.2 | June 8, 2023 | wanted |
| 1.14.3 | June 23, 2023 | wanted |
| 1.14.4 | July 13, 2023 | wanted |
| 1.14.5 (build 582369) | 2023 | **archived** |
| 1.14.6 (build 615145) | December 14, 2023 | **archived** |
| 1.14.7 (build 647472) | April 9, 2024 | **archived** |
| 2.1.0 (build 681846) | December 2024 | wanted |
| 2.1.1 (build 682875) | December 2024 | **archived** |

Any build not listed here is wanted too, including Mac builds if they exist.

## Verifying a manifest

Each manifest's SHA-1 is listed above and in [SHA1SUMS.txt](SHA1SUMS.txt). The hashes for 1.14.7 and 2.1.1 match the ones [egdata.app](https://egdata.app) recorded independently for those builds.

On Windows:

```
certutil -hashfile Dauntless_1.6.2.271281.manifest SHA1
```

## Contact

Open an issue, or message the maintainer on Discord: `addison2k`

## Credits

- 1.5.4: Navjit Singh
- 1.6.2 and 2.1.1: from the maintainer's own installs
- 1.12.0: zgh95
- 1.14.5: Santi
- 1.14.6: Khodex
- 1.14.7: Moyka009
- Tooling: [meszmate/manifest](https://github.com/meszmate/manifest), with a [fix for uncompressed chunks](TOOL.md)
- Inspired by [VastBlast/FortniteManifestArchive](https://github.com/VastBlast/FortniteManifestArchive)

Contributors are credited here by whatever name they choose, or kept anonymous on request.

## Disclaimer

This is an unofficial fan preservation project. It is not affiliated with or endorsed by Phoenix Labs, Epic Games or any current rights holder. Dauntless and its content belong to their respective owners. The manifests here are metadata describing files on Epic's servers. Rights holders who want something removed can open an issue and it will be taken down.

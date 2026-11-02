# Pomegr releases

Pomegr is a local-first observer for coding-agent sessions. It runs on your computer and
shows what your Claude Code and Codex sessions are doing.

This repository is where you download Pomegr and ask for help. It holds no Pomegr
application source.

## Download

Get the latest version from
[Releases](https://github.com/Lecarvalho/pomegr-dev/releases). Pomegr runs on
Windows x64. Each release has two builds:

| File | Use it when |
| --- | --- |
| `Pomegr-Setup-<version>-x64.exe` | You want Pomegr installed, with automatic updates. You do not need administrator rights |
| `Pomegr-Portable-<version>-x64.exe` | You want to run Pomegr from a folder. It does not update by itself |

Releases start at version 0.10.0. Older versions are not published here. Pomegr 0.9.0 and
earlier cannot update to these releases by themselves, so install the new version by hand.

The guide is at [pomegr.com/docs](https://pomegr.com/docs).

## Check your download

Each release lists the SHA-256 of its files in `SHA256SUMS.txt`. In PowerShell:

```powershell
Get-FileHash .\Pomegr-Setup-<version>-x64.exe -Algorithm SHA256
```

Compare the result with the line for that file. Windows should also show the publisher as
DSNK Technologie Inc.

## What a release holds

- The signed installer, the signed portable build, and the files that automatic updates read.
- Release notes, `SHA256SUMS.txt`, and the legal files that apply to that version.
- Source archives for the third-party components whose licenses ask for them.

GitHub adds two "Source code" archives to every release by itself. They hold only the files
of this repository, such as this page.

## Get help

Read [SUPPORT.md](SUPPORT.md), or write to [contact@pomegr.com](mailto:contact@pomegr.com).
Report a security problem as [SECURITY.md](SECURITY.md) describes.

[LEGAL.md](LEGAL.md) says which terms apply.

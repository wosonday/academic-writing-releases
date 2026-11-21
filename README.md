# Academic Writing Assistant

Academic Writing Assistant is a Microsoft Word add-in that helps you write academic
English. It checks grammar, vocabulary and coherence on your own computer. It can also
ask an AI provider for a second opinion when you choose to send a paragraph.

This repository hosts the released installers only. The source code is private.

## Requirements

- Windows 10 or Windows 11 (64-bit)
- Microsoft Word 2016 or later, or Microsoft 365 desktop
- About 8 GB of free disk space for a full install (about 2.5 GB without CoEdIT)

You do not need administrator rights, winget, Java or Python. The installer downloads
what it needs into your user profile and checks every program it downloads against a
SHA-256 checksum before using it.

## Install

1. Download `awctl.exe` from the latest release:
   https://github.com/wosonday/academic-writing-releases/releases/latest
2. Open PowerShell in the download folder and run:

   ```powershell
   .\awctl.exe install
   ```

   The installer first prints a plan with the download size of each part and the free
   space on your drive. Then it downloads the app, creates a localhost certificate, trusts
   it for the current user, registers the add-in with Word and installs every component:

   - `lt`: LanguageTool local grammar server (Java is downloaded for you, about 300 MB)
   - `mt`: local translation engine for English and Vietnamese (about 1 GB)
   - `quality`: Hy-MT2 quality translation engine with llama.cpp (about 1.2 GB)
   - `coedit`: CoEdIT editing model (Python, PyTorch and the model, about 5 GB)

   Parts that are already installed are skipped, so you can run the same command again
   after a failed or interrupted download. If one part fails, the others still install
   and a summary at the end shows what needs attention.
3. To leave parts out, use `--skip` (comma separated). For example, on a laptop with a
   small disk or a slow connection:

   ```powershell
   .\awctl.exe install --skip coedit
   ```

   To install only some parts, use `--with lt,mt` instead.
4. Open **Academic Writing** from the Start Menu or the Desktop. This starts the local
   services and opens the status page. Then open Word and use the "Open Assistant" button
   on the Home tab.

The installer also places `aw.exe` in the install folder
(`%LOCALAPPDATA%\academic-writing\app`). The shortcut runs `aw.exe`, and you can run it
from PowerShell as well.

## Check the installation

```powershell
.\awctl.exe doctor
```

`doctor` reports the backend, the certificate, the Word registration and each component.
The status page is at https://localhost:3100 when the app is running.

## Update

Download the new `awctl.exe` and run `.\awctl.exe install` again. Your settings, personal
dictionary and downloaded models are kept.

## Uninstall

```powershell
.\awctl.exe uninstall --purge
```

This removes the Word registration, the Start Menu and Desktop shortcuts, the install
folder and the localhost certificate for the current user.

## SmartScreen note

The executables are not code signed. Windows SmartScreen may show "Windows protected your
PC" on the first run. Choose "More info" and then "Run anyway". Each release includes
`SHA256SUMS.txt` so you can verify your download.

## Privacy

Everything runs on your computer. Your documents are not uploaded anywhere. A paragraph
leaves your machine only when you preview a request, confirm it and have configured an AI
provider in the settings. Local checks, spelling, writing rules and the local translation
engines work offline. The installer contacts only the download sites of the components
(GitHub, Adoptium, python.org, LanguageTool and Hugging Face).

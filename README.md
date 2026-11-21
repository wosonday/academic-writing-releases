# Academic Writing Assistant

Academic Writing Assistant is a Microsoft Word add-in that helps you write academic
English. It checks grammar, vocabulary and coherence on your own computer. It can also
ask an AI provider for a second opinion when you choose to send a paragraph.

This repository hosts the released installers only. The source code is private.

## Requirements

- Windows 10 or Windows 11 (64-bit)
- Microsoft Word 2016 or later, or Microsoft 365 desktop

## Install

1. Download `awctl.exe` from the latest release:
   https://github.com/wosonday/academic-writing-releases/releases/latest
2. Open PowerShell in the download folder and run:

   ```powershell
   .\awctl.exe install
   ```

   The installer downloads the payload, creates a localhost certificate, trusts it for the
   current user and registers the add-in with Word.
3. Optional components (comma separated):

   ```powershell
   .\awctl.exe install --with lt,coedit,mt,quality
   ```

   `coedit` needs Python and downloads about 3 GB on first use, so leave it out if you only
   need the local grammar and translation engines.

   - `lt`: LanguageTool local grammar server
   - `coedit`: CoEdIT editing model (Python, about 3 GB on first use)
   - `mt`: local translation engine for English and Vietnamese
   - `quality`: Hy-MT2 quality translation engine
4. Open Word and use the "Open Assistant" button on the Home tab.

Run `.\awctl.exe` (or `awctl.exe` from the install folder) to start the backend and open the
status page.

## Check the installation

```powershell
.\awctl.exe doctor
```

`doctor` reports the backend, the certificate, the Word registration and the optional
components. The status page is at https://localhost:3100 when the backend is running.

## Uninstall

```powershell
.\awctl.exe uninstall --purge
```

This removes the Word registration, deletes the install folder and removes the localhost
certificate for the current user.

## SmartScreen note

The executables are not code signed. Windows SmartScreen may show "Windows protected your
PC" on the first run. Choose "More info" and then "Run anyway". Each release includes
`SHA256SUMS.txt` so you can verify your download.

## Privacy

Everything runs on your computer. Your documents are not uploaded anywhere. A paragraph
leaves your machine only when you preview a request, confirm it and have configured an AI
provider in the settings. Local checks, spelling, writing rules and the local translation
engine work offline.

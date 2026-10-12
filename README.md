# Sonic Search

A very fast file search tool for NTFS drives on Windows. It reads the NTFS **Master File Table** directly, so it can index a whole drive in seconds, then search it as you type. It can also search *inside* files, including PDF, Word and Excel documents.

![Main search window](screenshots/main-search.png)

## Download

**[Download the latest version](../../releases/latest)** (Windows 10/11, about 3 MB)

1. Open the link above and download `SonicSearch-win.zip`.
2. Unzip it anywhere and run `SonicSearch.exe`.
3. Accept the administrator prompt (reading the drive's file table needs it; see [Running without admin rights](#running-without-admin-rights)).

> The app is not code-signed yet, so Windows SmartScreen may show a warning the first time. Choose **More info**, then **Run anyway**.

> The first release is still being prepared; until it appears under [Releases](../../releases), there is nothing to download yet.

## What it does

- **Instant search.** Results follow your typing. Matching modes: Contains, Starts with, Exact, Regex, and `*` / `?` wildcards.
- **Qualifiers you can combine:**

  | Qualifier | Example | Meaning |
  |---|---|---|
  | `ext:` / `type:` | `ext:pdf` | by file type |
  | `file:` / `folder:` | `folder: backup` | files only / folders only |
  | `size:` | `size:>10mb` | by size |
  | `date:` / `modified:` | `date:>7d` | by modified date |
  | `location:` / `path:` / `in:` | `location:desktop` | inside a folder (or a well-known name such as Desktop, Downloads) |
  | `drive:` | `drive:d` | one drive only |
  | `content:` / `text:` | `content:"invoice 2024"` | search inside files |

- **Full-text search in files.** Optional. Choose folders in Settings and Sonic Search builds a persistent index of the text in them, including **PDF, Word, Excel, PowerPoint and OpenDocument** files. `content:` searches then answer in milliseconds, ranked by relevance. When a file changes, it is re-read automatically.
- **Preview window (F3).** Source code with syntax highlighting, rendered Markdown, PDF and Office pages, with your search words highlighted and F3 / Shift+F3 to jump between matches.
- **Live updates.** New, renamed and deleted files appear in the results within a fraction of a second, with no rescan.
- **Several drives.** Pick which drives to index.
- **Favorites** in groups, with their own search box, and drag-and-drop from Explorer.
- **Most-opened first.** Files you open often rise to the top of the results.
- **Also finds** Windows tools (Device Manager, Services, ...) and Chrome / Edge bookmarks.
- **Global hotkey** (default Ctrl+S) to summon the window from anywhere; it lives in the system tray and can start with Windows.

![Settings](screenshots/settings.png)

## Requirements

- Windows 10 or 11 with an NTFS-formatted drive.
- .NET Framework 4.8 (already part of current Windows 10 / 11).
- Administrator rights, as described below.

## Running without admin rights

Reading the raw NTFS volume needs elevation, so by default Sonic Search asks for it at start. If you would rather not run the window elevated (an elevated window cannot accept drag-and-drop from Explorer), turn on **Settings > Indexing > Enable background indexing service**. That installs a small Windows service once (one admin prompt) which does the privileged reading; after that the window itself runs normally.

## Known limits

- NTFS drives only (FAT / exFAT / network shares are not indexed).
- Full-text search reads files through the Windows text extractors installed on your PC; scanned PDFs that contain only images have no text to find.
- Not code-signed yet.

## Source code

The source code is not public. This repository only hosts the downloads and release notes.

Sonic Search includes `NtfsReader`, derived from Jeroen Kessels' JkDefrag and used under the LGPL-2.1.

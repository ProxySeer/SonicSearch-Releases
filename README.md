# Sonic Search

A very fast file search tool for NTFS drives on Windows. It reads the NTFS **Master File Table** directly, so it can index a whole drive in seconds, then search it as you type. It can also search *inside* files, including PDF, Word and Excel documents - and it does not need administrator rights.

![Main search window](screenshots/main-search.png)

## Download

**[Download the latest version](../../releases/latest)** (Windows 10/11, about 5.5 MB)

1. Open the link above and download `SonicSearch-win.zip`.
2. Unzip it anywhere (keep the whole folder together) and run `SonicSearch.exe`.
3. On the first start, choose how Sonic Search should read your disk (see [Without administrator rights](#without-administrator-rights)). The recommended choice needs one administrator prompt, once.

> The app is not code-signed yet, so Windows SmartScreen or Defender may show a warning the first time. Choose **More info**, then **Run anyway**.

## What it does

- **Instant search.** Results follow your typing. Matching modes: Contains, Starts with, Exact, Regex, and `*` / `?` wildcards.
- **Open the first four results with Ctrl+1 ... Ctrl+4.** The first four rows show the shortcut. Ctrl+0 switches between Search and Favorites.
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

- **Full-text search in files.** Optional. Choose folders in Settings and Sonic Search builds a persistent index of the text in them, including **PDF, Word, Excel, PowerPoint and OpenDocument** files. `content:` searches answer in milliseconds, ranked by relevance. See [How fast is it?](#how-fast-is-it).
- **Preview window (F3).** Source code with syntax highlighting, rendered Markdown, PDF and Office pages, with your search words highlighted and F3 / Shift+F3 to jump between matches.
- **Live updates.** New, renamed and deleted files appear in the results within a fraction of a second, with no rescan. See [Live updates](#live-updates).
- **Several drives.** Pick which drives to index.
- **Favorites** in groups, with their own search box, and drag-and-drop from Explorer.
- **Most-opened first.** Files you open often rise to the top of the results.
- **Also finds** Windows tools (Device Manager, Services, ...) and Chrome / Edge bookmarks.
- **Global hotkey** (default Ctrl+S) to summon the window from anywhere; it lives in the system tray and can start with Windows.

![Settings](screenshots/settings.png)

## How fast is it?

Measured on a 24-core Windows 11 PC with an NVMe SSD and 2.94 million files on `C:`:

| | |
|---|---|
| Search all 2.94 M file names, every match | 17-30 ms |
| The first 50 results (what the window shows) | under 1 ms |
| Start with a saved index | about 1 second |
| First scan of the drive | about 30 s (about 20-45 s without admin rights) |

Full-text search, 58,000 documents (PDF, Office, text), 228 MB index:

| | |
|---|---|
| `content:` a word in a few files | about 7 ms |
| `content:` a word in about 900 files, with snippets | about 100 ms |
| `content:` a very common word (over 2,000 files), with snippets | about 200 ms |
| Open the saved index | under a second |
| Build the index from scratch | about 3 minutes, in the background at low priority |
| Keep it up to date | a changed file is re-read within seconds; unchanged files are never re-read |

Fewer cores means proportionally slower scans; typing stays instant either way. Memory use is about 250-300 MB for 2.9 million files, most of it reclaimable.

## Live updates

| How the disk is read | What reports a change | When Sonic Search was closed |
|---|---|---|
| Indexing service, or Administrator | the NTFS change journal, within about 100 ms; the whole drive, renames and moves exact | changes are replayed at the next start |
| Without administrator rights | a file watcher on every scanned folder, applied once a second | folder times are compared at the next start (a few seconds, in the background) |

The full-text index follows the file list: a changed document is re-read within seconds, and the work pauses while you type.

## Without administrator rights

Reading the raw NTFS volume needs elevation, so Sonic Search offers three ways to read your disk on the first start (and **Settings > Indexing > Choose again...** later):

1. **Install the indexing service (recommended).** One administrator prompt, once. A small Windows service does the privileged reading; the window itself runs normally and accepts drag-and-drop from Explorer. Everything on the drive is found.
2. **Run as Administrator every time.** Nothing is installed; you get a UAC prompt at every start.
3. **Search without administrator rights.** No prompt, nothing installed: Sonic Search lists your folders itself. The first scan takes a minute or two and later starts are instant (the check against the disk runs in the background). Folders your account cannot open - other users' profiles, system areas - are left out, and the status line says how many. Changes made while Sonic Search was closed are picked up at the next start.

## Requirements

- Windows 10 or 11 with an NTFS-formatted drive.
- .NET Framework 4.8 (already part of current Windows 10 / 11).

## Known limits

- NTFS drives only (FAT / exFAT / network shares are not indexed).
- Without administrator rights or the service, files in folders your account cannot open are not found.
- Full-text search reads PDF, Word, Excel, PowerPoint, OpenDocument and RTF files itself - nothing needs to be installed. Scanned PDFs that contain only images have no text to find, encrypted files can't be read, and `.msg` e-mails depend on an installed filter.
- Not code-signed yet.

## Source code

The source code is not public. This repository only hosts the downloads and release notes.

Sonic Search includes `NtfsReader`, derived from Jeroen Kessels' JkDefrag and used under the LGPL-2.1.

First public build of Sonic Search.

**Highlights**
- Reads the NTFS Master File Table directly: a drive is indexed in seconds and searched as you type.
- Compact in-memory index (about 85 bytes per file in tests) with live updates from the USN journal.
- `content:` search inside files, with an optional persistent full-text index that also covers PDF, Word, Excel, PowerPoint and OpenDocument files in the folders you choose.
- Preview window (F3) for code, Markdown, PDF and Office files, with matches highlighted.
- Qualifiers: `ext:`, `size:`, `date:`, `location:`, `drive:`, `file:` / `folder:`, `content:`.
- Optional background indexing service, so the window can run without administrator rights.

**Install:** unzip `SonicSearch-win.zip` anywhere and run `SonicSearch.exe`. Requires Windows 10/11 with an NTFS drive and .NET Framework 4.8. Not code-signed, so SmartScreen may warn on first run (More info, then Run anyway).

> **Repository setup in progress:** The v5 application source files are not yet uploaded. Download the source ZIP from the accompanying ChatGPT conversation, extract it, and upload the files to this repository before attempting installation. This repository currently contains documentation and CI scaffolding only.

# Paper Duplicate Cleaner for Windows

A local-first Windows utility for finding duplicate research PDFs and other repeated files, even when filenames differ. Includes a desktop review tool and a scheduled, resumable background scanner.

> **Status:** v5 source release. The scheduled scanner and Windows power-management features require real Windows 10/11 validation. This is **not** a signed executable or installer.

## Features

- SHA-256 comparison for byte-identical files with different names or in different folders/drives.
- Optional PDF extracted-text matching (via PyMuPDF). Text-identical PDFs can still have changed figures and other content; review manually.
- Manual **Run Now** scans and configurable background windows (2:00–7:00 AM and/or 11:00 AM–5:00 PM, local PC time).
- SQLite checkpoints for resuming scanning across permitted windows.
- Windows idle-sleep prevention while scans are active (monitor may still turn off).
- Per-user cross-process lock to prevent overlapping manual and scheduled scans.
- CSV reports under `Documents\\PaperDuplicateCleanerReports`.
- Scanning is read-only; no automatic file deletion.

## Install

1. Install Python 3 on Windows, ensuring `python` or the `py` launcher is available on PATH.
2. Download/clone this repository and keep it in a **permanent** directory, e.g. `D:\\Utilities\\PaperDuplicateCleaner`.
3. Optionally double-click `Install_PDF_Text_Matching.bat` to install PyMuPDF.
4. Double-click `Configure_Background_Scan.bat` to select scan folders and scheduled windows.
5. Choose **Run Now** to start immediately, or **Save and install Windows schedule** for unattended runs.
6. Open `Run_on_Windows.bat` for interactive review of duplicate groups.

You can also double-click `Run_Scan_Now.bat` after saving your settings.

## Important safety notes

- **Do not automatically delete files based on PDF text similarity.** Images, figures, equations, annotations, and formatting can differ despite identical extracted text.
- Full-drive scans may encounter protected OS paths and take hours or days. System directories are excluded by default in the scheduled scanner.
- The PC must be on. Task Scheduler does not turn on a powered-off machine; closing a laptop lid, hibernation, restarts, and power failure can still interrupt work.
- Configure the scheduled tasks under your user profile and keep the machine logged in (locking the screen is fine).
- The lock protects cooperating v3/v4/v5 instances under the same user; it does not coordinate unrelated cleanup programs.
- A checkpoint does not provide an immutable snapshot of files that continue changing while scanning.

## Storage

- Checkpoints: `%LOCALAPPDATA%\\PaperDuplicateCleaner\\resume.sqlite3`
- Scan status: `%LOCALAPPDATA%\\PaperDuplicateCleaner\\status.json`
- Reports: `%USERPROFILE%\\Documents\\PaperDuplicateCleanerReports`

## Source files

| File | Purpose |
| --- | --- |
| `PaperDuplicateCleaner.py` | Interactive desktop duplicate browser |
| `configure_background.py` | Scanner configuration interface |
| `background_scan.py` | Checkpointed scheduled/manual scan engine |
| `scanner_lock.py` | Cross-process single-instance protection |
| `Install_Background_Tasks.ps1` | Task Scheduler registration |
| `Remove_Background_Tasks.ps1` | Remove scheduled tasks |
| `*.bat` | Windows launch helpers |

See `README.txt` and `UPGRADE_FROM_V3_V4.txt` for full operational details and migration guidance.

## Limitations

- No semantic or fuzzy similarity matching of different revisions.
- Windows Task Scheduler integration and sleep behavior still need real-machine acceptance tests.
- Python is required; this repository does not ship a standalone `.exe`.

## License

No open-source license has been selected yet. A license should be added by the repository owner before inviting public reuse or contributions.
# DescriptEdit

Total Commander Lister plugin (WLX). Instead of a file preview, it shows
TC's own comment from `descript.ion` (in the same folder as the file/folder).

# Usage

| Key        | Action                                                                      |
| ---------- | --------------------------------------------------------------------------- |
| **Ctrl+S** | Saves the comment to `descript.ion` and closes the Lister window afterwards |
| **Ctrl+E** | Toggles editable/read-only for this session (does not change the INI)       |
| **ESC**    | Closes the Lister window (without saving)                                   |
| Typing     | Works as usual - the comment field is a RichEdit control                    |

There is **no auto-save** - only Ctrl+S writes anything. If the object
doesn't have a `descript.ion` entry yet, saving automatically creates one.
If you clear the text completely and save, `SetTCComment()` removes the
entry again.

When edit mode is active (either from the INI on open, or toggled with
Ctrl+E), three asterisks (`***`) appear at the front of the Lister window
title as an indicator. If they're gone, the field is read-only.

## Timestamp

On save, `" (HH:MM:SS | dd.MM.yyyy)"` (the save time, **not** the creation
time) is optionally appended right after the comment and saved into `descript.ion` along with it.

## Character limit

Hard-capped at 2000 characters (`EM_SETLIMITTEXT`) - you technically
cannot type or paste beyond that. The counter at the bottom shows the
current count live, updating as you type.

## INI settings

The settings file must be named **exactly like the DLL** and sit in the
same folder (`DescriptEdit.wlx64` → `DescriptEdit.ini`).

```ini
[DescriptEdit]
AppendTimestamp=1
Editmodus=1
HighlightColor=34,87,128
```

Changes to the INI take effect immediately the next time the Lister window
is opened - no need to restart TC.

- **AppendTimestamp** (`0`/`1`, default `1`): whether to append the timestamp on save
- **Editmodus** (`0`/`1`, default `1`): initial editable/read-only state. Can additionally be toggled per session with Ctrl+E, independent of the INI value
- **HighlightColor** (`R,G,B`, e.g. `34,87,128`): background color of the title and counter bar. Text color (black/white) is chosen automatically based on brightness, so it stays readable. **If this key is missing entirely**, no custom color is applied - it falls back to the normal system/window color

## Setting up in Total Commander

Add it manually under *Configuration → Options → Lister (F3)... → Plugins*
(point it to the `.wlx`/`.wlx64` path, set priority). The detect string is
`EXT="*" | EXT=""`, so it applies to both files and folders.



**Opening directly via button/CLI**

TC can also be launched directly with this plugin from the command line or
a button bar, without pressing F3 first:

```
%Commander_Path%\TOTALCMD64.EXE /S=L:Pdescriptedit "%P%N"
```

`%P%N` is path + name of the currently selected file/folder in the panel.

Ready-made button (right-click the button bar → Insert button):

```
TOTALCMD#BAR#DATA
%Commander_Path%\TOTALCMD64.EXE
/S=L:Pdescriptedit "%P%N"
%Commander_Path%\Icons\sb.ico
Opens the comment field in the Lister
-1
```

## Build

Requires WinLibs MinGW-w64 under `C:\mingw64`. For the 32-bit build,
additionally `i686-w64-mingw32-g++` (e.g. under `C:\mingw32`) - if the
compiler isn't found, `build.bat` skips that step automatically.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

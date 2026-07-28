# DescriptEdit

Total Commander Lister plugin (WLX). Instead of a file preview, it shows
TC's own comment from `descript.ion` (in the same folder as the file/folder).


## Usage

| Key        | Action                                                                                 |
| ---------- | -------------------------------------------------------------------------------------- |
| **Ctrl+S** | Saves the comment to `descript.ion`                                                    |
| **Ctrl+E** | Toggles editable/read-only for this session (does not change the INI)                  |
| **ESC**    | Closes the Lister window without saving - see safety note below                        |
| Typing     | Works as usual - the comment field is a RichEdit control with slightly rounded corners |

There is **no auto-save** - only Ctrl+S writes anything. If the object
doesn't have a `descript.ion` entry yet, saving automatically creates one.
If you clear the text completely and save, `SetTCComment()` removes the
entry again.

When edit mode is active (either from the INI on open, or toggled with
Ctrl+E), the comment field's background is a touch darker (light grey
instead of white) as an indicator. In read-only mode it's white. Both
colors are configurable via the INI (see below).

A status line at the bottom shows how many objects in the current folder
have a comment, how many don't, and the file size of `descript.ion` itself.
Recalculated on open and after each successful save (not on every
keystroke, to avoid noticeable slowdowns in large folders).

## Timestamp

On save, `" (HH:MM:SS | dd.MM.yyyy)"` (the save time, **not** the creation
time) is optionally appended right after the comment at half font size and
saved into `descript.ion` along with it.

## Character limit

Hard-capped at 2000 characters (`EM_SETLIMITTEXT`) - you technically
cannot type or paste beyond that. The counter at the bottom shows the
current count live, updating as you type.

## INI settings

The settings file must be named **exactly like the DLL** and sit in the
same folder (`DescriptEdit.wlx64` → `DescriptEdit.ini`).

```ini
[DescriptEdit]
; Behavior
AppendTimestamp=1
Editmodus=1

; Font
FontFace=Segoe UI
FontSize=11
TitleFontSize=12

; Colors (R,G,B)
HighlightColor=34,87,128
EditableBackgroundColor=236,236,236
ReadonlyBackgroundColor=255,255,255
```

Changes to the INI take effect immediately the next time the Lister window
is opened - no need to restart TC.

**Behavior:**

- **AppendTimestamp** (`0`/`1`, default `1`): whether to append the timestamp on save
- **Editmodus** (`0`/`1`, default `1`): initial editable/read-only state. Can additionally be toggled per session with Ctrl+E, independent of the INI value

**Font:**

- **FontFace** (default `Segoe UI`): font used for the comment field, title, and counter
- **FontSize** (default `11`, range 6–72): font size of the comment field in points. The timestamp is automatically shown at half that size
- **TitleFontSize** (default `FontSize + 1`, range 6–72): font size of the title ("Descript") and the counter

**Colors (each `R,G,B`, values 0–255):**

- **HighlightColor** (e.g. `34,87,128`): background color of the title and counter bar. Text color (black/white) is chosen automatically based on brightness. **If this key is missing entirely**, the normal system/window color is used
- **EditableBackgroundColor** (default `236,236,236`): background color of the comment field in edit mode
- **ReadonlyBackgroundColor** (default `255,255,255`): background color of the comment field in read-only mode


https://github.com/Native2904/DescriptEdit/blob/main/QuickView.png  https://github.com/Native2904/DescriptEdit/blob/main/Lister.png


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

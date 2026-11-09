# DescriptEdit

Total Commander Lister plugin (WLX). Shows TC's own file comment from
`descript.ion` (in the same folder as the file or folder) and lets you edit
it right there – in Lister (F3) and in Quick View (Ctrl+Q).

## Screenshots

<img width="961" height="843" alt="2026-11-09_194335" src="https://github.com/user-attachments/assets/b1b34cf4-9c85-48c7-ab35-c6d2f2764fc0" /> <img width="979" height="704" alt="2026-11-09_194413" src="https://github.com/user-attachments/assets/95ec0d29-6689-4392-a84e-0a4eacc1b555" />


## Usage

| Key | Action |
| --- | --- |
| **Ctrl+S** | Save the comment (does not close anything). "Saved" briefly appears at the bottom |
| **Ctrl+E** | Toggle editable / read-only for this session (does not change the INI) |
| **ESC** | Close the Lister window without saving (no effect in Quick View) |
| **N / P** | Read-only mode: next / previous file – the way TC users expect (see "Browsing") |
| **Shift+N / P** | Read-only mode: jump to the next / previous object **with a comment** |
| **Alt+→ / Alt+←** | Both modes: next / previous object in the folder (with Shift: only with a comment) |
| **0–9** | Read-only mode: plugin switching as usual in TC. Edit mode: normal typing |
| **F7 / F3** | Search / search again in the comment (also via the Lister menu) |
| Lister menu | "Copy" and "Select all" work on the comment field |

There is **no auto-save**; only Ctrl+S writes anything. If the object has no
entry yet, saving creates one. If you clear the text and save, the entry is
removed. If it was the last entry, `descript.ion` is deleted, just like TC
itself does.

The slim status line at the bottom shows on the left how many objects in the
folder have a comment and how many don't, plus the size of `descript.ion`;
on the right the character count (or "No comment"). Entries for files that
no longer exist are not counted.

## When does DescriptEdit appear?

DescriptEdit decides this itself **when opening** (F3 or Ctrl+Q),
separately for the Lister window and Quick View:

| Value | Behavior when opening |
| --- | --- |
| `commented` | only if a comment exists – otherwise the normal viewer or the next plugin is used |
| `force` | only on explicit request: **key 4** (Image/Multimedia) in Lister |
| `always` | for every file and folder |

Default: `LoadInLister=commented`, `LoadInQuickView=always`.
**Key 4 loads DescriptEdit** – that's how you add new comments. This holds
as long as no other plugin that accepts every file is listed **before**
DescriptEdit (see "order" below).

**While browsing, DescriptEdit stays loaded** – also for files without a
comment; the status line then says "No comment". The reason: while browsing
(N/P, cursor in Quick View) TC only asks the currently active plugin. If
DescriptEdit handed a file without a comment over to another plugin, it
would never come back – a plugin that accepts every file never hands back.

Once another plugin has taken over, it also stays while browsing. To get back
to DescriptEdit: key 4 (if DescriptEdit is listed before that plugin), or
close and reopen Quick View with Ctrl+Q twice – TC then asks the plugins from
the top again.

Example: F3 should show the content, Quick View the comments:

```ini
LoadInLister=force
LoadInQuickView=always
```

**Important – detect string and order:** For this to work, the detect string
must be `EXT="*" | EXT=""`, and DescriptEdit should be **near the top** of
the Lister plugin list – in any case **before** plugins that display every
file (e.g. HTML/Edge viewers, Universal Viewer). It is then asked first and only takes over when it
has something to show. If you changed the detect string to `FORCE & (...)`
earlier, please change it back: according to the Lister plugin guide, TC only
calls the plugin while browsing if the detect string is true – with `FORCE`
it wouldn't be.

## Browsing

- **Several files selected, F3:** N/P is handled by **TC itself** – with the
  arrows and the counter at the top right, in your sort order and selection.
  DescriptEdit simply shows the next comment.
- **Only one file opened:** TC can't browse then (it just reloads the same
  file). DescriptEdit notices this and goes through the whole folder itself: folders first,
  then by name, digits as numbers ("File2" before "File10"). The plugin does
  not know TC's panel sort order.
- **Shift+N/P** and **Alt+arrows** always use DescriptEdit's own folder list.

If there are unsaved changes, DescriptEdit asks before switching: **Yes**
saves, **No** discards, **Cancel** stays (when TC itself switches, only
Yes/No).

## Dark mode

DescriptEdit follows TC's dark mode automatically – also when you switch
while it is open. On Windows 10/11 the scroll bars turn dark too. The INI can
force the mode (`DarkMode=on/off`) and change the dark background colors.

## Character sets: DOS, ANSI, Unicode

`descript.ion` files come in different encodings. TC can write comments in
the Windows character set (ANSI), in the old **DOS character set** (checkbox
in *Configuration → Options → Operation → File comments*) or as Unicode
(UTF-8/UTF-16).

DescriptEdit detects the encoding itself. For single-byte files it tells DOS
and ANSI apart by the umlauts: "ä" is byte `84` in the DOS table and `E4` in
the Windows table. When saving, **the file keeps its encoding** so TC still
reads it correctly. Single lines that another program wrote in the other
table are shown correctly and converted on the next save.

If a character doesn't fit into the single-byte table (e.g. Chinese), the
file is saved as UTF-8 instead of losing characters.

To override detection (e.g. for Russian DOS files): `Encoding=dos` or
`Encoding=ansi`. New files are created as UTF-16 by default, or in the
encoding given by `Encoding=dos/ansi/utf8`.

## Multi-line comments – compatible with TC (Ctrl+Z)

Total Commander stores line breaks as `\n` and marks such comments at the end
of the line with the bytes `04 C2`. DescriptEdit reads and writes exactly this
format – in all encodings. Multi-line comments look the same in TC (Ctrl+Z,
tooltip) and in DescriptEdit.

Older DescriptEdit versions (1.0/1.1) wrote line breaks without the marker.
With `LegacyLineBreaks=1` these are still shown as line breaks and converted
to TC format on the next save.

## What happens on save

- The **encoding** is kept (see above).
- The **Hidden / System / Read-only** attributes are kept.
- DescriptEdit first writes a temporary file and then swaps it in. If writing
  fails (e.g. disk full), the old `descript.ion` stays intact.
- Extra data written by other programs on a line is preserved.
- If saving fails, a message with the Windows error text appears.

## Timestamp and character limit

On save, ` (HH:MM:SS | dd.MM.yyyy)` can be appended in half font size; an old
timestamp is replaced, not doubled. A comment can be up to 2000 characters
long; the counter at the bottom shows the current count.

## INI settings

The INI must have **the same name as the DLL** and be in the same folder
(`DescriptEdit.wlx64` → `DescriptEdit.ini`). Changes take effect the next
time the plugin opens, no TC restart needed.

```ini
[DescriptEdit]
; When does DescriptEdit appear? commented / force / always
LoadInLister=commented
LoadInQuickView=always

; Behavior
AppendTimestamp=1
EditMode=1
Language=auto
Encoding=auto
LegacyLineBreaks=1

; Font
FontFace=Segoe UI
FontSize=11
TitleFontSize=12

; Colors (R,G,B)
HighlightColor=34,87,128
EditableBackgroundColor=236,236,236
ReadonlyBackgroundColor=255,255,255
DarkMode=auto
DarkEditableBackgroundColor=52,52,58
DarkReadonlyBackgroundColor=38,38,38
```

| Key | Values (default) | Effect |
| --- | --- | --- |
| LoadInLister | `commented`/`force`/`always` (`commented`) | Whether DescriptEdit appears when opening the Lister window (F3) |
| LoadInQuickView | `commented`/`force`/`always` (`always`) | Whether DescriptEdit appears when opening Quick View (Ctrl+Q) |
| AppendTimestamp | `0`/`1` (`1`) | Append timestamp on save |
| EditMode | `0`/`1` (`1`) | Start editable/read-only; Ctrl+E also toggles per session (old name `Editmodus` is still read) |
| Language | `auto`/`de`/`en` (`auto`) | Language of the status line and messages; `auto` follows the Windows display language |
| Encoding | `auto`/`dos`/`ansi`/`utf8`/`utf16` (`auto`) | Detect or set the encoding (see "Character sets") |
| LegacyLineBreaks | `0`/`1` (`1`) | Show `\n` without TC marker (old DescriptEdit comments) as a line break |
| FontFace | name (`Segoe UI`) | Font for comment field and header bar |
| FontSize | 6–72 (`11`) | Comment field font size in points; the timestamp is shown at half size |
| TitleFontSize | 6–72 (`FontSize + 1`) | Font size of the header bar |
| HighlightColor | `R,G,B` (unset) | Color of the header bar; text color black/white chosen automatically |
| EditableBackgroundColor | `R,G,B` (`236,236,236`) | Background in edit mode (light) |
| ReadonlyBackgroundColor | `R,G,B` (`255,255,255`) | Background in read-only mode (light) |
| DarkMode | `auto`/`on`/`off` (`auto`) | Dark mode: follow TC or force on/off |
| DarkEditableBackgroundColor | `R,G,B` (`52,52,58`) | Background in edit mode (dark) |
| DarkReadonlyBackgroundColor | `R,G,B` (`38,38,38`) | Background in read-only mode (dark) |

## Setup in Total Commander

Easiest: open the ZIP in TC; installation runs automatically via
`pluginst.inf`. Then move DescriptEdit **near the top** in *Configuration →
Options → Plugins → Lister (WLX)*. Detect string: `EXT="*" | EXT=""` (files
and folders).

**Open directly from a button** (no F3 needed):

```
TOTALCMD#BAR#DATA
%COMMANDER_EXE%
/S=L:Pdescriptedit "%P%N"
%COMMANDER_PATH%\Icons\sb.ico
Opens the comment field in Lister
-1
```

## Building

`build.bat` builds the normal version, `build_debug.bat` a version that
writes a log file (`DescriptEdit_debug.log`) for troubleshooting – the normal
version contains none of that code.

Important: use a MinGW-w64 toolchain with thread model **win32** (`g++ -v`
shows "Thread model: win32"). The posix variant embeds library parts when
linking statically that some antivirus scanners wrongly flag.

## License

MIT – see LICENSE.

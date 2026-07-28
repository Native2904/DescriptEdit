# DescriptEdit

Total Commander Lister-Plugin (WLX). Zeigt statt eines Datei-Previews den
TC-eigenen Kommentar aus `descript.ion` (im selben Ordner wie die
Datei/der Ordner).

## Screenshots

![DescriptEdit im Vollbild](https://github.com/Native2904/DescriptEdit/blob/main/Lister.png)
![DescriptEdit im Lister-Fenster](https://github.com/Native2904/DescriptEdit/blob/main/QuickView.png)

## Bedienung

| Taste      | Wirkung                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------- |
| **Strg+S** | Speichert den Kommentar in `descript.ion`                                                    |
| **Strg+E** | Schaltet für diese Sitzung zwischen editierbar/schreibgeschützt um (verändert nicht die INI) |
| **ESC**    | Schließt das Lister-Fenster ohne zu speichern - siehe Sicherheitshinweis unten               |
| Tippen     | Ganz normal - Kommentarfeld ist ein RichEdit-Control mit leicht abgerundeten Ecken           |

Es gibt **kein Auto-Save** – nur Strg+S schreibt etwas. Ist für das Objekt
noch kein `descript.ion`-Eintrag vorhanden, legt das Speichern automatisch
einen neuen an. Wird der komplette Text gelöscht und gespeichert, entfernt
`SetTCComment()` den Eintrag wieder.

Ist der Editiermodus aktiv (beim Öffnen laut INI oder per Strg+E
umgeschaltet), ist der Hintergrund des Kommentarfelds ein Tick dunkler
(hellgrau statt weiß) als Kennzeichen. Im Lesemodus ist es weiß. Beide
Farbtöne sind über die INI einstellbar (siehe unten).

Unten im Fenster zeigt eine Statistik-Zeile, wie viele Objekte im aktuellen
Ordner einen Kommentar haben, wie viele nicht, sowie die Dateigröße der
`descript.ion` selbst. Wird beim Öffnen und nach jedem erfolgreichen
Speichern neu berechnet.

## Zeitstempel

Beim Speichern wird optional `" (HH:MM:SS | dd.MM.yyyy)"` (Speicherzeitpunkt,
**nicht** Erstellungszeitpunkt) in halber Schriftgröße direkt hinter den
Kommentar gehängt und so mit in der `descript.ion` gespeichert.

## Zeichenlimit

Hart begrenzt auf 2000 Zeichen (`EM_SETLIMITTEXT`) - man kann technisch
nicht darüber hinaus tippen/einfügen. Der Zähler unten zeigt den aktuellen
Stand live an, auch beim Tippen.

## INI-Einstellungen

Die Einstellungsdatei muss **exakt so heißen wie die DLL** und im selben
Ordner liegen (`DescriptEdit.wlx64` → `DescriptEdit.ini`).

```ini
[DescriptEdit]
; Verhalten
AppendTimestamp=1
Editmodus=1
Language=auto

; Schrift
FontFace=Segoe UI
FontSize=11
TitleFontSize=12

; Farben (R,G,B)
HighlightColor=34,87,128
EditableBackgroundColor=236,236,236
ReadonlyBackgroundColor=255,255,255
```

Änderungen an der INI wirken sofort beim nächsten Öffnen des Lister-Fensters -
kein TC-Neustart nötig.

**Verhalten:**

- **AppendTimestamp** (`0`/`1`, Default `1`): Zeitstempel beim Speichern anhängen oder nicht
- **Editmodus** (`0`/`1`, Default `1`): Startzustand editierbar/schreibgeschützt. Kann pro Sitzung zusätzlich mit Strg+E umgeschaltet werden, unabhängig vom INI-Wert
- **Language** (`auto`/`de`/`en`, Default `auto`): Sprache der Statistik-Zeile unten. 
  
  

**Schrift:**

- **FontFace** (Default `Segoe UI`): Schriftart für Kommentarfeld, Titel und Zähler
- **FontSize** (Default `11`, Bereich 6–72): Schriftgröße des Kommentarfelds in Punkt. Der Zeitstempel wird automatisch mit der halben Größe dargestellt
- **TitleFontSize** (Default `FontSize + 1`, Bereich 6–72): Schriftgröße von Titel ("Descript") und Zähler

**Farben (jeweils `R,G,B`, Werte 0–255):**

- **HighlightColor** (z. B. `34,87,128`): Hintergrundfarbe von Titel- und Zähler-Leiste. Textfarbe (schwarz/weiß) wird automatisch nach Helligkeit gewählt. **Fehlt dieser Schlüssel komplett**, wird die normale System-/Fensterfarbe verwendet
- **EditableBackgroundColor** (Default `236,236,236`): Hintergrundfarbe des Kommentarfelds im Editiermodus
- **ReadonlyBackgroundColor** (Default `255,255,255`): Hintergrundfarbe des Kommentarfelds im Lesemodus

## Einrichtung in Total Commander

Unter *Konfiguration → Optionen → Lister (F3)... → Plugins* manuell
eintragen (Pfad zur `.wlx`/`.wlx64`, Priorität setzen). Detect-String ist
`EXT="*" | EXT=""`, greift also für Dateien und Ordner gleichermaßen.

**Direkt per Button/CLI öffnen**

TC lässt sich auch von der Kommandozeile bzw. per Button-Leiste direkt mit
diesem Plugin starten, ohne erst F3 zu drücken:

```
%Commander_Path%\TOTALCMD64.EXE /S=L:Pdescriptedit "%P%N"
```

`%P%N` steht für Pfad + Name der aktuell markierten Datei/des markierten
Ordners im Panel.

Fertiger Button (Rechtsklick auf die Button-Leiste → Schaltfläche einfügen):

```
TOTALCMD#BAR#DATA
%Commander_Path%\TOTALCMD64.EXE
/S=L:Pdescriptedit "%P%N"
%Commander_Path%\Icons\sb.ico
Öffnet das Kommentarfeld im Lister
-1
```

## Build

Voraussetzung: WinLibs MinGW-w64 unter `C:\mingw64`. Für die
32-Bit-Variante zusätzlich `i686-w64-mingw32-g++` (z. B. unter
`C:\mingw32`) - fehlt der Compiler, überspringt `build.bat` den Schritt
automatisch.

## Lizenz

Dieses Projekt steht unter der MIT-Lizenz - Details siehe LICENSE-Datei.

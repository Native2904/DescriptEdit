# DescriptEdit

Total Commander Lister-Plugin (WLX). Zeigt statt eines Datei-Previews den
TC-eigenen Kommentar aus `descript.ion` (im selben Ordner wie die
Datei/der Ordner).

## Bedienung

| Taste      | Wirkung                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------- |
| **Strg+S** | Speichert den Kommentar in `descript.ion` und schließt danach das Lister-Fenster             |
| **Strg+E** | Schaltet für diese Sitzung zwischen editierbar/schreibgeschützt um (verändert nicht die INI) |
| **ESC**    | Schließt das Lister-Fenster (ohne zu speichern)                                              |
| Tippen     | Ganz normal - Kommentarfeld ist ein RichEdit-Control                                         |

Es gibt **kein Auto-Save** – nur Strg+S schreibt etwas. Ist für das Objekt
noch kein `descript.ion`-Eintrag vorhanden, legt das Speichern automatisch
einen neuen an. Wird der komplette Text gelöscht und gespeichert, entfernt
`SetTCComment()` den Eintrag wieder.

Ist der Editiermodus aktiv (beim Öffnen laut INI oder per Strg+E
umgeschaltet), erscheinen drei Sternchen (`***`) vorne im Lister-Fenstertitel
als Kennzeichen. Verschwinden sie, ist das Feld schreibgeschützt.

## Zeitstempel

Beim Speichern wird optional `" (HH:MM:SS | dd.MM.yyyy)"` (Speicherzeitpunkt,
**nicht** Erstellungszeitpunkt)  direkt hinter den
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
AppendTimestamp=1
Editmodus=1
HighlightColor=34,87,128
```

Änderungen an der INI wirken sofort beim nächsten Öffnen des Lister-Fensters -
kein TC-Neustart nötig.

- **AppendTimestamp** (`0`/`1`, Default `1`): Zeitstempel beim Speichern anhängen oder nicht
- **Editmodus** (`0`/`1`, Default `1`): Startzustand editierbar/schreibgeschützt. Kann pro Sitzung zusätzlich mit Strg+E umgeschaltet werden, unabhängig vom INI-Wert
- **HighlightColor** (`R,G,B`, z. B. `34,87,128`): Hintergrundfarbe von Titel- und Zähler-Leiste. Textfarbe (schwarz/weiß) wird automatisch nach Helligkeit gewählt, damit es immer lesbar bleibt. **Fehlt dieser Schlüssel komplett**, wird keine eigene Farbe gesetzt - stattdessen die normale System-/Fensterfarbe

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

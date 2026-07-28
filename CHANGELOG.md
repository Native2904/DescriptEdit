# Changelog

Alle nennenswerten Änderungen an diesem Projekt werden hier dokumentiert.
Format angelehnt an [Keep a Changelog](https://keepachangelog.com/de/1.0.0/).

## [1.1.0] - 2026-07-26

### Hinzugefügt
- Ordner-Statistik-Zeile: Anzahl Objekte mit Kommentar, Anzahl ohne
  Kommentar, Größe der `descript.ion`
- Leicht abgerundete Ecken am Kommentarfeld
- INI-Schlüssel `FontFace`, `FontSize`, `TitleFontSize` für Schrift
- INI-Schlüssel `EditableBackgroundColor`, `ReadonlyBackgroundColor` für
  den Editiermodus-Hintergrund
- INI-Schlüssel `Language` (`auto`/`de`/`en`): Statistik-Zeile passt sich
  automatisch der Windows-Anzeigesprache an, statt fest auf Deutsch zu
  stehen (Rückmeldung eines englischsprachigen Nutzers)

### Geändert
- Editiermodus-Anzeige: Hintergrundfarbe des Kommentarfelds statt
  Sternchen im Fenstertitel
- Strg+S speichert jetzt ausschließlich, ohne irgendetwas zu schließen
- ESC schließt weiterhin das Lister-Fenster ohne zu speichern, jetzt aber
  mit Sicherheitsprüfung (Fenstertitel muss mit `Lister (` beginnen) -
  schlägt die Prüfung fehl (z. B. im QuickView), passiert nichts
- Fehlercode-Behandlung beim Schreiben in `descript.ion` robuster
  (Schreibgeschützt-/Versteckt-/System-Attribute werden korrekt behandelt)

### Entfernt
- Automatisches Schließen des Lister-Fensters nach Strg+S (Sicherheitsproblem
  im QuickView, siehe unten)

### Behoben
- **Kritisch:** Strg+S konnte im QuickView (Strg+Q) Total Commander
  komplett schließen oder zum Absturz bringen. Ursache: das Lister-Fenster
  wurde per `GetAncestor(..., GA_ROOT) + WM_CLOSE` automatisch geschlossen;
  im QuickView ist das aber TCs Hauptfenster selbst. Strg+S speichert jetzt
  nur noch; ESC (Schließen ohne Speichern) bleibt erhalten, aber mit
  Sicherheitsprüfung des Fenstertitels
- Statistik zeigte teils mehr "Kommentare" als tatsächlich Dateien im
  Ordner vorhanden waren (verwaiste `descript.ion`-Einträge wurden
  mitgezählt, ohne zu prüfen, ob die zugehörige Datei noch existiert)
- RichEdit sendete `EN_CHANGE` nicht ohne `EM_SETEVENTMASK` - Strg+S hat
  dadurch zeitweise nie gespeichert
- Zeitstempel-Position in halber Schriftgröße konnte bei mehrzeiligen
  Kommentaren leicht verrutschen
- Ordner (im Gegensatz zu Dateien) wurden von TCs Detect-String nicht
  erfasst - `EXT=""` ergänzt

## [1.0.0] - 2026-07-25

### Hinzugefügt
- Erste Version: Anzeige und Bearbeitung von TC-Dateikommentaren
  (`descript.ion`) im Lister
- Speichern nur per Strg+S, kein Auto-Save
- Optionaler Zeitstempel beim Speichern
- Zeichenzähler mit Limit (2000 Zeichen)
- Editiermodus umschaltbar (Strg+E), Startzustand per INI
- Konfigurierbare Hervorhebungsfarbe für Titel/Zähler

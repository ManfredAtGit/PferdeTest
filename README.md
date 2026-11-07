# Zielsetzung

Bei PferdeTest handelt es sich um eine javascript serverless, client-side-only web application.  

Die App ist komplett eigenständig und basiert nur auf nativem HTML, CSS und JavaScript:
- Keine Frameworks oder Libraries: kein React, Vue, jQuery o. Ä., kein Build-Step, keine npm-Pakete, keine Importe
- Keine externen Ressourcen: keine CDN-Links, keine Google Fonts, keine Icon-Fonts, keine Bilder von außen. Schriften laufen über den nativen system-ui-Font-Stack, alle Icons (Pfeile, Häkchen, Griff, Start-Icon) sind selbst erzeugte Inline-SVGs
- Verwendete Browser-APIs sind ausschließlich Standard-Plattformfeatures: DOM (createElement/createElementNS, addEventListener), localStorage, Pointer Events (für Drag & Drop), getComputedStyle, JSON. Mehr nicht
- Die Datei funktioniert offline, braucht nur einen Browser, kann beliebig gehostet oder auch direkt als lokale Datei geöffnet werden, und es gibt keine Abhängigkeit, die gepflegt, aktualisiert oder geladen werden muss.
- Einzige „Abhängigkeit“ ist der Browser selbst, deshalb habe ich die unterstützte Baseline im Code kommentiert (Chrome/Edge 79+, Firefox 68+, Safari 13+ / iOS 13+).



## Was der Prototyp „PferdeTest" zeigt

- Single Choice & Multiple Choice: große ankllickbare Karten statt kleiner Radiobuttons, mit klarer Auswahl-Markierung; mobil 1 Spalte, ab Tablet 2 Spalten
- Freitext: Textarea mit Fokus-Rahmen und Zeichen-Zähler
- Sortierung: Liste mit Pfeil-Buttons (funktioniert zuverlässig auf Touch, im Gegensatz zu reiner Drag-&-Drop-Sortierung)
- Skala (Likert): segmentierte Bewertung mit Beschriftung an den Endpunkten
- Rahmen-UI: Startscreen, eine Frage pro Bildschirm, Fortschrittsbalken, Zurück/Weiter-Navigation, Ergebnis-Screen mit Antwort-Review un


## Was die App kann

- Vollständiger Test-Loop:
  - Startscreen mit Fragenanzahl, geschätzter Dauer und Fragetypen
  - Eine Frage pro Bildschirm mit Fortschrittsbalken
  - Pflicht-/Optional-Validierung
  - Ergebnis-Screen mit Punkten, Bestanden/Nicht-bestanden und Auswertung aller Antworten inklusive Erklärungen

- Alle 5 Fragetypen:
  - Single Choice
  - Multiple Choice
  - Freitext (mit Zeichen-Zähler und optionaler Stichwort-Bewertung)
  - Sortierung
  - Skala

- Antworten werden in `localStorage` gesichert:
  - Bei Reload oder Tab-Wechsel wird der Test automatisch am letzten offenen Punkt fortgesetzt
  - Mit „Von vorn beginnen“-Option

- Automatischer Dark Mode

- Reduzierte Animationen bei `prefers-reduced-motion`

## Cross-Browser- und Mobile-Absicherung

- Baseline:
  - Chrome/Edge 79+
  - Firefox 68+
  - Safari 13+ / iOS 13+
  - Samsung Internet 11+
  - Bewusster Verzicht auf:
    - `?.`
    - `??`
    - logische Zuweisungen
    - `:has()`
    - Flexbox-Gap

- Sortier-Drag-and-Drop über Pointer Events (Maus, Touch, Stift) mit Fallback über Auf-/Ab-Buttons, falls Pointer Events fehlen. Die Buttons sind zusätzlich per Tastatur bedienbar.

- Mobile-Optimierungen:
  - Mindestens 44–52 px Touch-Flächen
  - `touch-action: manipulation` gegen Doppel-Tipp-Zoom
  - 16 px Input-Font gegen iOS-Auto-Zoom
  - Safe-Area-Insets für Notches
  - `aria-pressed` und `aria-live` für Screenreader

- Sicherheit und Robustheit:
  - Kein Einsatz von `innerHTML` für Inhalte (kein HTML-Injection-Risiko)
  - `localStorage`-Fehler (z. B. im Privatmodus) werden abgefangen
  - Unbekannte Fragetypen führen zu einem Fallback statt zu einem Absturz

## Erweiterbarkeit

- Fragen werden ausschließlich im `TEST`-Objekt am Dateianfang gepflegt (JSON-artig), z. B. mit:
  - `points`
  - `required`
  - `hint`
  - `explanation`

- Neue Fragetypen können über die `TYPES`-Registry ergänzt werden:
  - Objekt mit:
    - `render`
    - `isEmpty`
    - `grade`
    - `display`
  - Anschließend genügt `type: "..."` in einer Frage.

- Alle UI-Texte liegen zentral in `STR` und können dadurch später einfach mehrsprachig gemacht werden.


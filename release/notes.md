## Änderungen 5.39.0 – Wieder schnell: Oberfläche, Engine und PC entlastet; das Beste aus MaxCode übernommen

Gemessen mit einem großen Bestand (810 Sitzungen, 152 Ziele, lange Chats) und einem gebauten Spiel im Agenten-Browser:

- **Agenten-Browser: Live-Bild gebremst (aus MaxCode).** Nach dem ersten Blick schickte der Browser bei bewegten Seiten
  jedes Bild – auch ohne Zuschauer und für immer. Gemessen: 53 Bilder/s, ohne Zuschauer 17,6 s Rechenzeit des Browsers
  und 2,0 s der Engine je 10 s (fast zwei Prozessorkerne). Jetzt ~20 Bilder/s beim Zusehen und nach 6 s ohne Zuschauer
  gar keine mehr (Engine 0 ms, Browser nur noch die Seite selbst); beim nächsten Blick läuft es sofort wieder.
- **Ziele einzeln gespeichert.** Alle Ziele standen in einer `goals.json`, die bei jeder Änderung komplett neu geschrieben
  wurde – alle 300 ms, solange ein Ziel lief (im Test 2,4 MB, 24 ms je Speichern; echte Ziele sind viel größer, unter
  Windows mit Virenscanner ein Vielfaches). So lange antwortete die Engine nicht. Jetzt je Ziel eine Datei unter
  `goals/`, geschrieben wird nur Geändertes (höchstens alle 0,4 s). Die alte Datei wird beim ersten Start übernommen und
  bleibt als `goals.json.bak` erhalten.
- **HTML-Live-Ansichten im Chat laufen nur noch sichtbar.** Jeder HTML-Block (gebaute Spiele, Seiten) lief als Live-Ansicht
  dauerhaft weiter, auch weit oben im Verlauf: 10 Spiele in einem Chat hielten die Oberfläche zu einem Drittel beschäftigt.
  Jetzt startet nur die neueste von selbst, ältere per Klick; außerhalb des Sichtbereichs ruhen sie. Der Chat bleibt dabei
  ganz unten (die Ansichten schoben ihn vorher nachträglich hoch).
- **Ziel-Karten und Zentrale laden nur, was sich geändert hat.** Jede Änderung irgendeines Ziels lud bisher alle Ziel-Karten
  eines Chats komplett neu – mehrmals pro Sekunde. Die Engine meldet jetzt, welche Ziele sich geändert haben; die Zentrale
  holt ihre Liste höchstens alle 0,7 s, die Live-Leiste nur laufende Ziele.
- **Schnellerer Start.** Seiten werden erst beim Öffnen geladen (vorher wartete die erste Ansicht auf alle elf), die 3D-Katze
  (Three.js, Modell, Texturen ≈ 1,2 MB und WebGL) nur, wenn die Katze eingeschaltet ist, die Laute der alten Katze nicht mehr
  im Startpaket (339 KB). Gemessen mit vierfach gedrosselter CPU (wie ein langsamer Laptop): Chat 2,1–2,8 s → 1,5–1,6 s,
  Zentrale 2,2–2,4 s → 1,6–1,7 s; geladen 4,1 MB → 2,5 MB.
- **Diktat-Knopf ohne Dauerschleife:** Er richtete sich 60-mal pro Sekunde neu aus, solange ein Eingabefeld den Fokus hatte
  (im Chat fast immer). Jetzt nur bei Scrollen, Größenänderung und alle 0,4 s.
- **Claude Code (Abo) – aus MaxCode übernommen:** keine Hintergrundaufgaben mehr (im Einmal-Modus endete der Lauf sonst mit
  „warte auf die Fertigmeldung“, die nie kam – der Bericht fehlte), eigene Claude-Code-Einstellungen, Plugins, Hooks, das
  eigene Claude-Gedächtnis und fremde `CLAUDE.md` bleiben draußen (Catcoder gibt die Projektanweisungen selbst mit), nur
  Catcoders Werkzeuge und MCP-Server (in MaxCode gemessen: 47,4k → 29,5k Tokens je erstem Aufruf), Denkaufwand als
  `--effort` (Opus 5.5/Sonnet 5 ignorieren das Denkbudget). Kennt eine ältere Claude-Code-Version die neuen Optionen nicht,
  läuft der Aufruf automatisch wie bisher.
- **DeepSeek-Kosten (aus MaxCode):** Preise mit Cache-Treffern (≈ 1/50) und dem halben Preis außerhalb der Spitzenzeit –
  vorher hatte DeepSeek keinen Preis, Kosten und Kostengrenzen zählten dort nicht.
- **Geprüft, bei Catcoder schon gelöst:** beweglicher Ziel-Stand außerhalb des System-Prompts (bei uns die „Lage“), Bilder
  im Verlauf rücken nur in Stufen (Prompt-Cache bleibt), Sperrzeiten für Gratis-Modelle je Fehlerart, kein festes
  Schrittlimit. **Nicht übernommen:** Code-Index (braucht ein Suchmodell über OpenRouter und ein Fremdpaket), ComfyUI,
  Andock-Ansicht – eigene Baustellen.


## Änderungen 5.43.6 – Spiele werden wirklich bedient geprüft, Coder testet selbst

- **Ursache gefunden:** Ein echt gebautes Snake-Spiel wurde als „fertig und geprüft“ gemeldet, startete aber im „Game Over“-Zustand und bewegte sich nie. Die Prüfung hatte nur Code und Bilder angesehen, nie gespielt.
- **Bedienprobe:** Spiele und interaktive Seiten werden bei der Prüfung echt bedient – Startzustand, Bewegung, Klicks, Pfeiltasten, WASD, Leertaste, Enter, Neustart-Knopf, Konsolenfehler. Ein Fehler ist nur noch Fehler, wenn er wirklich auftritt (ein Spiel, das ohne Steuerung an der Wand endet, ist korrekt erkannt).
- **Selbsttest für den Coder:** Neues Werkzeug `labor action=play`. Nach einer nicht bestandenen Bedienprobe prüft der Coder seinen Fix selbst, bis „BESTANDEN“ kommt – statt blind zu flicken.
- **Spiele aus einer Hand:** Kleine Spiele werden nicht mehr in parallele Teile zerlegt, deren IDs und Overlays nicht zusammenpassen. Bleibt ein Spiel nach mehreren Runden kaputt, wird die Logik einmal stimmig neu geschrieben.
- **Keine Hänger durch grübelnde Modelle:** Ein Modell, das nur nachdenkt und nie handelt, wird nach spätestens 2,5–4 Minuten gebremst und nach zwei Bremsungen durch ein anderes ersetzt (vorher hing ein Aufruf bis zu 13 Minuten).
- **Getestet:** Snake-Spiel mit echtem Gratis-Modell gebaut, geprüft und von einem Test-Bot gespielt (frisst, Punkte steigen, Neustart klappt). Zusätzlich To-do-Webseite aus einem Guss gebaut und ausgeliefert.

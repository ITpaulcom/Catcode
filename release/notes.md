## Änderungen 5.41.0 – Gezielt prüfen, sauber nebeneinander arbeiten, über Nacht durchhalten

- **Betroffene Tests (aus MaxCode).** Nach einer Änderung prüft Catcoder genau die Tests, die davon abhängen – gefunden
  über die Import-Kette (auch Python-Pakete), Textverweise und Einstiegsdateien wie den Server, den Tests per Pfad
  starten. Statt aller Tests (langsam) oder gar keiner (riskant) laufen die nächsten fünf; Hilfsdateien im Testordner
  zählen nicht als Test. Der Testläufer wird erkannt (vitest, jest, mocha, Playwright, `node --test`, pytest).
  Agents können es auch selbst nutzen: `check_project` mit `kind=affected`.
- **Kein falsches „steht oben“ mehr.** Las ein Agent eine Datei erneut, antwortete Catcoder „unverändert, steht oben“ –
  auch wenn die frühere Ausgabe inzwischen gekürzt oder verdichtet war. Der Agent sah den Inhalt dann gar nicht mehr und
  las wieder und wieder. Nach dem Kürzen und Verdichten kommt jetzt wieder der Inhalt.
- **Warnung bei gleichzeitigem Ändern.** Ändert ein Chat eine Datei, die ein anderer Chat im selben Projekt in den
  letzten 20 Minuten geändert hat, bekommt er sofort einen Hinweis (wer, wann) und liest sie frisch, statt die fremde
  Änderung zu überschreiben. Teammitglieder desselben Ziels sind ausgenommen (die stimmen sich ohnehin ab).
- **Über Nacht durchhalten.** Sind die Modelle überlastet oder ist das Abo-Limit erreicht, versucht ein Ziel es jetzt bis
  zu 12 Stunden lang (1, 2, 5, 10, 15 min, dann alle 30 min) statt nach einer Stunde aufzugeben – und nennt der Anbieter
  die Rücksetzzeit (Claude-/ChatGPT-Abo), geht es genau dann weiter. „Usage limit reached“ gilt jetzt auch bei Zielen als
  vorübergehend.
- **Tempo-Diagnose zeigt den echten Start:** wie lange die Windows-App bis zur fertigen Engine brauchte (aus
  `launcher.log`), wann die Engine selbst bereit war – mit Hinweis, wenn der Start langsam ist (oft der Virenscanner).
- Gemessen mit großem Bestand (1410 Chats, 202 Ziele, 94 MB): Engine in 0,3 s bereit, alle Abfragen unter 15 ms,
  Oberfläche nach 1,1 s bedienbar (danach 0,3 s).
- In der Team-Anzeige schläft eine kleine Katze, solange niemand arbeitet.

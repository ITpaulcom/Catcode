## Änderungen 5.50.0 – Effizienter Harness: weniger Ballast, weniger Runden, weniger Fehlversuche

Gemessen mit `bench/run.mjs`: 9 Aufgaben (Fehler beheben, Funktion bauen, Umbenennen, große Datei, Fehlersuche, Skript, Code verstehen, Tests schreiben, Browser-Spiel), dasselbe Gratis-Modell, alte und neue Fassung abwechselnd, je 3 Durchgänge (27 Aufgaben je Fassung):

| | 5.49.0 | 5.50.0 |
|---|---|---|
| bestanden | 23 von 27 | **26 von 27** |
| Modell-Runden je Aufgabe | 5,4 | **4,5** |
| doppelte Werkzeug-Aufrufe | 36 | **15** |
| Eingabe-Tokens gesamt | 1 976 000 | **1 167 000 (−41 %)** |
| feste Tokens je Anfrage (System + Werkzeuge) | 7 026 | **4 454 (−37 %)** |
| ohne das Browser-Spiel: bestanden / Eingabe-Tokens | 23 von 24 / 853 000 | **24 von 24 / 555 000 (−35 %)** |
| Browser-Spiel bestanden | 0 von 3 | **2 von 3** |

Die Bedienprobe für das Browser-Spiel hängt am Timing (laufen mehrere Browser gleichzeitig, verpasst sie Bewegungen). Die Bench-Prüfung versucht es deshalb jetzt bis zu dreimal. In einer Nachmessung mit 5.50.0 liefen alle drei gebauten Snake-Spiele, mit 58 000–207 000 statt bis zu 937 000 Tokens.

- **Nur noch die Werkzeuge, die der Auftrag braucht:** Bisher luden Gruppen schon bei Allerweltswörtern: Code-Analyse bei „Test“, „Fehler“ oder „Funktion“, das Geräte-Labor bei „Konsole“, Ausführen/Server bei „node“. Jetzt laden sie nur bei passenden Aufträgen. Reinraum-Analyse, Dateien verschieben, Bibliotheks-Doku, Gedächtnis-Suche und Skill-Dateien kommen auf Abruf, `git` nur in Git-Projekten, das Projektgedächtnis nur bei Projekten mit Karte oder Notizen. Bei einer typischen Coding-Aufgabe sind es 14 statt 22–32 Werkzeuge: 2 250 statt 4 400–5 900 Tokens bei jeder Anfrage. Der komplette feste Teil liegt damit bei etwa 13 500 Zeichen. Zum Vergleich: Allein die System-Prompt-Datei von Codex CLI für GPT-5.2 hat 21 652 Zeichen, ohne Werkzeuge.
- **Ändern und Prüfen in einer Runde:** Datei-Änderungen einer Antwort laufen jetzt vor Befehlen und Lesen. `edit_file` und `run_command` (Test) passen also in eine Antwort. Verteilt ein Modell beides auf zwei Runden, bekommt es einmal den Hinweis.
- **Keine doppelten Prüfungen:** Catcoder lässt die betroffenen Tests nach der Abschlussmeldung selbst laufen, und das Modell erfährt das. Hat es nach seiner letzten Änderung schon grün getestet, läuft nichts doppelt.
- **Live-Prüfung im Browser auch im Chat:** Geänderte Webseiten und Browser-Spiele öffnet Catcoder nach der Abschlussmeldung selbst: Konsole, bei Spielen eine Bedienprobe. Nur Probleme gehen ans Modell. Vorher klickte ein Modell sich durch 4–6 eigene Browser-Runden, im schlimmsten gemessenen Fall durch 51 Runden mit 31 doppelten Aufrufen und 937 000 Tokens, und das Spiel lief am Ende trotzdem nicht. Weil die Bedienprobe am Timing hängt, wiederholt Catcoder sie vor einer Fehlermeldung einmal. Klickt ein Modell beim Bauen einer Seite trotzdem Runde um Runde selbst (gemessen: 31 Klicks, 522 000 Tokens), erinnert Catcoder es nach 5 und nach 10 reinen Browser-Runden daran, dass die Prüfung danach automatisch kommt.
- **Weniger Fehlversuche bei Werkzeug-Aufrufen:** Packt ein Modell weitere Parameter im XML-Stil in einen Text (`press\n<parameter=key>\narrowright`), teilt Catcoder sie richtig auf, statt mit einem Fehler eine Runde zu verlieren.
- **Bench misst die vier Kennzahlen:** Werkzeug-Effizienz (beim ersten Mal richtig, doppelte Aufrufe), Overhead (feste Tokens je Anfrage und ihr Anteil), Runden je Aufgabe und Aufrufe je Runde, Erfolg und Zeit. Mit `--keep` bleiben die Projektordner zum Nachsehen erhalten.
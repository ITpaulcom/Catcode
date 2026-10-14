## Änderungen 5.40.0 – Kontext repariert, autonom bis zum Ende, Labor mit echten Browsern und Geräten, kleine ASCII-Katzen

- **„Liest immer dieselben Dateien, kommt nie zum Ergebnis“ – Ursache behoben.** Ab ≈ 130 000 Zeichen Verlauf (und gleich
  nach jedem Verdichten) blieben nur die letzten zwei Werkzeug-Ausgaben wörtlich, alle älteren wurden auf 1 500 Zeichen
  gekürzt (bei kleinen Modellen 500). Das Modell sah die eben gelesenen Dateien nicht mehr und las sie neu – und der
  Fortschritts-Wächter wertete genau das als Kreisen und schloss den Lauf nach 13 Schritten ohne Ergebnis ab. Jetzt (nach
  MaxCode-Vorbild) bleibt eine **Menge** wörtlich: die jüngsten Ausgaben bis ≈ 40 % der Verdichtungsgrenze (mindestens
  60 000 Zeichen); gekürzt wird nur in großen Stufen (der Prompt-Cache bleibt, gemessen 94,5 % Treffer). Gekürzte
  Dateien behalten ihre **Gliederung mit Zeilennummern** (Funktionen, Klassen, Überschriften), sodass gezielt nachgelesen
  werden kann; ältere Doppel-Lesungen derselben Datei werden als „überholt“ markiert statt doppelt mitgeschickt. Wird
  gekürzt, vergisst der Wächter die Lesevorgänge – erneutes Lesen gilt dann nicht als Kreisen.
- **Gedächtnis je Projekt.** Was in einem Chat an einem Projekt gemacht wurde (Auftrag, Ergebnis, geänderte Dateien), steht
  im Projekt-Tagebuch; ein neuer Chat am selben Projekt weiß sofort, was zuletzt passiert ist – ohne alles neu zu lesen.
  Es kommt mit der „Lage“ (nicht im System-Prompt), der Prompt-Cache bleibt.
- **Modelle mit Anmeldefehler werden 10 Minuten übersprungen (aus MaxCode)** – abgelaufene Claude-Code-Anmeldung oder
  falscher Key kosten nicht mehr jeden Lauf einen Fehlversuch; es geht gleich mit dem Ausweichmodell los.
- **DeepSeek/GLM (aus MaxCode):** der Denkteil geht bei jedem Agent-Schritt zurück (DeepSeek antwortete sonst mit Fehler
  400, GLM dachte jedes Mal neu); neu im Katalog: DeepSeek V4.1 Flash (1 Mio. Tokens Kontext).
- **Team-Aufrufe aus Claude Code ohne Zeitlimit:** lange Team-Aufgaben brachen nach 5 Minuten mit „fetch failed“ ab.
- **Schneller unter Windows:** Die Modell-Liste wartete bis zu 1,5 s auf lokale Server (Ollama, LM Studio – unter Windows
  dauert eine abgewiesene Verbindung ≈ 2 s), jeder Chat wartete beim Öffnen darauf – jetzt im Hintergrund. Die Zentrale
  ließ alle 15 s `git status` für jedes Projekt laufen – jetzt zwischengespeichert (45 s, höchstens drei gleichzeitig).
  Im Leerlauf fragt die Oberfläche nur noch zweimal je 30 s.
- **Tempo-Messung eingebaut:** Einstellungen → Diagnose → Tempo zeigt, wo es auf dem eigenen PC hängt (Engine blockiert,
  langsame Anfragen, Startzeit und Hänger der Oberfläche); Auffälliges steht im Protokoll („Tempo: …“).
- **Autonom weiter.** Ein Ziel endete nach seinem Zeitbudget (Standard 20 min) mit offenen Punkten und wartete auf
  „weitermachen“. Jetzt arbeitet das Team die offenen Punkte selbst ab – Runde für Runde, bis nichts mehr offen ist. Es
  hört auf, wenn eine Runde nichts mehr verbessert (kein Kreisen) oder nach der eingestellten Rundenzahl (Standard 4,
  Einstellungen → Agents: „Autonom weiter“, 0 = erst fragen; dort auch das Zeitbudget je Runde). Bis 16 parallele Agents.
- **Mehrere Chats und Ziele im selben Projekt** wissen voneinander: Jeder Lauf erfährt, wer nebenan arbeitet und an
  welchen Dateien – diese werden vor dem Ändern frisch gelesen und nur gezielt geändert, statt sich gegenseitig zu
  überschreiben. Die Zentrale zeigt Git-Änderungen sofort nach jedem Lauf (vorher bis zu 45 s alt).
- **Labor: echte Browser.** Safari-Geräte (iPhone, iPad, Mac) laufen auf Wunsch wirklich mit **WebKit**, Firefox-Geräte
  wirklich mit **Gecko** – Fehler, die nur dort auftreten, zeigen sich auch im Labor. Ein Klick auf „Engines“ lädt beide
  (≈ 250 MB, ohne npm); ohne sie stellt Chromium die Geräte nach und das Labor sagt das ehrlich („nachgebildet“). Neue
  Browser: Firefox für Android, Chrome für iOS, Opera, Brave, Vivaldi, Firefox und Edge am Mac – 52 Geräte.
- **Labor: volle Gerätedaten.** Jedes Gerät hat sein Datenblatt (Chip, Kerne, Arbeitsspeicher, Grafikchip, Display mit
  Zoll/ppi/Hz, Farbraum, HDR, Notch-Abstände, Sensoren, Netz, Browser) – und die getestete Seite sieht genau das, so wie
  der echte Browser es meldet: `hardwareConcurrency`, `deviceMemory` (nur Chromium), WebGL-Renderer („Apple GPU“,
  „Adreno (TM) 830“, „RTX 4070“ …), `color-gamut`, `dynamic-range`, Farbtiefe, `env(safe-area-inset-*)`, Bildschirm samt
  Taskleiste, Akku und Netz (nur Chromium), Touch-Punkte, Anbieter. Das Datenblatt steht unter jedem Gerät (ⓘ).
- **Labor: Geräte wie echt** statt grauer Umrisse: Gehäuse in der echten Farbe mit Metallrahmen, Seitentasten,
  Dynamic Island, Notch oder Kameraloch, Statusleiste mit Uhrzeit/Netz/Akku, die Leisten des jeweiligen Browsers
  (Safari unten, Chrome oben, Samsung Internet, Firefox; am PC Fenster mit Tabs für Edge, Chrome, Firefox, Opera,
  Safari mit Ampel), Laptop mit Unterteil, Monitor mit Standfuß, Fernseher, Konsole, Uhr mit Krone und Armband, Switch mit
  Joy-Cons. In der Geräteliste kleine Geräte in Gehäusefarbe.
- **Kleine ASCII-Katzen in der Oberfläche:** Wo es nichts zu sehen gibt, sitzt eine kleine, sanft bewegte Katze aus
  Schriftzeichen – sie schläft im leeren Chat, winkt beim neuen Chat, spielt mit Wolle bei leeren Projekten, schaut durchs
  Fernrohr in Zentrale und Labor, sucht mit der Lupe, sitzt neben der Uhr, angelt nach Modellen; Pfotenspuren und
  funkelnde Sterne. Bewegt wird nur, was sichtbar ist (bei „weniger Bewegung“ steht alles still).

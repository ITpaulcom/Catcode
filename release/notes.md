## Änderungen 5.48.0 – Eigene Modelle bis zu 5× schneller: llama.cpp direkt, passende Modelle für deinen PC

Gemessen auf derselben Hardware mit demselben Modell und demselben Agenten-Verlauf (großer System-Prompt, Werkzeuge, Dateiinhalte):

| | bisher | 5.48.0 |
|---|---|---|
| 6 Agenten-Runden | 586 s | **119 s** |
| je Runde ab Runde 2 | 88 s | **18,5 s** |
| Code ausgeben (Datei mit kleiner Änderung) | 27 Tokens/s | **56 Tokens/s** |

- **Modellhost 2 – das offizielle llama.cpp:** Catcoder lädt beim ersten Start die passende Fassung von llama.cpp (NVIDIA: CUDA 13 bzw. 12 je nach Treiber, AMD/Intel: Vulkan, sonst CPU; Mac: Metal) und prüft die Prüfsumme. Startet eine Grafikkarten-Fassung nicht, nimmt Catcoder die nächste; geht gar nichts, die bisherige Laufzeit.
- **Prompt-Cache:** Ein Agent schickt in jeder Runde den ganzen Verlauf. Bisher las das Modell ihn jedes Mal komplett neu, jetzt kommt der gemeinsame Anfang aus dem Cache (80–90 % ab Runde 2).
- **Turbo (spekulatives Dekodieren ohne zweites Modell):** Code und wiederkehrende Texte kommen etwa doppelt so schnell, bei gleichem Ergebnis. In den Einstellungen abschaltbar.
- **Kontext passend zum Speicher statt 4096:** llama.cpp nimmt so viel Kontext und so viele Schichten auf die Grafikkarte, wie hineinpassen (im Test 40 960 Tokens). Dazu kommen parallele Plätze, sodass Teammitglieder gleichzeitig arbeiten statt in einer Warteschlange. Werkzeugaufrufe laufen im Format des jeweiligen Modells.
- **Versteckte Fehler behoben:** Ohne Netzwerkfreigabe bekam der Anbieter „Eigene Modelle“ einen leeren Token. Außerdem nahm die Engine 128 000 Tokens Kontext an, obwohl das Modell mit 4096 lief. Jetzt gelten der echte Token und der echte Kontext, und der Host zählt als lokal (passender Werkzeugsatz).
- **Denken mit Budget statt Stoppuhr:** Kleine lokale Modelle wie Qwen3 dachten minutenlang vor dem ersten Schritt. Jetzt begrenzt der Aufwandsregler das Denken in Tokens (aus, 1 024, 4 096, 8 192, 16 384). Im Test: ganze Aufgabe 121 s statt 449 s.
- **Läuft über Tage:** Stürzt llama.cpp im Betrieb ab, startet Catcoder es mit demselben Modell neu (bis zu dreimal je Stunde).
- **Empfohlen für deinen PC:** Sechs geprüfte Coding-Modelle (Qwen3 4B, Qwen3.5 9B, gpt-oss 20B, Devstral Small 2, Qwen3-Coder 30B, Qwen3.6 27B). Catcoder erkennt Grafikkarte, Grafikspeicher und Arbeitsspeicher und zeigt, was ganz auf die Grafikkarte passt, was mit Arbeitsspeicher läuft und welches Modell am besten passt. Ein Klick lädt herunter und startet das Modell.
- **Bekannte Grenze:** Gemessen wurde auf der CPU. Auf Grafikkarten ist llama.cpp ohnehin um ein Vielfaches schneller. Die Auswahl der Windows-Fassung (CUDA/Vulkan) ist über Tests abgesichert, auf echter Windows-Hardware aber noch nicht gemessen.

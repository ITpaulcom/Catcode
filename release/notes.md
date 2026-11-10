## Änderungen 5.51.0 – Dreistufen-Speicher: Riesen-MoE wie DeepSeek V4.1 Flash aus Grafikkarte + Arbeitsspeicher + SSD

Modelle, die größer sind als Arbeitsspeicher und Grafikkarte zusammen, laufen jetzt lokal (`lib/speicherplan.mjs`):

- **Grafikkarte = Beschleuniger** für alles, was jedes Token braucht: Attention, gemeinsame Experten, Router, Ausgabekopf, KV-Cache – dazu so viele ganze Experten-Schichten, wie noch passen (llama.cpp `--fit`).
- **Arbeitsspeicher** hält die häufig gebrauchten Experten; gerechnet wird nur auf den **Leistungskernen** (unter Windows nimmt llama.cpp sonst alle Kerne – beim i9-13900HX 24 statt 8; E-Kerne bremsen den Gleichschritt). Erkannt über die CPU-Sets von Windows bzw. sysfs unter Linux, Threads fest auf je einen P-Kern (`-t 8 -C 0x5555 --cpu-strict 1`).
- **SSD**: das Modell bleibt eingeblendet (mmap), das Betriebssystem lädt seltene Experten nach. Die Engram-Tabelle von DeepSeek V4.1 (110–135 GB) liest llama.cpp zeilenweise von der SSD (`--lazy-mode on`). Ohne Umpacken (`--no-repack` – sonst kopiert llama.cpp die Gewichte in eigenen Speicher: gemessen vom System beendet) und ohne Aufwärmen mit allen Experten (`--no-warmup` – das läse beim Start das ganze Modell). Großer Vorlauf-Block (`-ub 4096`): die Experten wandern je Block einmal zur Grafikkarte.
- **Vorher sehen, was einen erwartet:** Catcoder liest den Aufbau aus der Tensor-Tabelle der GGUF-Datei (auch aufgeteilte Modelle und Köpfe von Hugging Face, ohne Download) und schätzt das Tempo aus Bytes je Token und Bandbreite je Stufe (Grafikspeicher je Karte, RAM-Takt, SSD-Lesetest). Die Modellliste zeigt die Schätzung je Modell, die Modellhost-Karte den Speicherplan.
- **DeepSeek V4.1 Flash** in der Modellliste (Unsloth, MIT): UD-Q2_K_XL (335 GB, Benchmark-Mittel 87,5 statt 87,7 bei Q8 – praktisch Originalqualität) und UD-IQ1_S (233 GB, 83,5). Aufgeteilte Modelle laden alle Teile mit Prüfsumme, Platz auf der SSD wird vorher geprüft. Die eingebaute llama.cpp-Fassung (b12079) kennt `deepseek41`, Engram und das DSML-Werkzeugformat.
- Kürzeres Nachdenken bei Modellen von der SSD (Denk-Budget 320 statt 768 Tokens), damit eine Runde nicht minutenlang nur denkt.

Gemessen (echter llama-server b12079, MoE mit 820 MB, Arbeitsspeicher per cgroup auf 400 MB begrenzt): läuft mit denselben Antworten, 3,1–3,5 Tokens/s statt 40–46 ohne Grenze; ohne `--no-repack` vom System beendet. Daraus kalibriert: Nachladen über mmap erreicht ≈ 15 % der Lese-Rate der SSD.

**Was das für einen Dell G16 7630 heißt** (RTX 4070 Laptop 8 GB / 256 GB/s, 32 GB DDR5-4800, i9-13900HX; Aufbau aus den echten GGUF-Köpfen):

| Modell | Speicherplan | geschätzt |
|---|---|---|
| DeepSeek V4.1 Flash UD-IQ1_S (233 GB) | Grafikkarte 6,4 GB · RAM 26 GB · SSD 200 GB | 0,4–1,5 Tokens/s, Vorlauf ~1 min je 4 000 neue Tokens |
| DeepSeek V4.1 Flash UD-Q2_K_XL (335 GB) | Grafikkarte 6,4 GB · RAM 26 GB · SSD 300 GB | 0,2–0,8 Tokens/s |
| gpt-oss 20B | Grafikkarte 6,1 GB · RAM 5,5 GB | 37–41 Tokens/s |
| Qwen3.5 9B | ganz auf der Grafikkarte | ~36 Tokens/s |

Grenze der Physik: Jedes Token von V4.1 Flash liest 6,6 GB dichte Gewichte (Attention, gemeinsame Experten) und 1,8 GB Experten. Über die 256 GB/s der Laptop-4070 und DDR5 sind das selbst mit unbegrenztem Arbeitsspeicher höchstens ~13 Tokens/s; 50 Tokens/s brauchen das ganze Modell im Grafikspeicher mehrerer großer Karten. Auf einem Laptop ist V4.1 Flash damit etwas für Aufträge im Hintergrund (Dauerlauf, über Nacht).

## Änderungen 5.52.0 – Turbo-Ausgabe: MTP, Selbstabstimmung auf dem eigenen PC, große Modelle, die flüssig bleiben

Wie schnell ein Modell schreibt, hängt daran, wie viele Gewichte es **je Token** lesen muss – nicht an seiner Gesamtgröße. 50 Tokens/s heißt 20 ms je Token: Eine Grafikkarte mit 256 GB/s (RTX 4070 Laptop) schafft in der Zeit rund 4 GB, zweikanaliges DDR5 rund 1 GB. Der Ausgabekopf allein ist nur ein Bruchteil davon (bei DeepSeek V4.1 Flash 0,5 von 8,4 GB je Token) – „die Grafikkarte nur für die Ausgabe“ beschleunigt deshalb nichts; jedes Token braucht den ganzen Durchlauf. Flüssig laufen große Modelle, wenn sie **wenig je Token** anfassen (MoE mit kleinem aktivem Anteil), ganz in Arbeitsspeicher + Grafikspeicher liegen und je Durchlauf mehr als ein Token liefern. Dafür:

- **MTP-Turbo:** Modelle mit eingebautem MTP-Kopf (Qwen3.5/3.6/3.8, Gemma 4 …) sagen je Durchlauf mehrere Ausgabe-Tokens voraus und prüfen sie gleich mit – gleiches Ergebnis, mehr Tokens je Durchlauf (`--spec-type draft-mtp`, zusätzlich zum n-gram-Turbo). Catcoder erkennt den Kopf im Modell oder eine Beiwagen-Datei (`mtp-….gguf`) und schaltet MTP mit Grafikkarte ein. Startet es damit nicht, lädt das Modell ohne.
- **Auf diesen PC abstimmen** (Einstellungen → Eigene Modelle): lädt das Modell in mehreren Einstellungen – Standard, MTP mit 1–3 Entwurfs-Tokens, Experten-Cache auf der Grafikkarte (`--moe-cache-mib`: alle Experten im Arbeitsspeicher, die häufig gebrauchten zusätzlich im Grafikspeicher) –, misst jede mit Text und Code auf dem Gerät und behält die schnellste. Sie gilt für dieses Modell auf dieser Hardware und greift bei jedem Laden; stürzt es damit ab, fällt Catcoder auf den Standard zurück. Dazu die Messung „mehrere Agents gleichzeitig“.
- **Große Modelle, die flüssig bleiben:** neu in der Liste Qwen3.6 35B (A3B) mit MTP (35 Mrd. Parameter, je Token rechnen 3 Mrd.; Apache-2.0; SWE-bench Verified 73,4 laut Hersteller). Mit erkannter Hardware empfiehlt Catcoder das **stärkste Modell, das geschätzt flüssig läuft** (≥ 25 Tokens/s ohne Turbo), statt des kleinsten, das ganz auf die Grafikkarte passt.
- **Schätzung rechnet mit der CPU:** nicht nur Speicherbandbreite, auch Kerne × Takt (gemessen 4,1 GB/s quantisierte Gewichte je Kern bei 2,1 GHz).

Gemessen (echter llama-server b12079, Qwen3.5 2B mit MTP-Kopf, 4 CPU-Kerne ohne Grafikkarte, Tokens/s Text / Code):

| Einstellung | Text | Code |
|---|---|---|
| ohne Turbo | 12,4 | 13,4 |
| n-gram-Turbo (Standard) | 13,7 | 34,5 |
| MTP allein, 1 / 2 / 3 Entwurfs-Tokens | 10,9 / 10,1 / 8,9 | 11,8 / 12,1 / 11,0 |
| MTP 2 + n-gram | 15,4 | 31,2 |
| 4 Agents gleichzeitig (Standard) | zusammen 27–28,5 statt 12–14 | |

Auf der CPU kostet jeder Entwurf volle Rechenzeit, MTP allein ist dort ein Verlust – die Abstimmung wählt dann den Standard. Auf einer Grafikkarte sind zusätzliche Positionen je Durchlauf fast gratis; die Hersteller nennen 1,15–1,25× für MoE und bis 1,7× für dichte Modelle. Was auf einem Gerät gewinnt, zeigt nur die Messung dort – deshalb die Abstimmung.

**Dell G16 7630** (RTX 4070 Laptop 8 GB, 32 GB DDR5, i9-13900HX), geschätzt ohne Turbo aus den echten GGUF-Köpfen:

| Modell | je Token gelesen | geschätzt |
|---|---|---|
| Qwen3.6 35B-A3B (Q4_K_S, 21 GB) | 2,1 GB dicht + 0,6 GB Experten | 31–35 Tokens/s; mit MTP und bei Code mehr |
| gpt-oss 20B | 1,0 + 1,3 GB | 31–37 |
| Qwen3.5 9B (ganz auf der Grafikkarte) | 5,1 GB | ~36 |
| Qwen3.8-Flash-Next 125B-A6B (UD-IQ1_S, 66 GB)¹ | 3,8 + 0,65 GB | 5–13 (17 % der Experten auf der SSD); mit 64 GB RAM 22–24 |
| DeepSeek V4.1 Flash 552B (UD-IQ1_S) | 6,6 + 1,8 GB | 0,4–1,5 |

¹ über die Modellsuche ladbar, nicht in der eingebauten Liste: Die Qwen Community License 1.0 verlangt für Anbieter von KI-Coding-Assistenten eine eigene Lizenz (private und interne Nutzung ist frei).

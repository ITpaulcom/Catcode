## Änderungen 5.53.0 – Überholt: Claude auf dem neuesten API-Stand, schnellere Ausgabe, neue Laufzeit und Modelle

**Claude (Anthropic-API, Stand November 2026)**
- **Modelle:** Opus 5.5 (Standard), Sonnet 5.5, Haiku 5.5, Fable 5.2 – mit aktuellen Preisen. Ältere (Sonnet 5, Haiku 4.5, Fable 5.1, Opus 4.8) laufen weiter.
- **Denken:** adaptiv, mit sichtbarer Zusammenfassung der Gedanken (die neuen Modelle zeigen sonst nur eine lange Pause). Stufen gehen als `output_config.effort` hinaus, „max“ bleibt jetzt max (vorher bei Sonnet/Fable/Haiku auf „high“ gekappt). „Aus“ heißt bei Modellen, die immer denken, „so wenig wie möglich“.
- **Gedanken-Blöcke bleiben gültig:** Bei Opus 5.5, Sonnet 5.5, Haiku 5.5 und Fable 5.x sind sie an den Verlauf gebunden. Ändert sich der Verlauf (Verdichten, andere Werkzeuge), lässt die API die älteren Blöcke fallen, statt die Anfrage abzulehnen. Lehnt sie trotzdem ab, schickt Catcoder einmal ohne sie und in dieser Sitzung erst ab da wieder mit; das Denken bleibt an. Neu gespeichert werden auch die Gedanken der Abschlussantwort, damit die Kette keine Lücke hat.
- **Schutzfilter:** Lehnt ein Modell ab (`stop_reason: refusal`), beantwortet ein Ausweichmodell von Anthropic dieselbe Anfrage im selben Aufruf (`fallbacks: "default"`). Auch ein Wechsel mitten in der Antwort wird richtig zerlegt und so zurückgeschickt, wie die API es verlangt. Lehnt die ganze Kette ab, geht es mit dem nächsten Modell der eigenen Ausweichkette weiter. Das abgelehnte Modell gilt dabei nicht als ausgefallen, und Gratis-Modelle springen nicht ungefragt ein.
- **Große Dateien ohne Hänger:** Werkzeug-Eingaben werden gestreamt (`eager_input_streaming`). Eine 2 000-Zeilen-Datei kommt Stück für Stück, statt nach minutenlanger Stille, die der Wächter nach 120 s als „hängt“ wertete. Ohne den Puffer prüft die API das JSON nicht mehr, deshalb parst Catcoder streng und repariert nichts stillschweigend.
- **Nie eine halbe Datei:** Ist eine Antwort an der Längengrenze mitten in einem Werkzeugaufruf abgeschnitten, wird dieser Aufruf nie ausgeführt (vorher konnte die JSON-Reparatur einen halben Datei-Inhalt „gültig“ machen).
- **Schnellmodus** (Verbindungen → API-Keys, Schalter erscheint mit Anthropic-Key): Opus 5.5 und Fable 5.2 schreiben bis zu 2,5× schneller, zum doppelten Preis, und die Kosten werden doppelt gezählt. Ist das eigene Kontingent des Schnellmodus erschöpft, geht es sofort in normaler Geschwindigkeit weiter.
- Was ein Endpunkt im Anthropic-Format nicht kennt (andere Anbieter, Proxys), fällt einzeln weg und bleibt gemerkt. Beta-Zusätze gehen nur an Anthropic selbst.

**Weitere Anbieter:** Kimi K3.1 Pro (seit 23. Oktober 2026 per API, `kimi-k3.1-pro`) in der Liste, sobald ein Kimi-Key hinterlegt ist.

**Eigene Modelle**
- **llama.cpp b12317** (vorher b12079), gemessen mindestens gleich schnell (CPU: Qwen3 0,6B 47–52 statt 40–47 Tokens/s, Qwen3 4B 7,0–7,2 wie zuvor). Neu ist `--moe-cache-cpu-share`: Im Experten-Cache rechnet die CPU seltene Experten selbst, statt sie über PCIe zur Grafikkarte zu schieben. Die Abstimmung probiert das als eigene Variante. Vorhandene Installationen arbeiten weiter und holen die neue Fassung im Hintergrund; sie gilt ab dem nächsten Laden eines Modells, die alte wird beim nächsten Start aufgeräumt. Klappt der Download nicht, bleibt die alte, ohne bei jedem Start neu zu laden.
- **Modellliste:** Qwen3.5 4B mit MTP (statt Qwen3 4B ohne), Qwen3.8 27B mit MTP (statt Qwen3.6 27B), Aufbau jeweils aus den echten GGUF-Köpfen.

**Gemessen:** Engine bereit nach 233 ms, Oberfläche vollständig nach 270–400 ms, Eigenaufwand der Engine je Agenten-Runde 6–14 ms. Die Wartezeit liegt also beim Modell, daher die Hebel oben.

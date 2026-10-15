## Änderungen 5.43.3 – Spiele direkt aus Steam fusionieren

- **Steam-Bibliothek in der Reinraum-Werkstatt:** „Aus Steam wählen“ zeigt alle installierten Spiele (alle Steam-Bibliotheken, auch auf anderen Laufwerken) mit Titelbild und Suche – ein Klick macht das Spiel zur Quelle, zwei oder mehr Spiele schalten automatisch auf **Fusion** (Mauer aus). Laufzeit-Pakete (Proton, Redistributables) werden ausgeblendet; gelesen wird nur, nichts gestartet.
- **Auch im Chat:** „Fusioniere Hollow Knight und Celeste aus Steam“ – die Leitung nimmt die Spiele per Name (`{ steam: "Hollow Knight" }`) oder App-ID; ist eins nicht installiert, kommt eine klare Meldung mit der Liste der installierten Spiele. Das Werkzeug `reinraum` kann die Bibliothek auflisten (action=steam).
- Kleinigkeit: Katzen-Seite wirft keinen Fehler mehr, wenn man sie verlässt, während die 3D-Vorschau noch lädt.

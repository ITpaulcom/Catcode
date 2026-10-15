## Änderungen 5.42.0 – Reinraum-Werkstatt, Aufwandsregler im MaxCode-Design, Feinschliff überall

- **Reinraum-Werkstatt (Clean-Room-Nachbau).** Programme und Spiele verstehen, sauber neu bauen, verbessern, nach Rust
  portieren – oder mehrere zu etwas Neuem **fusionieren** („nimm Spiel A und Spiel B und mach daraus …“, „baue CatCut
  nach und mach den Export schneller“). Start in der Zentrale (Kolben-Symbol bei den Zielen) oder einfach im Hauptchat.
  - **Erkunden:** Je Original schreibt ein Analyst eine Verhaltens-Spezifikation (`spec/<name>.md`): Funktionen,
    Bedienung, Spielmechanik, Dateiformate, Abnahme-Tests, Verbesserungsideen. Das neue Werkzeug `reinraum` liest
    Programme, ohne sie auszuführen: Windows-/Linux-/Mac-Programme (Architektur, Oberfläche, eingebundene Bibliotheken und
    Funktionen → Grafik, Gamepad, Audio, Netzwerk …), Versionsangaben, Packer, Texte und Dateiformate sowie Spiel-Engines
    (Unity, Godot, Unreal, GameMaker, RPG Maker, LÖVE, Electron, Ren'Py, Qt) und Lizenzhinweise.
  - **Mauer (für einen sauberen Neubau):** Leitung, Bau-Team und Prüfer sehen die Originale nie – weder über
    Dateiwerkzeuge noch über Befehle. Jeder Versuch wird abgelehnt und protokolliert. Gebaut wird nur nach der
    Spezifikation.
  - **Spiele fusionieren (Mauer aus):** Bei einer Fusion ist die Mauer standardmäßig aus – das ganze Team nutzt die
    Originale direkt: Skripte und Logik portieren, Level und Daten übernehmen, Grafiken, Sounds und Schriften
    wiederverwenden und zu einem neuen Spiel verschmelzen. Jede übernommene Datei steht in `HERKUNFT.md`. Der Schalter
    „Mauer“ im Dialog (oder `mauer` im Hauptchat) stellt es für jeden Modus um.
  - **Tiefere Analyse:** Klassen, Methoden und Texte aus .NET-Programmen und Unity-Spielen (Assembly-CSharp), Archive
    auflisten und entpacken (ZIP/APK/JAR/.love, Electron .asar, Godot .pck), GameMaker-Namen und -Texte, RPG-Maker-Daten,
    IL2CPP-Bezeichner und **Fundstellen**: wo Logik, Level, Grafik, Audio und Schriften liegen.
  - **Neu bauen:** standardmäßig in Rust (Cargo-Projekt mit Tests; Spiele z. B. mit Bevy/macroquad, Desktop mit egui) –
    Rust wird bei Bedarf automatisch eingerichtet. Andere Sprachen sind wählbar.
  - **Nachvollziehbar:** `REINRAUM.md` hält Verfahren, Quellen, Spezifikationen und abgelehnte Zugriffe fest. Mit Mauer
    wird nur Verhalten übernommen; ohne Mauer ist das Ergebnis für den eigenen Gebrauch (Weitergabe nur mit Rechten an den
    Originalen). Kopierschutz, DRM und verschlüsselte Archive werden nie umgangen.
- **Aufwandsregler im MaxCode-Design** mit vier Stufen: Sparsam (grün, Blatt) · Mittel · Hoch (Funken, lila Glühen) ·
  Maximal (Plasma, Schockwelle, Blitze, glühender Rand ums Eingabefeld). Beim Überfahren oder per Tastatur zeigt ein
  Schild Stufe und Bedeutung. Der Regler springt beim Umschalten nicht mehr seitlich; frühere Einstellungen bleiben gültig.
- **Feinschliff:** gleichnamige Projekte zeigen ihren Ordnernamen; das Suchfeld der Einstellungen ragt nicht mehr
  heraus; Schalter sind im dunklen System-Design gut sichtbar; die Projektansicht passt auf schmale Fenster; Vorschauen
  von Zielen zeigen keine Markdown-Reste mehr; die schlafende Katze sitzt sauber in der Team-Anzeige.

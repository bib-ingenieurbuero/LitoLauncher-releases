# Lito Launcher

Startprogramm des Bassam Ingenieurbüros für **BIB Lito**, **AVALito** und **PDFLito**.
Der Launcher zeigt alle drei Programme mit installierter und neuester Version, installiert sie
und sorgt dafür, dass beim Öffnen eines Programms ein verfügbares Update **sofort** geladen wird.
Er aktualisiert sich auch selbst.

Dieses Repository enthält nur die fertigen Programmdateien (kein Quellcode) und den Programmkatalog
`katalog.json`.

## Installieren

1. Unter **Releases** die neueste Version öffnen und **`LitoLauncher-win-Setup.exe`** herunterladen.
2. Die Datei doppelklicken. Die Installation braucht keine Administratorrechte und dauert wenige Sekunden.
3. Danach startet der Launcher. Die Verknüpfungen „BIB Lito“, „AVALito“ und „PDFLito“ auf dem Desktop
   und im Startmenü öffnen die Programme ab jetzt über den Launcher – immer in der neuesten Version.

**Hinweis zu Windows SmartScreen:** Beim ersten Start kann „Der Computer wurde durch Windows geschützt“
erscheinen, weil das Setup (noch) nicht digital signiert ist. Auf **„Weitere Informationen“** und dann auf
**„Trotzdem ausführen“** klicken.

## Deinstallieren

Windows-Einstellungen → Apps → „Lito Launcher“ → Deinstallieren. Die ursprünglichen Verknüpfungen der
Programme werden dabei wiederhergestellt; BIB Lito, AVALito und PDFLito bleiben installiert.

## katalog.json – zentrale Steuerung aller Geräte

Jeder Lito Launcher liest bei jeder Prüfung (beim Start, alle 4 Stunden, nach dem Aufwachen und beim Öffnen eines
Programms) `katalog.json` aus diesem Repository (Zweig `main`). Änderungen wirken ohne neue Launcher-Version.
Angegebene Felder ersetzen die eingebauten Werte; fehlende bleiben, wie sie sind. Nur Repositorys der Konten
`bib-ingenieurbuero` und `ing-bassam` werden akzeptiert. Ungültige Angaben werden ignoriert (das Programm bleibt,
notfalls mit seinen eingebauten Angaben). Nicht erlaubt: Namen mit Pfadzeichen und die Setup-Schalter `/ALLUSERS`,
`/DIR=`, `/D=`, `/LOG`, `/LOADINF`, `/SAVEINF`, `/GROUP=`, `--installto` (die setzt der Launcher selbst).

| Feld (je Programm) | Bedeutung |
|--------------------|-----------|
| `kanaele` | `{ "stabil": { "vorabversionen": false }, "test": { "vorabversionen": true } }` – „Stabil“ nimmt die als „latest“ markierte Veröffentlichung, „Test“ darf die neueste Vorabversion („Pre-release“) nehmen. Den Kanal wählt jedes Gerät in den Einstellungen (Vorgabe: Stabil). |
| `mindestVersion` | z. B. `"0.3.0"`: Ist eine ältere Version installiert, installiert der Launcher das Update **vor** dem Start – unabhängig von den Einstellungen – mit der Meldung „Pflicht-Update: das Dateiformat hat sich geändert“. Ein laufendes Programm wird nie beendet. Scheitert die Installation, wird dieselbe Version automatisch höchstens einmal in 24 Stunden erneut versucht. |
| `gesperrteVersionen` | z. B. `["0.3.1"]`: Diese Versionen werden nie installiert; ist die neueste gesperrt, nimmt der Launcher die nächstältere zulässige Version. |
| `hinweis` | `{ "id": "bib-2026-11", "text": "Bitte vor dem Update alle Projekte speichern.", "wichtig": false }` – erscheint im Launcher beim Programm; der Anwender kann ihn ausblenden. Ein Hinweis mit **neuer** `id` erscheint wieder. |
| `erstinstallationsArgumente` | Setup-Schalter für die Erstinstallation auf einem neuen Gerät (je Benutzer, ohne Administratorrechte). |
| `ausblenden` | `true` nimmt ein Programm aus dem Launcher. |

Beispiel – Pflicht-Update für AVALito ab 0.4.0, Version 0.4.1 zurückgezogen, Hinweis anzeigen:

```json
{
  "schema": 1,
  "programme": [
    {
      "id": "ava",
      "mindestVersion": "0.4.0",
      "gesperrteVersionen": ["0.4.1"],
      "hinweis": { "id": "ava-2026-11-format", "text": "AVALito 0.4 speichert Projekte in einem neuen Format.", "wichtig": true }
    }
  ]
}
```

(Nur die zu ändernden Felder müssen angegeben werden; die übrigen kommen aus dem eingebauten Katalog.)

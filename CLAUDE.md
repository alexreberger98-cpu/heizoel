# Heizöl-Stand – Hinweise für Claude Code

## Worum es geht
Private Web-App, mit der ich den Heizölstand meines Tanks erfasse und eine
Prognose bekomme, wie lange das Öl noch reicht. Ich nutze sie ausschließlich
auf dem iPhone als Web-App vom Home-Bildschirm. Veröffentlicht wird sie über
GitHub Pages direkt aus dem Branch `main` (Ordner `/`).

## Über mich
Ich bin Programmier-Einsteiger und lerne gerade „Vibe Coding“.
- Antworte und erkläre auf Deutsch, kurz und in einfacher Sprache.
- Erkläre nach jeder Änderung in 3–5 Sätzen, was du geändert hast und warum.
- Fachbegriffe beim ersten Auftreten in einem Satz erklären.
- Wenn eine Aufgabe unklar ist oder mehrere sinnvolle Wege hat: erst fragen.
- Lieber kleine, überschaubare Änderungen als große Umbauten.

## Aufbau
- `index.html` – die komplette App (HTML, CSS und JavaScript in einer Datei)
- `sw.js` – Service Worker für die Offline-Nutzung
- `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`
  – App-Beschreibung und Symbole für den Home-Bildschirm
- Kein Build-Schritt, keine Frameworks, keine externen Bibliotheken.
  Bitte so beibehalten.

## Regeln, die nicht gebrochen werden dürfen
1. **Daten:** Die Ablesungen liegen im `localStorage` unter dem Schlüssel
   `heizoel-tracker-v1` im Format
   `{settings:{capacity, reserve}, readings:[{id, date:"JJJJ-MM-TT", liters, created}]}`.
   Schlüssel und Format nie ändern, ohne vorher zu fragen. Falls doch nötig,
   alte Daten beim Start automatisch übernehmen (Migration), nie verwerfen.
2. **Export/Import:** Das CSV-Format (`Datum;Liter`, plus Zeilen `Tankgröße;…`
   und `Reserve;…`) muss rückwärtskompatibel bleiben. Ältere Exporte müssen
   sich immer importieren lassen.
3. **Updates:** Bei jeder Änderung an einer Datei die Versionsnummer in `sw.js`
   (`const CACHE = "heizoel-v…"`) um eins erhöhen. Kommen Dateien hinzu oder
   fallen weg, die Liste `FILES` in `sw.js` anpassen.
4. **Handy zuerst:** Die App muss auf dem iPhone (Safari, ca. 375 px Breite)
   gut bedienbar sein. Eingabefelder und Knöpfe mindestens 44 px hoch,
   nichts darf sich überlappen. Hell- und Dunkelmodus unterstützen.
5. **Sprache:** Alle Texte in der App auf Deutsch, mit typografischen
   Anführungszeichen („ “) und Apostrophen (’).
6. **Privatsphäre:** Keine Analyse-, Tracking- oder Werbedienste einbauen.
   Externe Anfragen nur, wenn ich sie ausdrücklich beauftragt habe.

## Wie die Prognose funktioniert
- Abschnitte zwischen zwei Ablesungen mit sinkendem Stand zählen als
  Verbrauch; steigt der Stand, gilt das als Betankung und der Abschnitt
  wird übersprungen.
- `learnProfile()` lernt daraus einen Verbrauch in Litern pro Tag für jeden
  Monat. Monate mit wenig Daten werden mit der Standardkurve `MONTH_SHARE`
  gemischt (`PRIOR_DAYS`).
- Warmwasser läuft im Sommer über Solarthermie, daher ist der
  Sommerverbrauch sehr niedrig. Das ist korrekt und kein Fehler.

## Prüfen vor dem Abschluss
- JavaScript auf Syntaxfehler prüfen (z. B. Skriptteil mit `node --check`).
- Wenn möglich mit Testdaten durchspielen: Ablesung eintragen, Import,
  Export, Neuladen – die Daten müssen erhalten bleiben.
- Im Pull Request kurz auf Deutsch beschreiben, was sich geändert hat und
  wie ich es am iPhone prüfen kann.

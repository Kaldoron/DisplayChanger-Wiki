# Changelog

🇬🇧 [English](Changelog.md) | 🇩🇪 **Deutsch**

Nutzerseitige Änderungen an DisplayChanger, neueste zuerst. Das Plugin nutzt keine
Versionsnummern für Releases, daher sind die Einträge nach Datum gruppiert.

## 2026-08-24 – 2026-08-26

**Compositions auf WorldEdit-Basis neu gebaut**
Compositions (gespeicherte Display-Layouts) waren eine Zeit lang deaktiviert und wurden jetzt
komplett neu gebaut:

- Die Flächenauswahl läuft jetzt über die bekannten WorldEdit-Befehle `//pos1`/`//pos2` statt über
  ein eigenes Auswahl-System.
- Beim Speichern einer Composition werden jetzt Baurechte über die *gesamte* ausgewählte Fläche
  geprüft, nicht mehr nur an deiner Standposition.
- Composition-Namen werden validiert (Kleinbuchstaben, Zahlen, `-`, `_`, max. 32 Zeichen).
- Neu: `/display composition list` und `/display composition delete <name>`.

Siehe [Compositions (Layouts speichern)](Readme.de.md#compositions-layouts-speichern).

**Grafisches Menü (GUI)**
Alle Display-Einstellungen – Position, Rotation, Skalierung, Helligkeit, Leuchtfarbe,
Textoptionen und mehr – lassen sich jetzt über ein klickbares Inventar-Menü bearbeiten
(`/display gui <menü>`). Das ersetzt die bisherige Abhängigkeit vom externen Plugin "Genesis Boss
Shop". Siehe [Das grafische Menü (GUI) nutzen](Readme.de.md#das-grafische-menü-gui-nutzen).

**Undo**
`/display undo` macht die letzte Änderung am aktuell bearbeiteten Display rückgängig. Siehe
[Rückgängig machen (Undo)](Readme.de.md#rückgängig-machen-undo).

**Leuchtfarbe zurücksetzen**
`/display glowcolor reset` setzt eine eigene Leuchtfarbe zurück auf den Standard, ohne das
Display neu spawnen zu müssen.

**WorldGuard und WorldEdit sind jetzt optional**
Das Plugin erkennt beim Start, ob WorldGuard und WorldEdit installiert sind. Fehlt WorldGuard,
werden Regions-/Baurechteprüfungen einfach übersprungen; fehlt WorldEdit, ist nur die
Composition-Funktion deaktiviert (mit einer klaren Meldung im Spiel) – alles andere funktioniert
weiterhin normal.

**Behoben**
- `/display brightness reset` hat die Helligkeit nicht korrekt zurückgesetzt.

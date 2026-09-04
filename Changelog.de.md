# Changelog

🇬🇧 [English](Changelog.md) | 🇩🇪 **Deutsch**

Nutzerseitige Änderungen an DisplayChanger, neueste zuerst. Das Plugin nutzt keine
Versionsnummern für Releases, daher sind die Einträge nach Datum gruppiert.

## 2026-09-04

**Display-Gruppen mit ausgelaufenem Blickwinkel reparieren**
Neuer Befehl `/display selection fix`. Manche vor diesem Fix gebauten Displays konnten die
Blickrichtung, in die du im Moment des Spawnens geschaut hast, unsichtbar in ihre Positionsdaten
übernommen haben – solange das Teil genau an seinem Bauort stehen blieb, sah es weiterhin korrekt
aus, aber sobald es in eine Composition gespeichert und wieder geladen wurde, konnte es plötzlich in
eine falsche Richtung zeigen. Registriere die betroffene Gruppe mit `/display selection register`
und führe dann `/display selection fix` aus – erkennt und repariert jedes Display der Gruppe, ohne
die aktuelle Optik zu verändern, du musst also nichts neu bauen. Siehe [Eine Gruppe von Displays
bewegen und rotieren](Readme.de.md#eine-gruppe-von-displays-bewegen-und-rotieren).

**Behoben**
- Beim Spawnen eines Displays mit einem generischen Item (alles außer Spielerkopf oder
  Namensschild) in der Haupthand konnte die Blickrichtung im Spawn-Moment unsichtbar in das Display
  übernommen werden – ohne sichtbare Auswirkung, bis das Display später in eine Composition
  gespeichert und wieder geladen wurde, wo es dann anders ausgerichtet sein konnte als vorher. Neu
  gespawnte Displays sind davon nicht mehr betroffen; siehe oben `/display selection fix`, um
  bestehende zu reparieren.

## 2026-09-02

**Display am Block-Raster zentrieren**
Der neue Befehl `/display center` setzt die Position des ausgewählten Displays auf die Ecke der
Blockzelle, in der es sich gerade befindet, und passt die Translation an die aktuelle Skalierung
an, sodass es mittig in dieser Zelle sitzt. Praktisch für präzise Positionierung, nachdem ein
Display verschoben wurde – funktioniert bei Block-, Item- und TextDisplays. Siehe
[Transformation: Bewegen, Skalieren, Rotieren](Readme.de.md#transformation-bewegen-skalieren-rotieren).

## 2026-09-01

**Selektieren ohne WorldEdit-Berechtigung**
Die neuen Befehle `/display selection pos1` und `/display selection pos2` setzen einen
WorldEdit-Selektionspunkt auf den Block, auf dem du gerade stehst – dieselbe Selektion, die auch
`//pos1`/`//pos2` setzen würde, aber gesteuert nur über DisplayChangers eigenes Recht
`displaychanger.selection.pos` statt über WorldEdits eigenes. Nützlich für Spieler, die
Compositions speichern oder eine Selection-Gruppe registrieren können sollen, ohne WorldEdits
eigene Baurechte zu bekommen. Siehe [Compositions (Layouts
speichern)](Readme.de.md#compositions-layouts-speichern) und [Eine Gruppe von Displays bewegen und
rotieren](Readme.de.md#eine-gruppe-von-displays-bewegen-und-rotieren).

**Behoben**
- `/display selection rotate` konnte mehrteilige Konstruktionen zerlegen, statt sie als eine feste
  Gruppe zu rotieren: Displays, deren Position innerhalb des Baus über einen Feinversatz definiert
  ist (nicht nur über ihre Basisposition), landeten übereinandergestapelt mit uneinheitlicher
  Ausrichtung, statt sich gemeinsam zu drehen. Beim Rotieren einer Selektion bleiben Position und
  Ausrichtung jedes Teils jetzt korrekt aneinander gekoppelt. Siehe [Eine Gruppe von Displays
  bewegen und rotieren](Readme.de.md#eine-gruppe-von-displays-bewegen-und-rotieren).
- `/display rotation add` (auch in der `<x> <y> <z>`-Form) konnte von der gewünschten Achse
  abweichen, sobald ein Display bereits mehr als eine Rotation erhalten hatte – z. B. drehte ein
  Yaw nach einem vorherigen Pitch um die bereits gekippte eigene Achse des Displays statt um die
  senkrechte Weltachse. Aufeinanderfolgende Rotationen über verschiedene Achsen verhalten sich
  jetzt konsistent. Siehe [Transformation: Bewegen, Skalieren,
  Rotieren](Readme.de.md#transformation-bewegen-skalieren-rotieren).

## 2026-08-31

**Eine Gruppe von Displays gemeinsam bewegen und rotieren**
Neue `/display selection`-Befehle behandeln jedes Display innerhalb einer WorldEdit-Selektion als
eine feste Gruppe:

- `/display selection register` – registriert jedes Display innerhalb deiner aktuellen
  WorldEdit-Selektion (`//pos1`/`//pos2`) als Gruppe und speichert die Mitte der Selektion als
  festen Pivotpunkt.
- `/display selection move <dx> <dy> <dz>` – bewegt die gesamte Gruppe um einen relativen Versatz.
- `/display selection rotate <yaw|pitch|roll|all> <degrees>` – rotiert die gesamte Gruppe
  gemeinsam um ihren Pivotpunkt.
- `/display selection unregister` – hebt die Gruppenregistrierung auf.

`/display undo` macht jetzt zusätzlich zu Einzeländerungen auch die letzte Gruppen-Bewegung/
-Rotation rückgängig – je nachdem, was zuletzt passiert ist. Siehe [Eine Gruppe von Displays
bewegen und rotieren](Readme.de.md#eine-gruppe-von-displays-bewegen-und-rotieren).

**Item- oder Block-Display beim Spawnen erzwingen**
`/display spawn <feet|front|head> <material> <item|block>` erlaubt es, ein Material, das beides
unterstützt – etwa einen Block –, statt des üblichen Block-Displays als Item-Display (dessen
flaches Icon) zu spawnen, oder umgekehrt. Siehe [Displays spawnen](Readme.de.md#displays-spawnen).

**Behoben**
- `/display duplicate` hat bisher deine gesamte Liste registrierter Displays durch nur die neue
  Kopie ersetzt, sodass du zu vorher registrierten Displays nur durch erneutes Registrieren
  zurückwechseln konntest. Die Kopie wird jetzt stattdessen an diese Liste angehängt, der Rest
  deiner registrierten Displays bleibt erhalten.

## 2026-08-24 – 2026-08-26

**Transparenz bei der Hintergrundfarbe**
`/display text backgroundcolor` akzeptiert jetzt einen optionalen Alpha-Wert (`0`–`255`) nach
einem Colorname oder einer RGB-Farbe, und bei HEX als zwei zusätzliche Hex-Stellen
(`#RRGGBBAA`) – `0` macht den Hintergrund vollständig unsichtbar. `reset` stellt weiterhin nur
Minecrafts durchscheinenden Standardhintergrund wieder her und blendet ihn nicht aus; für echte
Unsichtbarkeit Alpha `0` verwenden. Siehe [Text-Displays](Readme.de.md#text-displays).

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

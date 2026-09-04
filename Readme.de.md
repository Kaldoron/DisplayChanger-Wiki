# DisplayChanger

🇬🇧 [English](Readme.md) | 🇩🇪 **Deutsch**

DisplayChanger ist ein Paper-Server-Plugin, mit dem du **Display-Entities** (Item-Displays,
Block-Displays und Text-Displays) spawnen, bearbeiten und speichern kannst – ganz ohne externen
Editor, alles direkt im Spiel per Chat-Befehl oder über ein grafisches Menü.

Nutze **/display** für jeden Befehl unten.

> Brauchst du eine Berechtigung dafür? Frag deinen Server-Admin – die vollständige Liste der
> Permission-Nodes findest du unter [Berechtigungen](#berechtigungen).

## Inhaltsverzeichnis

- [Das grafische Menü (GUI) nutzen](#das-grafische-menü-gui-nutzen)
- [Displays spawnen](#displays-spawnen)
- [Displays zum Bearbeiten auswählen](#displays-zum-bearbeiten-auswählen)
- [Rückgängig machen (Undo)](#rückgängig-machen-undo)
- [Duplizieren und Löschen](#duplizieren-und-löschen)
- [Transformation: Bewegen, Skalieren, Rotieren](#transformation-bewegen-skalieren-rotieren)
- [Darstellung: Ausrichtung, Sichtweite, Schatten, Helligkeit](#darstellung-ausrichtung-sichtweite-schatten-helligkeit)
- [Leuchten (Glow)](#leuchten-glow)
- [Text-Displays](#text-displays)
- [Compositions (Layouts speichern)](#compositions-layouts-speichern)
- [Eine Gruppe von Displays bewegen und rotieren](#eine-gruppe-von-displays-bewegen-und-rotieren)
- [Berechtigungen](#berechtigungen)
- [Regionsschutz (WorldGuard)](#regionsschutz-worldguard)
- [Resource Pack: durchsichtiges GUI-Panel](#resource-pack-durchsichtiges-gui-panel)
- [Fehlerbehebung](#fehlerbehebung)
- [Changelog](Changelog.de.md)
- [Beispiele - Kreationen von Usern](Examples.de.md)

## Das grafische Menü (GUI) nutzen

Vieles von dem, was unten beschrieben ist, gibt es auch als klickbares Inventar-Menü – praktisch,
wenn du dir keine Befehle merken willst. Öffne es mit `/display gui <menü>`, ausgehend vom
Hauptmenü:

```
/display gui display_basics
```

Von dort aus springst du über die Buttons in der obersten Reihe zwischen den Untermenüs, und jedes
Untermenü hat einen "Zurück"-Button zu Basics. Jeder Abschnitt unten zeigt das passende GUI-Menü
direkt neben den zugehörigen Befehlen, dazu einen Hinweis, wo sich GUI und Befehle unterscheiden –
die GUI ist schnell und arbeitet mit festen Voreinstellungen (feste Schrittgrößen, vordefinierte
Farben, …), während die Befehle jeden exakten Wert annehmen und ein paar Dinge abdecken, für die es
in der GUI gar keinen Button gibt (z. B. `/display duplicate`, `/display undo`, den tatsächlichen
Text eines Text-Displays setzen oder eigene RGB-/HEX-Farben).

> **Hinweis:** Die Menü-Buttons sind aktuell unabhängig von deiner `config.yml`-Spracheinstellung
> auf Deutsch beschriftet – die Chat-Nachrichten des Plugins folgen aber weiterhin ganz normal
> dieser Einstellung.

Der durchsichtige Kisten-Hintergrund in den Screenshots stammt von einem optionalen Resource Pack
– siehe [Resource Pack: durchsichtiges GUI-Panel](#resource-pack-durchsichtiges-gui-panel).

## Displays spawnen

**`/display spawn <feet|front|head> [material] [item|block]`**

Spawnt ein Display an deinen Füßen, an deinem Kopf oder vor dir.

- Ohne Ortsangabe spawnt das Display vor dir.
- Ohne Materialangabe wird das Item in deiner Haupthand verwendet.
- Was du bekommst, hängt vom Item/Material ab:
  - Ein **Block-Item** (z. B. `STONE`) → ein **Block-Display**.
  - Ein beliebiges anderes **Item** (z. B. `DIAMOND`) → ein **Item-Display**, das dieses Item zeigt.
  - Ein **Spielerkopf** mit Textur → ein **Item-Display**, das genau diesen Skin zeigt.
  - Ein **Namensschild** → ein **Text-Display**. Hat das Namensschild einen benutzerdefinierten
    Namen (am Amboss umbenannt), wird dieser Name zum Text des Displays; sonst wird ein
    Platzhaltertext verwendet.
- Bei einem explizit angegebenen Material, das beide Formen unterstützt (jedes Block-Item), kannst
  du am Ende `item` oder `block` anhängen, um zu erzwingen, welche Form du bekommst – z. B. spawnt
  `/display spawn front stone item` das flache Item-Icon von Stein statt eines 3D-Blocks. Eine
  Kombination zu erzwingen, die das Material nicht unterstützt (z. B. `block` bei einem
  Nicht-Block-Item), gibt einen Fehler statt etwas zu spawnen.

Nach dem Spawnen ist das Display automatisch ausgewählt und bereit zur Bearbeitung – ein separater
"Auswahl"-Schritt danach ist nicht nötig.

*GUI:* Das Menü `display_basics` hat einen Button pro Spawn-Ort (Füße/Vorne/Kopf), immer mit dem
Item in deiner Hand – genau wie der Befehl.

## Displays zum Bearbeiten auswählen

DisplayChanger hält dich immer bei genau einem Display "eingeloggt". Was auch immer du spawnst,
registrierst oder wechselst, wird zum aktiven Display für jeden Bearbeitungsbefehl unten.

- **`/display register`** – registriert jedes Display im Umkreis von 5 Blöcken (konfigurierbar) um
  dich herum und wählt das erste davon aus.
- **`/display switch next`** / **`/display switch previous`** – wechselt zwischen deinen
  registrierten Displays. Das aktuell ausgewählte Display wird kurz hervorgehoben.
- **`/display infoall`** – listet alle deine registrierten Displays im Chat auf.
- **`/display info`** – zeigt detaillierte Informationen (Position, Skalierung, Rotation,
  Helligkeit, Leuchten usw.) zum aktuell ausgewählten Display.
- **`/display unregister`** – entfernt deine registrierten Displays und verlässt den
  Bearbeitungsmodus.

*GUI:* Das Menü `display_basics` hat passende Buttons für alles oben (Registrieren,
Vorheriges/Nächstes wechseln, Info für alle Displays, Info für das ausgewählte, und Unregister) –
volle Parität zu den Befehlen, in keine Richtung feiner steuerbar.

![Display - Basics menu](gui-screenshots/display-basics.png)

## Rückgängig machen (Undo)

**`/display undo`**

Macht die letzte Änderung am gerade bearbeiteten Display rückgängig, Schritt für Schritt. Jede
erfolgreiche Änderung (Skalierung, Rotation, Bewegung, Helligkeit, Sichtweite, Schatten,
Ausrichtung, Leuchten, Leuchtfarbe und Textänderungen) wird gemerkt, solange du dieses Display
bearbeitest.

Der Undo-Verlauf wird gelöscht, sobald du zu einem anderen Display wechselst, `/display
unregister` ausführst oder dich abmeldest – Undo gilt immer nur für das Display, das du gerade
bearbeitest, und nur für die aktuelle Sitzung.

*GUI:* Es gibt nirgendwo im Menü einen Undo-Button – Undo ist nur per Befehl verfügbar.

## Duplizieren und Löschen

- **`/display duplicate`** – spawnt eine vollständige Kopie des aktuell ausgewählten Displays
  (gleiches Aussehen, Transformation, Leuchten, Schatten usw.) vor dir, fügt sie zu deinen
  registrierten Displays hinzu und wählt die Kopie aus.
- **`/display delete`** – entfernt das aktuell ausgewählte Display dauerhaft.

*GUI:* `display_basics` hat einen Löschen-Button (siehe Screenshot oben), aber keinen
Duplizieren-Button – Duplizieren ist nur per Befehl möglich.

## Transformation: Bewegen, Skalieren, Rotieren

**Bewegen**

`/display move <x|y|z> <value>` bewegt das Display entlang einer einzelnen Achse.
`/display move <x> <y> <z>` bewegt es entlang aller drei Achsen gleichzeitig.

Stelle dem Wert ein `~` voran für eine relative Bewegung (z. B. bewegt `~1` einen Block weiter);
ohne `~` ist der Wert eine absolute Koordinate.

*GUI:* Das Menü `display_movement` verschiebt das Display in festen Schritten von 0.001, 0.01, 0.1
oder 1 Block pro Klick nach West/Ost, Nord/Süd und Oben/Unten. Ideal für schnelles, präzises
Nachjustieren, aber der Befehl ist flexibler: Er erlaubt *jede* Schrittgröße, lässt dich eine
exakte absolute Koordinate eintippen statt einer relativen Bewegung, und kann alle drei Achsen in
einem einzigen Aufruf bewegen.

![Display - Movement menu](gui-screenshots/display-movement.png)

**Zentrieren**

`/display center` setzt die Position des Displays auf die Ecke der Blockzelle, in der es sich
gerade befindet, und passt die Translation an die aktuelle Skalierung an, sodass es mittig in
dieser Zelle sitzt – praktisch, wenn ein Display nach dem Verschieben wieder exakt am Block-Raster
ausgerichtet werden soll, statt auf einer beliebigen fraktionalen Position zu stehen. Funktioniert
bei Block-, Item- und TextDisplays; nur per Befehl, kein GUI-Äquivalent.

**Skalierung**

`/display scale <set|add> <x|y|z|all> <value>` setzt die Skalierung auf der angegebenen Achse
(oder allen Achsen gleichzeitig) oder addiert dazu.
`/display scale reset` setzt die Skalierung zurück auf die Werte, mit denen das Display gespawnt
wurde.

*GUI:* Das Menü `display_scale` vergrößert/verkleinert Breite, Höhe und Tiefe unabhängig
voneinander (oder einheitlich) in festen Schritten von 0.1 oder 1, plus ein Reset-Button. Es
addiert immer nur zur aktuellen Skalierung – direkt auf einen exakten Wert springen (`set`) geht
nur per Befehl.

![Display - Scale menu](gui-screenshots/display-scale.png)

**Rotation**

`/display rotation <set|add> <pitch|roll|yaw|all> <value>` setzt die Rotation auf der angegebenen
Achse oder addiert dazu.
`/display rotation <set|add> <x> <y> <z>` setzt Yaw, Pitch und Roll auf einmal oder addiert dazu.
`/display rotation reset` setzt die Rotation zurück auf die Werte, mit denen das Display gespawnt
wurde.

*GUI:* Das Menü `display_rotation` kippt/dreht das Display in festen Schritten von 1° oder 10° pro
Klick (Pitch, Yaw und Roll haben jeweils eigene Buttons), plus ein Reset-Button. Wie bei der
Skalierung wird nur zur aktuellen Rotation addiert – einen exakten Winkel setzen oder alle drei
Achsen gleichzeitig setzen geht nur per Befehl.

![Display - Rotation menu](gui-screenshots/display-rotation.png)

## Darstellung: Ausrichtung, Sichtweite, Schatten, Helligkeit

**Ausrichtung (Billboard)**

`/display billboard <mode>` steuert, wie sich das Display zu dir dreht:

| Modus        | Verhalten                                          |
| ------------ | --------------------------------------------------- |
| `fixed`      | Display dreht sich nie – bleibt genau in der gesetzten Rotation. |
| `vertical`   | Display dreht sich zu dir, aber nur um die vertikale (Auf/Ab-)Achse. |
| `horizontal` | Display dreht sich zu dir, aber nur um die horizontale Achse. |
| `center`     | Display richtet sich auf jeder Achse vollständig zu dir aus. |

**Sichtweite**

`/display viewrange <value>` legt fest, wie weit entfernt (als Prozentwert) das Display noch
sichtbar ist – kombiniert sowohl die server- als auch die client-seitige Renderdistanz (der
kleinere Wert gewinnt).

**Schatten**

`/display shadowradius <value>` legt die Größe des Schattens des Displays fest.
`/display shadowstrength <value>` legt die Stärke/Dunkelheit des Schattens fest.
Ein Schatten ist erst sichtbar, wenn **beide** Werte größer als 0 sind.

*GUI:* Das Menü `display_settings` deckt alle drei oben an einer Stelle ab – Ausrichtung hat alle
vier Modi als Buttons (volle Parität zum Befehl), während Sichtweite (25/50/75/100 %) und
Schattenradius/-stärke (Preset-Buttons) nur eine Handvoll fester Werte plus Reset bieten. Jeder
andere Prozent- oder Schattenwert braucht den Befehl.

![Display - View Settings menu](gui-screenshots/display-settings.png)

**Helligkeit**

`/display brightness <value>` setzt die Helligkeit des Displays, von `0` (komplett dunkel) bis
`15` (voll beleuchtet, ignoriert das umgebende Lichtlevel).
`/display brightness reset` setzt sie zurück auf die Helligkeit, mit der das Display gespawnt
wurde.

*GUI:* Das Menü `display_brightness` hat einen Button pro Helligkeitsstufe (0–15) plus Reset – da
Helligkeit in Minecraft ohnehin nur 16 mögliche Werte hat, deckt die GUI hier bereits den vollen
Bereich ab.

![Display - Brightness menu](gui-screenshots/display-brightness.png)

## Leuchten (Glow)

`/display glow <true|false>` schaltet den Umriss-Leuchteffekt an oder aus.

`/display glowcolor <colorname>` setzt die Leuchtfarbe auf eine der vordefinierten
Farbstoff-Farben: `black`, `blue`, `brown`, `cyan`, `gray`, `green`, `light_blue`, `light_gray`,
`lime`, `magenta`, `orange`, `pink`, `purple`, `red`, `white`, `yellow`.

`/display glowcolor rgb <r> <g> <b>` setzt eine eigene RGB-Leuchtfarbe.
`/display glowcolor hex <#RRGGBB>` setzt eine eigene HEX-Leuchtfarbe.
`/display glowcolor reset` setzt die Leuchtfarbe auf den Standard zurück.

Leuchten und Leuchtfarbe sind **bei Text-Displays nicht verfügbar** – Text-Displays haben keinen
Umriss, der leuchten könnte.

*GUI:* Das Menü `display_glowcolors` schaltet Leuchten an/aus und bietet alle 16 vordefinierten
Farbstoff-Farben plus Reset – eigene RGB- oder HEX-Farben gibt es nur per Befehl.

![Display - Glowcolor menu](gui-screenshots/display-glowcolors.png)

## Text-Displays

Diese Befehle funktionieren nur, solange ein **Text-Display** ausgewählt ist (siehe [Displays
spawnen](#displays-spawnen) – halte ein Namensschild in der Hand, um eines zu spawnen).

- **`/display text settext <text>`** – setzt den Text des Displays und ersetzt dabei alles, was
  vorher da stand.
- **`/display text addtext <text>`** – hängt Text an den bestehenden Text an, statt ihn zu
  ersetzen.

  Beide Befehle verstehen [MiniMessage](https://docs.advntr.dev/minimessage/format.html)-
  Formatierung, du kannst also z. B. schreiben:

  ```
  /display text settext <gold><bold>Willkommen!
  /display text addtext <newline><gray>auf dem Server
  ```

- **`/display text alignment <left|center|right>`** – setzt die Textausrichtung.
- **`/display text linewidth <set|add> <value>`** – setzt die maximale Zeilenbreite, bevor der
  Text umbricht, oder addiert dazu.
- **`/display text seethrough <true|false>`** – macht den Text durch Wände hindurch sichtbar
  (`true`) oder nur bei Sichtkontakt (`false`).

*GUI:* Das Menü `display_text` deckt Zeilenbreite (nur ±5/±10-Schritte, kein exaktes `set`),
Sichtbarkeit durch Wände (volle Parität) und Ausrichtung (volle Parität) ab. **Es gibt keinen
Button, um den Text selbst zu setzen oder zu ergänzen** – `settext`/`addtext` gibt es nur per
Befehl.

![Display - Text-Displays menu](gui-screenshots/display-text.png)

**Deckkraft**

`/display text opacity <0-100>` legt fest, wie deckend der Text ist, als Prozentwert.

*GUI:* Das Menü `display_text_opacity` bietet feste 10-%-Schritte (0/10/20/…/100). Jeder andere
exakte Prozentwert braucht den Befehl.

![Display - Opacity menu](gui-screenshots/display-text-opacity.png)

**Hintergrundfarbe**

`/display text backgroundcolor <colorname [alpha]|rgb <r> <g> <b> [alpha]|hex <#RRGGBB[AA]>|reset>`
setzt die Farbe des Hintergrund-Panels hinter dem Text. Colorname, RGB und HEX akzeptieren alle
einen optionalen Alpha-Wert von `0` (vollständig unsichtbar) bis `255` (voll deckend, Standard) –
bei HEX sind das zwei zusätzliche Stellen am Ende (`#RRGGBBAA`) statt eines eigenen Arguments.

`reset` stellt Minecrafts eingebauten durchscheinenden Hintergrund wieder her – das macht den
Hintergrund **nicht** unsichtbar. Um den Hintergrund vollständig auszublenden, setze stattdessen
Alpha auf `0`, z. B. `/display text backgroundcolor black 0` oder
`/display text backgroundcolor hex #00000000`.

*GUI:* Das Menü `display_text_backgroundcolors` bietet dieselben 16 vordefinierten Farben plus
Reset – eigene RGB-/HEX-Farben und Alpha gibt es nur per Befehl.

![Display - Backgroundcolor menu](gui-screenshots/display-text-backgroundcolors.png)

## Compositions (Layouts speichern)

Mit Compositions kannst du eine Gruppe von Displays als benanntes, wiederverwendbares Layout
speichern – ähnlich einem WorldEdit-Schematic, nur beschränkt auf Display-Entities. Benötigt das
[WorldEdit](https://enginehub.org/worldedit)-Plugin. Für Compositions gibt es kein GUI-Menü –
alles unten ist nur per Befehl verfügbar.

1. Wähle mit WorldEdit den Bereich um die Displays aus, die du speichern willst (`//pos1`,
   `//pos2`).
2. Führe **`/display composition save <name>`** aus. Jedes Display innerhalb dieser Selektion wird
   erfasst – vollständige Position/Rotation/Skalierung, Leuchten, Schatten, Helligkeit, Sichtweite
   und (bei Text-Displays) Text/Ausrichtung/Hintergrundeinstellungen.
3. Stell dich später dorthin, wo du das Layout haben willst, und führe **`/display composition
   load <name>`** aus – das gesamte Layout wird relativ zu deiner aktuellen Position und
   Blickrichtung eingefügt, sieht also unabhängig vom Ort gleich aus.

Weitere Composition-Befehle:

- **`/display composition list`** – listet deine gespeicherten Compositions auf.
- **`/display composition delete <name>`** – löscht eine deiner gespeicherten Compositions.

Hinweise:

- Compositions sind privat – du kannst nur deine eigenen speichern, laden, auflisten oder löschen.
- Namen dürfen nur Kleinbuchstaben, Zahlen, `-` und `_` enthalten (max. 32 Zeichen).
- Zum Speichern brauchst du Baurechte im ausgewählten Bereich (siehe
  [Regionsschutz](#regionsschutz-worldguard)) und ein installiertes WorldEdit; Laden, Auflisten und
  Löschen einer Composition benötigen kein WorldEdit.
- Hast du WorldEdits eigenes Selektionsrecht nicht? Nutze `/display selection pos1` und
  `/display selection pos2` statt `//pos1`/`//pos2` – sie setzen dieselbe Selektion von deiner
  aktuellen Position aus, gesteuert nur über `displaychanger.selection.pos`.

## Eine Gruppe von Displays bewegen und rotieren

Zusätzlich zum Bearbeiten einzelner Displays kannst du jedes Display innerhalb einer
WorldEdit-Selektion als feste Gruppe registrieren und alle gemeinsam als ein starres Objekt bewegen
oder rotieren. Benötigt das [WorldEdit](https://enginehub.org/worldedit)-Plugin. Dafür gibt es kein
GUI-Menü – alles unten ist nur per Befehl verfügbar.

1. Wähle mit WorldEdit den Bereich um die Displays aus, die du als Gruppe bewegen oder rotieren
   willst (`//pos1`, `//pos2`).
2. Führe **`/display selection register`** aus. Jedes Display innerhalb dieser Selektion wird Teil
   der Gruppe; die Mitte der Selektion wird als Pivotpunkt der Gruppe gespeichert.
3. **`/display selection move <dx> <dy> <dz>`** – bewegt jedes Display der Gruppe um den
   angegebenen Versatz (relativ, in Blöcken).
4. **`/display selection rotate <yaw|pitch|roll|all> <degrees>`** – rotiert jedes Display der
   Gruppe gemeinsam um den bei der Registrierung gespeicherten Pivotpunkt; der Pivot bewegt sich
   mit der Gruppe mit.
5. **`/display selection fix`** – repariert Displays, die vor dem 2026-09-04-Fix gebaut wurden und
   dabei unsichtbar die Blickrichtung im Spawn-Moment in ihre Positionsdaten übernommen haben. Ein
   solches Display sah weiterhin korrekt aus, solange es genau an seinem Bauort stehen blieb, aber
   sobald es in eine Composition gespeichert und wieder geladen wurde, konnte es plötzlich in eine
   falsche Richtung zeigen. `fix` erkennt und repariert jedes betroffene Display der Gruppe, ohne
   die aktuelle Optik zu verändern – nach dem Fix gespawnte Displays sind nie betroffen, du
   brauchst das also nur für ältere Bauten.

Weitere Selection-Befehle:

- **`/display selection unregister`** – hebt die aktuelle Gruppenregistrierung auf.

Hinweise:

- `/display undo` macht auch eine Gruppen-Bewegung/-Rotation rückgängig, genau wie eine
  Einzeländerung – es wählt automatisch, was zuletzt passiert ist, eine Einzeländerung oder eine
  Gruppenaktion.
- Es gelten zwei Server-Grenzen, beide vom Admin in `config.yml` konfigurierbar: Eine Selektion
  kann höchstens eine festgelegte Anzahl Displays auf einmal betreffen (Standard 300), und eine
  einzelne Bewegung ist auf eine festgelegte Distanz pro Achse begrenzt (Standard 64 Blöcke).
- Zum Registrieren einer Gruppe brauchst du Baurechte über den gesamten ausgewählten Bereich; zum
  Bewegen brauchst du Baurechte sowohl an der aktuellen als auch an der Zielposition, und zum
  Rotieren Baurechte über den Bereich, den die Gruppe dabei überstreicht (siehe
  [Regionsschutz](#regionsschutz-worldguard)).
- Hast du WorldEdits eigenes Selektionsrecht nicht? Nutze `/display selection pos1` und
  `/display selection pos2` statt `//pos1`/`//pos2` – sie setzen dieselbe Selektion von deiner
  aktuellen Position aus, gesteuert nur über `displaychanger.selection.pos`.

## Berechtigungen

Frag deinen Server-Admin danach, wenn ein Befehl bei dir nicht funktioniert:

| Berechtigung                         | Gewährt Zugriff auf                                    |
| ------------------------------------- | ---------------------------------------------------- |
| `displaychanger.default`             | Den Befehl `/display` und alle Unterbefehle, inklusive der GUI. |
| `displaychanger.composition.save`    | `/display composition save` und `/display composition delete`. |
| `displaychanger.composition.load`    | `/display composition load` und `/display composition list`. |
| `displaychanger.selection.register`  | `/display selection register` und `/display selection unregister`. |
| `displaychanger.selection.move`      | `/display selection move`. |
| `displaychanger.selection.rotate`    | `/display selection rotate` und `/display selection fix`. |
| `displaychanger.selection.pos`       | `/display selection pos1` und `/display selection pos2` – setzt einen WorldEdit-Selektionspunkt ohne WorldEdits eigenes Recht. |
| `displaychanger.reload`              | `/display reload` (nur Admin: lädt `config.yml` neu). |

## Regionsschutz (WorldGuard)

Ist auf dem Server [WorldGuard](https://worldguard.enginehub.org/) installiert, benötigt jede
Display-Aktion (Spawnen, Bearbeiten, Löschen, Composition speichern, …) Baurechte in der Region, in
der du stehst – du musst Owner, Member der Region oder Server-Operator sein. Ohne WorldGuard gelten
nur die oben genannten Berechtigungen.

## Resource Pack: durchsichtiges GUI-Panel

Das [grafische Menü](#das-grafische-menü-gui-nutzen) öffnet sich als großes Kisten-Inventar, das
die Sicht auf das Display, das du gerade bearbeitest, teilweise verdecken kann. Dieses Repository
enthält ein optionales Resource Pack, das diese Kisten-Textur durchsichtig macht, damit du das
Display hinter dem Menü sehen kannst, während du es bearbeitest:

[`invisible-chests-datapack/InvisibleChests_1.21.11.zip`](invisible-chests-datapack/InvisibleChests_1.21.11.zip)

Lade es herunter und füge es als Resource Pack auf deinem Client hinzu (oder lass es dir vom
Server zuschicken), um es zu nutzen.

## Fehlerbehebung

| Meldung                                             | Was sie bedeutet                                                        |
| ---------------------------------------------------- | ---------------------------------------------------------------------- |
| *No displays registered*                            | Führe zuerst `/display register` aus (oder spawne ein Display).       |
| *No display selected*                               | Registriere ein Display oder wechsle zu einem, bevor du bearbeitest.  |
| *You do not have build rights here*                 | Dir fehlen die WorldGuard-Regionrechte (oder die Permission) für diese Stelle. |
| *Display is too far away*                           | Geh näher an das Display heran, das du bearbeitest.                   |
| *Invalid material*                                  | Das Item/Material, mit dem du spawnen wolltest, ist ungültig oder steht auf der Sperrliste des Servers. |
| *This command can only be applied to text displays* | Du hast einen `text`/`glow`-Befehl bei einem nicht passenden Display-Typ ausgeführt. |
| *No WorldEdit selection found*                      | Führe `//pos1` und `//pos2` aus (oder `/display selection pos1`/`pos2`, falls du WorldEdits eigenes Recht nicht hast), bevor du eine Composition speicherst oder eine Selection-Gruppe registrierst. |
| *This feature requires WorldEdit*                   | WorldEdit ist auf diesem Server nicht installiert.                    |
| *Invalid composition name*                          | Nur Kleinbuchstaben, Zahlen, `-` und `_` sind erlaubt (max. 32 Zeichen). |
| *No selection group registered*                     | Führe `/display selection register` vor `move`/`unregister` aus.      |
| *Too many displays in the selection*                | Verkleinere deine WorldEdit-Selektion – sie überschreitet das konfigurierte Server-Limit. |
| *The move distance exceeds the configured limit*     | Teile die Bewegung in kleinere Schritte auf, oder bitte einen Admin, `selection_move_radius` zu erhöhen. |
| *Cannot spawn ... as a ... display*                 | Das gespawnte Material unterstützt den erzwungenen `item`/`block`-Typ nicht. |

**Warum kann ich hier nicht bauen?** – Prüfe deine WorldGuard-Regionrechte oder frag einen OP.

**Warum wird mein Befehl abgelehnt, obwohl ich die Berechtigung habe?** – Meist hast du kein
Display ausgewählt, oder der angegebene Wert liegt außerhalb der konfigurierten Grenzen des
Servers.

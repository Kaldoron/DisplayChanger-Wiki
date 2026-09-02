# Changelog

🇬🇧 **English** | 🇩🇪 [Deutsch](Changelog.de.md)

User-facing changes to DisplayChanger, newest first. The plugin doesn't use version numbers for
releases, so entries are grouped by date instead.

## 2026-09-02

**Center a display on the block grid**
New `/display center` command snaps the selected display's position to the corner of the block
cell it currently occupies, and adjusts its translation to match the current scale, so it sits
centered in that cell. Handy for precise positioning after nudging a display around — works on
Block, Item and Text displays. See [Transform: move, scale, rotate](Readme.md#transform-move-scale-rotate).

## 2026-09-01

**Select without a WorldEdit permission**
New `/display selection pos1` and `/display selection pos2` commands set a WorldEdit selection
point to the block you're currently standing on — the same selection `//pos1`/`//pos2` would set,
but gated only by DisplayChanger's own `displaychanger.selection.pos` permission instead of
WorldEdit's. Useful for players who should be able to save compositions or register a selection
group without being granted WorldEdit's own building permissions. See [Compositions (saving
layouts)](Readme.md#compositions-saving-layouts) and [Moving and rotating a group of
displays](Readme.md#moving-and-rotating-a-group-of-displays).

**Fixed**
- `/display selection rotate` could tear multi-piece constructions apart instead of rotating them
  as one rigid group: displays whose position within the build comes from a fine offset (not just
  their base position) ended up piled on top of each other with mismatched facings instead of
  turning together. Rotating a selection now keeps every piece's position and orientation
  correctly locked together. See [Moving and rotating a group of
  displays](Readme.md#moving-and-rotating-a-group-of-displays).
- `/display rotation add` (including its `<x> <y> <z>` form) could drift off the axis you asked
  for once a display already had more than one rotation applied — e.g. adding yaw after a pitch
  would tilt around the display's own already-tilted axis instead of the world's vertical axis.
  Repeated rotations across different axes now behave consistently. See
  [Transform: move, scale, rotate](Readme.md#transform-move-scale-rotate).

## 2026-08-31

**Move and rotate a group of displays together**
New `/display selection` commands let you treat every display inside a WorldEdit selection as one
rigid group:

- `/display selection register` — registers every display inside your current WorldEdit selection
  (`//pos1`/`//pos2`) as a group, capturing the selection's center as a fixed pivot point.
- `/display selection move <dx> <dy> <dz>` — moves the whole group by a relative offset.
- `/display selection rotate <yaw|pitch|roll|all> <degrees>` — rotates the whole group together
  around its pivot.
- `/display selection unregister` — clears the group registration.

`/display undo` now also undoes the most recent group move/rotate, alongside single-display edits
— whichever happened most recently. See [Moving and rotating a group of
displays](Readme.md#moving-and-rotating-a-group-of-displays).

**Force item or block display when spawning**
`/display spawn <feet|front|head> <material> <item|block>` lets you force a material that supports
both — like a block — to spawn as an Item Display (its flat icon) instead of the usual Block
Display, or vice versa. See [Spawning displays](Readme.md#spawning-displays).

**Fixed**
- `/display duplicate` used to replace your entire list of registered displays with just the new
  copy, so switching back to displays you'd registered before duplicating meant registering them
  again. The copy is now added to that list instead, leaving the rest of your registered displays
  intact.

## 2026-08-24 – 2026-08-26

**Background color transparency**
`/display text backgroundcolor` now takes an optional alpha value (`0`–`255`) after a colorname or
RGB color, and as two extra hex digits (`#RRGGBBAA`) for HEX — `0` makes the background fully
invisible. Note that `reset` still restores Minecraft's default translucent background rather than
hiding it; use alpha `0` for true invisibility. See [Text displays](Readme.md#text-displays).

**Compositions rebuilt on WorldEdit**
Compositions (saved display layouts) had been disabled for a while and are now fully rebuilt:

- Area selection now uses the standard WorldEdit `//pos1`/`//pos2` commands instead of a separate
  selection system.
- Saving a composition now checks build rights across the *whole* selected area, not just where
  you're standing.
- Composition names are validated (lowercase letters, numbers, `-`, `_`, max 32 characters).
- New: `/display composition list` and `/display composition delete <name>`.

See [Compositions (saving layouts)](Readme.md#compositions-saving-layouts).

**Graphical menu (GUI)**
All display settings — position, rotation, scale, brightness, glow color, text options, and more
— can now be edited through a click-based inventory menu (`/display gui <menu>`). This replaces
the previous dependency on the external "Genesis Boss Shop" plugin. See [Using the graphical menu
(GUI)](Readme.md#using-the-graphical-menu-gui).

**Undo**
`/display undo` reverts the last change made to the display you're currently editing. See
[Undo](Readme.md#undo).

**Glow color reset**
`/display glowcolor reset` clears a custom glow color back to default without respawning the
display.

**WorldGuard and WorldEdit are now optional**
The plugin detects at startup whether WorldGuard and WorldEdit are installed. Without WorldGuard,
region/build-rights checks are simply skipped; without WorldEdit, only the composition feature is
disabled (with a clear in-game message) — everything else keeps working.

**Fixed**
- `/display brightness reset` wasn't resetting brightness correctly.

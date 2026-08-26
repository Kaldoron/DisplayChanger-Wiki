# Changelog

User-facing changes to DisplayChanger, newest first. The plugin doesn't use version numbers for
releases, so entries are grouped by date instead.

## 2026-08-24 – 2026-08-26

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

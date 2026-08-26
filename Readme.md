# DisplayChanger

DisplayChanger is a Paper server plugin that lets you spawn, edit and save **Display Entities**
(Item Displays, Block Displays and Text Displays) without any external editor — everything is
done in-game, either with chat commands or with a graphical menu.

Use **/display** or the shortcut **/edp** for every command below.

> Need permission to use any of this? Ask your server admin — see [Permissions](#permissions) for
> the full list of permission nodes.

## Table of contents

- [Spawning displays](#spawning-displays)
- [Selecting displays to edit](#selecting-displays-to-edit)
- [Undo](#undo)
- [Duplicating and deleting](#duplicating-and-deleting)
- [Transform: move, scale, rotate](#transform-move-scale-rotate)
- [Appearance: billboard, view range, shadow, brightness](#appearance-billboard-view-range-shadow-brightness)
- [Glow](#glow)
- [Text displays](#text-displays)
- [Graphical menu (GUI)](#graphical-menu-gui)
- [Compositions (saving layouts)](#compositions-saving-layouts)
- [Permissions](#permissions)
- [Region protection (WorldGuard)](#region-protection-worldguard)
- [Resource pack: see-through GUI panel](#resource-pack-see-through-gui-panel)
- [Troubleshooting](#troubleshooting)

## Spawning displays

**`/display spawn <feet|front|head> [material]`**

Spawns a display positioned at your feet, your head, or in front of you.

- If no location is given, the display spawns in front of you.
- If no material is given, the item currently in your main hand is used.
- What you get depends on the item/material:
  - A **block item** (e.g. `STONE`) → a **Block Display**.
  - Any other **item** (e.g. `DIAMOND`) → an **Item Display** showing that item.
  - A **Player Head** with a texture → an **Item Display** showing that exact skin.
  - A **Name Tag** → a **Text Display**. If the name tag has a custom name set (renamed on an
    anvil), that name becomes the display's text; otherwise a placeholder text is used.

Once spawned, the display is automatically selected and ready to edit — there is no separate
"select" step after spawning.

## Selecting displays to edit

DisplayChanger keeps you "logged in" to one display at a time. Whatever you spawn, register, or
switch to becomes the active display for every editing command below.

- **`/display register`** — registers every display within a radius of 5 blocks (configurable)
  around you and selects the first one.
- **`/display switch next`** / **`/display switch previous`** — cycles through your registered
  displays. The currently selected display is briefly highlighted.
- **`/display infoall`** — lists all of your registered displays in chat.
- **`/display info`** — shows detailed information (position, scale, rotation, brightness, glow,
  etc.) about the currently selected display.
- **`/display unregister`** — clears your registered displays and leaves edit mode.

## Undo

**`/display undo`**

Reverts the last change made to the display you're currently editing, one step at a time. Every
successful edit (scale, rotation, move, brightness, view range, shadow, billboard, glow,
glow color, and text changes) is remembered while you're editing that display.

The undo history is cleared when you switch to another display, run `/display unregister`, or
disconnect — undo only ever applies to the display you are currently editing, and only for the
current session.

## Duplicating and deleting

- **`/display duplicate`** — spawns a full copy of the currently selected display (same look,
  transform, glow, shadow, etc.) in front of you and selects the copy.
- **`/display delete`** — permanently removes the currently selected display.

## Transform: move, scale, rotate

**Move**

`/display move <x|y|z> <value>` moves the display along a single axis.
`/display move <x> <y> <z>` moves it along all three axes at once.

Prefix the value with `~` for a relative move (e.g. `~1` moves 1 block further); without `~` the
value is an absolute coordinate.

**Scale**

`/display scale <set|add> <x|y|z|all> <value>` sets or adds to the scale on the given axis (or
all axes at once).
`/display scale reset` resets the scale back to the values the display was spawned with.

**Rotation**

`/display rotation <set|add> <pitch|roll|yaw|all> <value>` sets or adds to the rotation on the
given axis.
`/display rotation reset` resets the rotation back to the values the display was spawned with.

## Appearance: billboard, view range, shadow, brightness

**Billboard (alignment)**

`/display billboard <mode>` controls how the display turns to face you:

| Mode         | Behaviour                                          |
| ------------ | --------------------------------------------------- |
| `fixed`      | Display never turns — stays exactly as rotated.      |
| `vertical`   | Display turns to face you, but only around the vertical (up/down) axis. |
| `horizontal` | Display turns to face you, but only around the horizontal axis. |
| `center`     | Display always fully faces you, on every axis.       |

**View range**

`/display viewrange <value>` sets how far away (as a percentage) the display can still be seen,
combining both the server-side and client-side render distance settings (whichever is smaller
wins).

**Shadow**

`/display shadowradius <value>` sets the size of the display's shadow.
`/display shadowstrength <value>` sets the strength/darkness of the shadow.
A shadow is only visible once **both** values are greater than 0.

**Brightness**

`/display brightness <value>` sets the display's brightness, from `0` (fully dark) to `15` (fully
lit, ignores the surrounding light level).
`/display brightness reset` resets it back to the brightness the display was spawned with.

## Glow

`/display glow <true|false>` turns the outline glow effect on or off.

`/display glowcolor <colorname>` sets the glow color to one of the predefined dye colors:
`black`, `blue`, `brown`, `cyan`, `gray`, `green`, `light_blue`, `light_gray`, `lime`, `magenta`,
`orange`, `pink`, `purple`, `red`, `white`, `yellow`.

`/display glowcolor rgb <r> <g> <b>` sets a custom RGB glow color.
`/display glowcolor hex <#RRGGBB>` sets a custom HEX glow color.
`/display glowcolor reset` resets the glow color to default.

Glow and glow color are **not available for Text Displays** — text displays don't have an outline
to glow.

## Text displays

These commands only work while a **Text Display** is selected (see [Spawning
displays](#spawning-displays) — hold a Name Tag to spawn one).

- **`/display text settext <text>`** — sets the display's text, replacing whatever was there
  before.
- **`/display text addtext <text>`** — appends text to the existing text instead of replacing it.

  Both commands understand [MiniMessage](https://docs.advntr.dev/minimessage/format.html)
  formatting, so you can write things like:

  ```
  /display text settext <gold><bold>Welcome!
  /display text addtext <newline><gray>to the server
  ```

- **`/display text alignment <left|center|right>`** — sets the text alignment.
- **`/display text linewidth <set|add> <value>`** — sets or adds to the maximum line width before
  the text wraps.
- **`/display text seethrough <true|false>`** — makes the text visible through walls (`true`) or
  only when in line of sight (`false`).
- **`/display text opacity <0-100>`** — sets how opaque the text is, as a percentage.
- **`/display text backgroundcolor <colorname|rgb <r> <g> <b>|hex <#RRGGBB>|reset>`** — sets the
  background panel color behind the text, the same color options as
  [glow color](#glow) above.

## Graphical menu (GUI)

Everything above is also available as a click-based inventory menu, useful if you don't want to
remember commands, or want to make small nudges to a display's position/scale/rotation.

Open it with **`/display gui <menu>`**. The main menu to start from is `display_basics`:

```
/display gui display_basics
```

From there, buttons in the top row let you jump between the sub-menus, and each sub-menu has a
"back" button to return to Basics.

> **Note:** the menu buttons are currently labelled in German regardless of your `config.yml`
> language setting — the chat messages the plugin sends you still follow that setting normally.

The see-through chest background in the screenshots below comes from the optional resource pack —
see [Resource pack: see-through GUI panel](#resource-pack-see-through-gui-panel).

### `display_basics`

`/display gui display_basics` — the main menu: spawn a display at your feet/front/head, register
displays nearby, switch between them, get info, delete the selected display, and jump to every
other menu below.

![Display - Basics menu](gui-screenshots/display-basics.png)

### `display_movement`

`/display gui display_movement` — nudge the selected display along all 6 directions, in steps of
1, 0.1, or 0.01 blocks.

![Display - Movement menu](gui-screenshots/display-movement.png)

### `display_rotation`

`/display gui display_rotation` — tilt and rotate the selected display in 1° or 10° steps, or
reset its rotation.

![Display - Rotation menu](gui-screenshots/display-rotation.png)

### `display_scale`

`/display gui display_scale` — grow or shrink the selected display's width, height and depth
independently (in 0.1 or 1 steps), or scale it uniformly, or reset it.

![Display - Scale menu](gui-screenshots/display-scale.png)

### `display_settings`

`/display gui display_settings` — shadow radius and strength, view range, and billboard
alignment.

![Display - View Settings menu](gui-screenshots/display-settings.png)

### `display_brightness`

`/display gui display_brightness` — set the brightness directly (0–15), or reset it.

![Display - Brightness menu](gui-screenshots/display-brightness.png)

### `display_glowcolors`

`/display gui display_glowcolors` — turn glow on/off and pick a glow color. Only has an effect on
Item and Block Displays, not Text Displays.

![Display - Glowcolor menu](gui-screenshots/display-glowcolors.png)

### `display_text`

`/display gui display_text` — Text-Display-only settings: line width, see-through, text
alignment, and links into the two menus below. Only usable while a Text Display is selected.

![Display - Text-Displays menu](gui-screenshots/display-text.png)

### `display_text_backgroundcolors`

`/display gui display_text_backgroundcolors` — pick the Text Display's background color, or
reset/clear it.

![Display - Backgroundcolor menu](gui-screenshots/display-text-backgroundcolors.png)

### `display_text_opacity`

`/display gui display_text_opacity` — set the Text Display's opacity in 10% steps.

![Display - Opacity menu](gui-screenshots/display-text-opacity.png)

## Compositions (saving layouts)

Compositions let you save a group of displays as a named, reusable layout — like a WorldEdit
schematic, but scoped to display entities. Requires the [WorldEdit](https://enginehub.org/worldedit)
plugin.

1. Select the area with WorldEdit (`//pos1`, `//pos2`) around the displays you want to save.
2. Run **`/display composition save <name>`**. Every display inside that selection is captured —
   full position/rotation/scale, glow, shadow, brightness, view range, and (for text displays)
   text/alignment/background settings.
3. Later, stand wherever you want the layout and run **`/display composition load <name>`** — the
   whole layout is pasted relative to your current position and facing, so it looks the same no
   matter where you paste it.

Other composition commands:

- **`/display composition list`** — lists your saved compositions.
- **`/display composition delete <name>`** — deletes one of your saved compositions.

Notes:

- Compositions are private — you can only save, load, list, or delete your own.
- Names may only contain lowercase letters, numbers, `-` and `_` (max 32 characters).
- Saving requires build rights in the selected area (see
  [Region protection](#region-protection-worldguard)) and requires WorldEdit to be installed;
  loading, listing, and deleting a composition do not require WorldEdit.

## Permissions

Ask your server admin for these if a command doesn't work for you:

| Permission                        | Grants access to                                    |
| ---------------------------------- | ---------------------------------------------------- |
| `displaychanger.default`          | The `/display` command and all of its subcommands, including the GUI. |
| `displaychanger.composition.save` | `/display composition save` and `/display composition delete`. |
| `displaychanger.composition.load` | `/display composition load` and `/display composition list`. |
| `displaychanger.reload`           | `/display reload` (admin-only: reloads `config.yml`). |

## Region protection (WorldGuard)

If the server has [WorldGuard](https://worldguard.enginehub.org/) installed, every display action
(spawning, editing, deleting, saving a composition, ...) requires build rights in the region
you're standing in — you need to be the region owner, a region member, or a server operator.
Without WorldGuard, only the permissions above apply.

## Resource pack: see-through GUI panel

The [Graphical menu](#graphical-menu-gui) opens as a large chest-style inventory, which can
partially block your view of the display you're editing. This repository includes an optional
resource pack that makes that chest texture transparent, so you can see the display behind the
menu while you edit it:

[`invisible-chests-datapack/InvisibleChests_1.21.11.zip`](invisible-chests-datapack/InvisibleChests_1.21.11.zip)

Download it and add it as a resource pack on your client (or have your server push it) to use it.

## Troubleshooting

| Message                                   | What it means                                                        |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| *No displays registered*                  | Run `/display register` (or spawn a display) first.                   |
| *No display selected*                     | Register or switch to a display before editing.                       |
| *You do not have build rights here*       | You're missing WorldGuard region rights (or the permission node) for this spot. |
| *Display is too far away*                 | Move closer to the display you're editing.                            |
| *Invalid material*                        | The item/material you tried to spawn with isn't valid, or is on the server's hidden-items list. |
| *This command can only be applied to text displays* | You ran a `text`/`glow` command while a non-matching display type was selected. |
| *No WorldEdit selection found*            | Run `//pos1` and `//pos2` before saving a composition.                |
| *This feature requires WorldEdit*         | WorldEdit isn't installed on this server.                             |
| *Invalid composition name*                | Only lowercase letters, numbers, `-` and `_` are allowed (max 32 characters). |

**Why can't I build here?** — Check your WorldGuard region rights, or ask an OP.

**Why is my command rejected even though I have permission?** — Usually you have no display
selected, or the value you gave is outside the server's configured limits.

# EditDisplays v1.0

You can use either use **/display** or the shortcut **/edp** for each command.

### Spawn displays

With **/display spawn <feet|front|head> <item>** you can spawn a display either positioned at your feet, your head position or in front of you.

If no location is given as an argument, the display will be spawned in front of you.  
If no item is given as an argument, the item from your main hand will be spawned as a display.

After spawning the display can be edited directly using commands or the menu which can be opened with /edpb

### Choose display to edit

With **/display register** all displays within a radius of 5 blocks will be registered and the currently selected display will shortly be highlighted. The highlighted display is then ready for being edited. You can switch through the the registered displays with **/display switch next** and **/display switch previous**.

You get a list of all registered displays with **/display infoall**

After editing the edit mode can be left using **/display unregister**.

### Delete display

With **/display delete** you can delete the currently selected display.

### Get detailed information about selected item

With **/display info** you can get detailed information about the currently selected display.

### Edit display scale

With **/display scale <set|add> <x|y|z|all> <value>** you can set or add the scale of the selected display for the given direction.  
With **/display scale reset** you can reset the scale for all directions to the default values the display was spawned with.

### Move display

With **/display move <x|y|z> <value>** you can move the display in the given direction.   
If value is used with ~ then the movement is relative while if no ~ is used the value is a given coordinate where the display will be moved to.  
With **/display move <valueX> <valueY> <valueZ>** you can move the display in all directions at once. 

### Rotate display

With **/display rotation <set|add> <pitch|roll|yaw|all> <value>** you can set or add the rotation of the selected display for the given direction.  
With **/display rotation reset** you can reset the rotation for all directions to the default values the display was spawned with.

### Display alignment

With **/display billboard \<vertical\>** you can make the display move with you vertically.  
With **/display billboard \<horizontal\>** you can make the display move with you horizontally.  
With **/display billboard \<center\>** you can make the display move with you vertically and horizontally.

### Display viewrange

With **/display viewrange <value>** you can set how many percent of the serverside or clientside (smaller value has higher priority) the display should have. The displays viewrange sets in which range it still can be seen by players.

### Display shadows

With **/display shadowradius <value>** the size of the displays shadow can be set.  
With **/display shadowstrength <value>** the strength of the displays shadow can be set.  
A shadow is only visible if both the values for shadowradius and shadowstrength each are higher than 0.

### Display brightness

With **/display brightness <value>** the displays brightness can be set. Minimum value is 0 and maximum value is 15.
With **/display brightness reset** the displays brightness can be reset to the default brightness the display was spawned with.

### Display glowcolors

With **/display glowcolor <true|false>** you can activate and deactivate glow for the display.  
With **/display glowcolor <yellow|orange|red|brown|lime|green|pink|purple|light_blue|cyan|light_blue|white|light_gray|gray|black>** you can set the displays glowcolor to any of the minecraft dye colors.  
With **/display glowcolor rgb <valueR> <valueG> <valueB>** you can set the displays glowcolor to any rgb color.

This does not work for TextDisplays since they do not have glowcolors.

## TextDisplays

### Text alignment

With **/display text alignment <left|center|right>** you can set the alignment of the text of the TextDisplay

### Linewidth

With **/display linewidth <set|add> <value>** the linewidth of the displays text can be set.

### Seethrough

With **/display seethrough <true|false>** you can decide weather the TextDisplay should be seen through walls or not.

### Opacity

With **/display text opacity <percentvalue>** you can set the opacity of the text of the TextDisplay

### Backgroundcolor

With **/display text backgroundcolor <yellow|orange|red|brown|lime|green|pink|purple|light_blue|cyan|light_blue|white|light_gray|gray|black>** you can set the TextDisplays backgroundcolor to any of the minecraft dye colors.  
With **/display text backgroundcolor rgb <valueR> <valueG> <valueB>** you can set the displays backgroundcolor to any rgb color.
# pokemon-hacking-projects

## Contents:
-   **Mapping**
    -   Tutorial 1
    -   Tutorial 2
    -   Tutorial 3
    -   Tutorial 4
    -   Tutorial 5
    -   Tutorial 6
-   **Scripting**
    -   [Tutorial 7: The Basics of Scripting](#tutorial-7-the-basics-of-scripting-video-link)
    -   [Tutorial 8: Msgbox Exhaustion](#tutorial-8-msgbox-exhaustion-video-link)
    -   [Tutorial 9: Exchanging Possessions](#tutorial-9-exchanging-possessions-video-link)
    -   [Tutorial 10: History Never Repeats Itself](#tutorial-10-history-never-repeats-itself-video-link)
    -   [Tutorial 11: Mobility](#tutorial-11-mobility-video-link)
    -   [Tutorial 12: Instant Script Activation](#tutorial-12-instant-script-activation-video-link)

## Mapping:


## Scripting:
Following [tutorials](https://www.youtube.com/watch?v=UgI35RdZvq4&list=PLfI5DBI4tNyLBYGNhf1Ee8cgdmMtiilps) made by [Anthroyd](https://www.youtube.com/channel/UCQF_14PVCYMRQv3pJVe13OA), below are notes taken in the process of learning.


### Tutorial 7: The Basics of Scripting (*[Video link](https://www.youtube.com/watch?v=wJw4tz0kcAI)*)
#### Objectives:
-   How does XSE work?
-   Simple NPC Script
-   Insert Script into the game


#### Tools
-   [**XSE - eXtreme Script Editor**](https://github.com/Gamer2020/Unofficial_*XSE*/releases) by HackMew: known as "The best scripting tool for Gen III binary hacks."
    -   [An *XSE* discussion post on PokéCommunity](https://www.pokecommunity.com/showthread.php?t=164276)
-   [**HxD**](https://mh-nexus.de/en/downloads.php?product=HxD): A straightforward hex editor.
-   [**AdvancedMap (1.92)**](http://ampage.no-ip.info/index.php?seite=home) by LU-HO: Map editor for Gen III binary hacks.


#### Start Scripting
Starting a new script in *XSE* with the following lines:
```
#dynamic 0x800000
#org @start
lock
faceplayer
msgbox @talk 0x6
release
end

#org @talk
= I'm an NPC.
```

There are many components in the above scrpit:
-   `#dynamic 0x800000`: This line searches in the game ROM for free memory locations starting at memory address with `0x800000`. Whenever we use `0x` following a number, we are telling the compiler that this number is in hexadecimal format instead of decimal format.

    We are using the specific location `0x800000` just because it is a conventional location in general to begin looking for free space. In the [Hexadecimal Data Format section](#hexadecimal-data-format), there is a bit more information about the data format and memory of the game.

-   `#org @start`: This line denotes the beginning of the script.

-   `lock`: Locks the player in place so he/she cannot move while the NPC is speaking.

-   `faceplayer`: Makes the NPC faces the player while the dialogue is triggered.

-   `release`: Releases the player from locking.

-   `msgbox @talk 0x6`: Points to a message box with name  `@talk` with type `0x6`. `0x6` refers to dialogue box that has no special properties.

-   `#org @talk`: Assignment of dialogue text.

-   `end`: Indicates the end of the script.

-   To add comment in the script, simply put single quotation mark `' ` at the end of line and add comments after.

-   Blank space means nothing in XSE.

-   Note that using key `F1` or going to `Help > Command Help` in *XSE* brings us a list of all known commands will be displayed.

![Scripted Dialogue Example](/docs/tute7-dialogue.png)


#### Compiling
-   Save the script somewhere
-   Go to `Tools > Batch Compiler` in *XSE*
-   Load up the ROM
-   Choose the script
-   Hit `Compile`

A new window with compiler output and a list of offsets will pop up, this shows where in the ROM our script data has been stored.

-   In Offsets list, look for the pointer for `@start` offset and click `Copy`
-   In *AdvancedMap* go to `Events`
-   Increment the `Number of person events` by 1 in `Amount of events`
-   Hit `Change Events`, this adds a player event and place it at the top left of map
-   Drag the `Event` to the desired location and change the `Movement Type` if needed
-   Paste the copied `@start` offset value into the `Script Offset` area
-   `Save` the Changes
-   Now run the game


#### Hexadecimal Data Format
Opening the game ROM in a hex editor shows all game data in hex format, this is how the game data is normally saved. (Be careful when dealing with this file since you can accidentally break things and damage the entire ROM unintentionally.)

![Hxd Editor with Emerald ROM Loaded](/docs/hxd-view.png)

Search for `FFFFFFFFFF` which indicates a whole lot of empty space in the ROM and this will potentially be the memory blocks that we would be manipulating over, but not through a hex editor, instead through scripting in *XSE* (at this stage).

It is worth mentioning that *Emerald* is known with a lot less free space while *FireRed* is known to have a lot more empty space.


### Tutorial 8: Msgbox Exhaustion (*[Video link](https://www.youtube.com/watch?v=cL9m4ZRasbc)*)

#### Objectives:
-   Long Strings
-   Coloring Strings
-   Formatting Dialogue Boxes

#### Long Strings
To implement long strings within a text box, refer to the following new line characters:
-   `\n` - Basic new line character, used only once for the first newline.
-   `\l` - Additional new line character, used after the first newline, for scrolling through text within the dialogue box.
-   `\p` - New paragraph character, used when a fresh text box is needed instead of scrolling.

For Gen III Pokémon games, usually 35 character would fit well in one line. By highlighting a chunk of text in XSE, it tells you how many characters have been highlighted in the status bar.

Example:
```
#org @talk
= I'm an NPC. No matter how hard \nI try, I can never escape this \lreality.\p Now that I really think about it, \lit's pretty darn bad.
```


#### Coloring Strings
To color a string, simply put the corresponding color code for the specific game before the part of text needed to be colored.

Example:
```
#org @t3
= [black_fr]It's my favourite instrument!

#org @t2
= [black_fr]How unfortunate!

#org @t1
= [black_fr]Do you play [green_fr]piano[black_fr]?
```

Some predefined color codes in Gen III Pokémon Games:
-   Color Codes: [Pokémon Generation III ROM Hacking - Color Codes.txt](docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Color%20Codes.txt)

Other codes mentioned:
```
[player] - Displays the player's name
[rival] - Displays the rival's name
```

#### Formatting Dialogue Boxes
There are multiple pre-formatted dialogue boxes what defines the behaviour when a dialogue is triggered, it is done by declaring the dialogue type when initiating the message box.

-   `0x2` - Regular NPC Script Type, no `lock`, `faceplayer` or `release` command is required, only start and stop is needed.

-   `0x3` - Sign post, no `lock`, `faceplayer` or `release` command is required, only start and stop is needed, it also changes the message box style to a signpost style.

-   `0x4` - No clicking sound or scrolling effect for the message box (spooky). Remember to use the `closeonkeypress` command after `msgbox` command and before `release`. Commands like `lock`, `faceplayer`, `release` needs to be added.

-   `0x5` - Reserved for Yes/No question, example:

    ```
    #dynamic 0x800000
    #org @start
    lock
    faceplayer
    msgbox @t1 0x5
    compare LASTRESULT 0x1
    if 0x1 goto @yes
    msgbox @t2 0x6
    release
    end

    #org @yes
    msgbox @t3 0x6
    release
    end

    #org @t3
    = [black_fr]It's my favourite instrument!

    #org @t2
    = [black_fr]How unfortunate!

    #org @t1
    = [black_fr]Do you play [green_fr]piano[black_fr]?
    ```

    The `compare` command determines if player has chosen `YES` for the message, and if so, the script will go to the `@yes` pointer and continue from there. Commands like `lock`, `faceplayer`, `release` needs to be added.

-   `0x6` - No specific property has been attached to the message box. Meaning commands like `lock`, `faceplayer`, `release` needs to be added.

-   There are also other types of message boxes, e.g. multiple choice, however will be covered in a future video.

### Tutorial 9: Exchanging Possessions (*[Video link](https://www.youtube.com/watch?v=H47rkg9sGzM)*)

#### Objectives:
-   Give the player a Pokémon
-   Give the player an item
-   Check if the player has a particular item
-   Comparing Numbers in XSE
-   Take an item away from the player

#### Give the player a Pokémon
The command to give the player a Pokémon is the following:
```
givepokemon 0xPOKEMON 0xLEVEL 0xITEM 0x0 0x0 0x0
```
The area with `POKEMON`, `LEVEL` and `ITEM` needs to be filled with hex number with the correlated Pokémon or item codes. Gen III codes are as below:

-   Pokémon Codes: [Pokémon Generation III ROM Hacking - Pokémon Values.txt](docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Pokémon%20Values.txt)
-   Item Codes: [Pokémon Generation III ROM Hacking - Item Values.txt](docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Item%20Values.txt)

Here is an example of using the `givepokemon` command.
```
#dynamic 0x800000
#org @start
lock
faceplayer
msgbox @t1 0x6
givepokemon 0x7 0xA 0x8A 0x0 0x0 0x0
setflag 0x828
release
end

#org @t1
= [black_fr]Here is a Pokemon!
```
Notice that if you wish to give the player a Pokémon before professor Oak gives him/her the fisrt Pokémon, you set a flag. This number varies between versions of Pokémon: `0x828`'for fr, `0x800` ruby, and `0x860` for emerald.

Sounds and congratulating message aren't a part of the `givepokemon` command, playing sounds will be covered in a future tutorial.


#### Give the player an item
The command to give the player an item is the following:
```
giveitem 0xITEM 0xQUANTITY TYPE_OF_MSG
```

Example as following:
```
#dynamic 0x800000
#org @start
lock
faceplayer
msgbox @t1 0x6
giveitem 0x4 0x12 MSG_OBTAIN
release
end

#org @t1
= [black_fr]Here is an Item!
```
Note that there are two types of messages when giving the player an item.
-   `MSG_FIND` - Player found the item
-   `MSG_OBTAIN` - Player obtained through an NPC

This command will play a sound effect for obtaining the item and also a congratulating message depending on which type was chosen.


#### Check if the player has a particular item
The command for checking if the player have the particular item with the specific quantity is the following:
```
checkitem 0xITEM 0xQUANTITY
```

#### Comparing Numbers in XSE
Below is an example to check and compare item from the player's bag:
```
#dynamic 0x800000

#org @start
lock
faceplayer
msgbox @t1 0x6
checkitem 0xD 0x1
compare LASTRESULT 0x1
if 0x4 goto @yes
msg @t2 0x6
release
end

#org @yes
removeitem 0xD 0x1
msgbox @t3 0x6
release
end

#org @t3
= [blue_fr]Man[black_fr]: I took a [green_fr]Potion[black_fr] from you!

#org @t2
= [blue_fr]Man[black_fr]: You don't have what I am\nlooking for!

#org @t1
= [blue_fr]Man[black_fr]: Let me check your bag...
```
Note we used the command `compare` and `goto` again. Using these two commands we can create events with very dynamic situations and results. At the very end we can converge the event again if needed using `goto @complete` and define `@complete` in the script.

Comparison Operators:
-   `0x0` -  Less than
-   `0x1` -  Equal to
-   `0x2` -  Greater than
-   `0x3` -  Less than or equal to
-   `0x4` -  Greater than or equal to
-   `0x5` -  Not equal to

#### Take an item away from the player
The command to take an item away from the player is the following:

```
removeitem 0xITEM 0xQUANTITY
```

From the same example used above, we took the Potion away from the player after we checked that he/she has enough of it.


### Tutorial 10: History Never Repeats Itself (*[Video link](https://www.youtube.com/watch?v=II4_T3MCnLo)*)

#### Objectives:
-   Flags
-   Fade Screen and Hide Sprite
-   Variables
-   *AdvancedMap*'s Script Events
-   `spriteface`
-   `showpokepic`
-   `pause`

#### Flags
Flags are used when we only want an event to occur once or after a certain point in the storyline is reached. Every single instance of a flag use can be attributed to an on or off state.

Each flag is represented by a hexadecimal value.

Some notable flags in the game:
![Notable Flags](/docs/notable-flags.png)

In *AdvancedMap*, when click on an NPC in a particular map, there is a `Person ID`. It also applies to pickable items on the ground.

To set a flag in ROM:
```
setflag 0xFLAGNUM
```

To check if a flag is set:
```
checkflag 0xFLAGNUM
```

An example of flag usage:
```
#dynamic 0x800000

#org @start
lock
faceplayer
checkflag 0x200
if 0x1 goto @alreadyReceived
msgbox @t1 0x6
giveitem 0xD 0x1 MSG_OBTAIN
setflag 0x200
release
end

#org @alreadyReceived
msgbox @t2 0x6
release
end

#org @t2
= You can't have another one!

#org @t1
= Here's a Pokéball!
```

Flag lists:
-   FireRed Safe Flags: `0x200`-`0x2FF`.
-   Safe Flag list for Ruby and Emerald cannot be found on the internet but see below `Unsafe Flags` when decided what to use.
-   Special Flags: [Pokémon Generation III ROM Hacking - Special Flags.txt](/docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Special%20Flags.txt)
-   Unsafe Flags: [Pokémon Generation III ROM Hacking - Unsafe Flags.txt](/docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Unsafe%20Flags.txt)

#### Fade Screen and Hide Sprite
Command for screen fading is the following:
```
fadescreen 0xTYPE
```
Where there are 4 types of fading effects that can be utilised:
-   `0x0` - Fade screen from black to normal
-   `0x1` - Fade screen from normal to black
-   `0x2` - Fade screen from white to normal
-   `0x3` - Fade screen from normal to white

Further more, to hide a sprite:
```
hidesprite 0xNUM
```
Where `0xNUM` is the hexadecimal value of the NPC as shown in *AdvancedMap* in the `Person Event` number box.

Below is an example of an NPC disappearing after dialogue:
```
#dynamic 0x800000

#org @start
lock
faceplayer
msgbox @t1 0x6
fadescreen 0x1
hidesprite 0x4
setflag 0x201
fadescreen 0x0
release
end

#org @t1
= I was never here...
```
The `FLAGNUM` will then need to be inserted into *AdvancedMap* as the Person ID of the NPC in order for the game to know which sprite to set as hidden.

Additionally, there is a command to clear the value of a flag:
```
clearflag 0xFLAGNUM
```
This clears the value of the particular flag as if it has not been set.


#### Variables
Variable can be set to any number between `0x0` to `0xffff`. This means we can set a variable to any of 65,536 values.

Recall we used the `compare` command when checking answer for YES/NO question:
```
compare 0xVARNUM 0x1
if 0x1 goto @yes
```
Where `0xVARNUM` is the associated variable number. In the YES/NO question case, it would be `LASTRESULT`.

The above would effective be checking if `0xVARNUM` stores the value `0x1`.

To set a variable, use the following command:
```
setvar 0xVARNUM 0xVALUE
```

-   FireRed Safe Variables: `0x4011`-`0x40FF`
-   Safe Variables for Ruby and Emerald should mostly be the same as FireRed, but is not fully confirmed.


#### *AdvancedMap*'s Script Events
Script events are used to stop the player in his or her tracks and trigger a script.

An example of this would be Professor Oak stopping us from going in to the tall grass:

<img alt="Script Event Tile Placement" src="/docs/script-event-oak-example.png" width="200px">

<img alt="Script Event Inspector" src="/docs/script-event-oak-example-inspector.png" width="200px">

Every one of the safe variables will have initial value of `0x0`.

Note that a script event will only activate if the variable number stores the variable value shown in *AdvancedMap*.

Example:
```
#dynamic 0x800000

#org @start
lock
spriteface 0x4 0x3
spriteface 0xff 0x4
msgbox @t1 0x6
applymovement 0xFF 0x4
waitmovement 0x0
release
end

#org @m1
#raw 0x8
#raw oxFE

#org @t1
= If you wish to enter, you must\nfirst speak to me.
```
This scripts stops the player from entering an area, forces the player and the NPC to look at each other, triggers a dialogue, then moves the player in the desired direction.

This script will be assigned to the related tiles.

```
#dynamic 0x800000

#org @start
lock
facebplayer
msgbox @allowPass 0x6
setvar 0x4011 0x1
release
end

#org @allowPass
= You may now enter the building.
```
This script will be assigned to the NPC, and after spoken to the NPC, the variable value will no longer be `0` and the above script event will then not be triggered.

#### `spriteface`
To force a sprite looking at a direciton:
```
spriteface 0xTARGET 0xDIRECTION
```
Where the `0XTARGET` can be set to:
-   Any NPC `Person ID` in the map
-   `0xFF` - Set target to player instead of an NPC

And the direction of which the target is set to face `0xDIRECTION`:
-   `0x1` - Down
-   `0x2` - Up
-   `0x3` - Left
-   `0x4` - Right

#### `showpokepic`
To show a Pokémon picture, use the following command:
```
showpokepic 0xPOKEMON 0xX 0xY
```
Where `0xPOKEMON` is the Pokémon code of which Pokémon you wish to show, `0xX` and `0xY` will be the location of which you place the picture. Use `0xA 0x3` to place the picture in the centre.

#### `pause`
`pause` delays the script execution for a specified amount of time:
```
pause 0xTIME
```
The value `0x30` is approximately 1 second in real time.

Other commands will be covered in a future tutorial.


### Tutorial 11: Mobility (*[Video link](https://www.youtube.com/watch?v=CukAvOWy8pU)*)
#### Objectives:
-   Moving a Sprite
-   Moving the Camera

#### Moving a Sprite
To Move a sprite, use the following command:
```
applymovement 0xPERSONEVENT @movement
```
This would linked the defined movement set to the targeted `Person Event`.

```
waitmovement 0xPERSONEVENT
```
This will make the script wait until the movement event specified has finished. Otherwise the script will continue to run along with the movements.

Using `0x0` as the `Person Event` number will refer to whatever the most recent event number that was called.

Example of an NPC moving back and forth and face downwards:
```
#dynamic 0x800000

#org @start
lock
applymovement 0x4 @m1
waitmovement 0x0
msgbox @t1 0x6
release
end

$org @t1
= I've finished moving.

#org @m1
#raw 0x12
#raw 0x13
#raw 0x13
#raw 0x12
#raw 0x0
#raw 0xFE
```

`#raw 0xFE` must be used for the end of movement or the game will freeze.

List of all movement values:
-   Movement Values: [Pokémon Generation III ROM Hacking - Movement Values.txt](docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Movement%20Values.txt)


#### Moving a Camera
To apply special actions such as camera movements or camera shake effects, use the following command:
```
special 0xSPECIAL
```
Where `0xSPECIAL` refers to the particular special effect to apply.
-   `0x113` - Detach Camera from the Player
-   `0x114` - Attach Camera to the Player

Use `0x7F` as `0xPERSONEVENT` for `applymovement` to move the camera instead of an NPC/Player.

Example of detaching the camera from the player and move it around:
```
#dynamic 0x800000

#org @start
lock
special 0x113
applymovement 0x7F @moveCameraUp
waitmovement 0x0
applymovement0xFF @movePlayer
waitmovement 0x0
applymovement 0x7F @moveCameraDown
waitmovement 0x0
special 0x114
release
end

#org @movePlayer
#raw 0x12
#raw 0x13
#raw 0xFE

#org @moveCameraUp
#raw 0x11
#raw 0x11
#raw 0xFE
```

Note that at any given time, the player can see 7 tiles to the left of Player, 7 tiles to the right, 5 and 1/2 upwards and 5 and 1/2 downwards.

List of other special values:
-   Special Values: [Pokémon Generation III ROM Hacking - Special Values.txt](docs/Pokémon%20Generation%20III%20ROM%20Hacking%20-%20Special%20Values.txt)


### Tutorial 12: Instant Script Activation (*[Video link](https://www.youtube.com/watch?v=FttPQvAyWB4&list=PLfI5DBI4tNyLBYGNhf1Ee8cgdmMtiilps&index=13)*)

#### Objectives:
-   Level Script
-   Decompiling a Script
-   Activate Script Immediately
-   Properly Saving a `Map Script`
-   Other `Map Script` Types

#### Level Script
In *AdvancedMap*, select a map and go to the `Header` tab. In `Map Script` contains all scripts used related to the map/level.

<img alt="Map Script Options" src="/docs/map-script-options.png" width="600px">


#### Decompiling a Script
To decompile a script, copy the `Offset` and paste in *XSE*, hit decompile.

For instance, decompiling a Fire Red Script we get:
```
'-----------------
#org 0x166477
setworldmapflag 0x893
checkflag 0x234
if 0x0 call 0x8166484
end

'-----------------
#org 0x166484
movesprite2 0x1 0x1E 0xC
movesprite2 0x5 0x1A 0x1F
movesprite2 0x6 0x1B 0x1F
return
```

The `setworldmapflag 0xMAP` allows player to fly to this map. This can be found in any `Map Script` with type `03`. This does not exist in Ruby / Emerald.

The `call` command allow script to jump to another mamory location.

The `movesprite2` command allows movements of a hidden sprite. There is a [discussion](pokecommunity.com/showthread.php?t=371236) about this command in the PokéCommunity.


#### Activate Script Immediately
`Map Script` with type `02` are scripts that immediately run as soon as the player hit the new territory or arrive at an old territory. Keep track at the variable/flag so that the event does not happen everytime the player arrives (unless that is what you want).

Example of showing a piece of text when first enter a map, and never showing it again:
```
#dynamic 0x800000

#org @start
lock
msgbox @t1 0x4
closeonkeypress
setvar 0x3011 0x1
release
end

#org @t1
= [blue_fr]This [red_fr]is [black_fr]it.
```

This script will run when player first enter the area. Then a type `03` script will clear some flags and setup all other sprites after `02` is concluded.


#### Properly Saving a `Map Script`
-   Remember to change variable of `Map Script` so that the event only occurs once (unless you wanted it otherwise)
-   Hit `Save Map Scripts`
-   Hit `Ctrl-H` while still in the `Header` tab, this will show some more technical information
-   Under `Map Options`, copy `Map Script Offset` value
-   Paste in *XSE*
-   Hit `Level Script` button (the spanner button) next to the Decompile button
-   Hit `Decompile`
-   If there is a command which says:
    ```
    #raw word 0xFFFF
    ```
    Change `0xFFFF` to `0x0000`. Sometimes this is already done.
-   Hit `Compile` to overwrite the previous script (the cog button).


#### Other `Map Script` Types
-   Type `01` - Setmaptile: this is used if the map needs to be dynamically restructured when the player enters. This will be talked about further in a future tutorial.
-   Type `00` - No Scripts: does not do anything.
-   Type `04` - Typically used when player is warpped and needed to be set to a particular behaviour or orientation before the screen fades from black to normal.
-   Type `05` and `07` are hardly ever used.

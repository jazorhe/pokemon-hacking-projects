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
    -   [Tutorial 7: The Basics of Scripting](#tutorial-7-the-basics-of-scripting-vide-link)
    -   [Tutorial 8: Msgbox Exhaustion](#tutorial-8-msgbox-exhaustion-video-link)
    -   [Tutorial 9: Exchanging Possessions](#tutorial-9-exchanging-possessions-video-link)
    -   [Tutorial 10: History Never Repeats Itself](#tutorial-10-history-never-repeats-itself-video-link)

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


### Tutorial 8: Msgbox Exhaustion (*[Vide link](https://www.youtube.com/watch?v=cL9m4ZRasbc)*)

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
Some predefined color codes in Gen III Pokémon Games.
```
----- FIRE RED & LEAF GREEN -----
-   White - [white_fr]
-   Black - [black_fr] (default)
-   Gray - [grey_fr]
-   Red - [red_fr]
-   Orange - [orange_fr]
-   Green - [green_fr]
-   Light Green - [lightgreen_fr]
-   Blue - [blue_fr]
-   Light Blue - [lightblue_fr]
-   Light Blue 2 - [lightblue2_fr]
-   Cyan - [cyan_fr]
-   Light Blue 3 - [lightblue3_fr]
-   Navy Blue - [navyblue_fr]
-   Dark Navy Blue - [darknavyblue_fr]

----- RUBY & SAPPHIRE -----
-   Transparent - [transp_rs]
-   Dark Gray - [darkgrey_rs] (default)
-   Red - [red_rs]
-   Light Green - [lightgreen_rs]
-   Blue - [blue_rs]
-   Yellow - [yellow_rs]
-   Cyan - [cyan_rs]
-   Magenta - [magenta_rs]
-   Gray - [grey_rs]
-   Black - [black_rs]
-   Light Gray - [lightgrey_rs]
-   White - [white_rs]
-   Sky Blue - [skyblue_rs]
-   Dark Sky Blue - [darkskyblue_rs]

----- EMERALD -----
-   White - [white_em]
-   Dark Gray - [darkgrey_em] (default)
-   Gray - [grey_em]
-   Red - [red_em]
-   Orange - [orange_em]
-   Green - [green_em]
-   Light Green - [lightgreen_em]
-   Blue - [blue_em]
-   Light Blue - [lightblue_em]
-   White 4 - [white4_em]
-   Lime Green- [limegreen_em]
-   Aqua - [aqua_em]
-   Navy - [navy_em]
```

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

### Tutorial 9: Exchanging Possessions (*[Vide link](https://www.youtube.com/watch?v=H47rkg9sGzM)*)

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

-   Pokémon Codes: [Pokémon Generation III ROM Hacking - Pokémon Values.txt]()
-   Item Codes: [Pokémon Generation III ROM Hacking - Item Values.txt]()

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
![Script Event Tile Placement](/docs/script-event-oak-example.png =200px)
<!-- ![Script Event Tile Placement](/docs/script-event-oak-example.png =200px) -->

![Script Event Tile Placement](/docs/script-event-oak-example.png){ width=50% }

![Script Event Inspector, 20%](/docs/script-event-oak-example-inspector.png)
![Script Event Tile Placement](/docs/script-event-oak-example.png | width=100)

![Script Event Inspector](/docs/script-event-oak-example-inspector.png =250x250)

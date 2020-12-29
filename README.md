# pokemon-hacking-projects


## Mapping:


## Scripting:
Following [tutorials](https://www.youtube.com/watch?v=UgI35RdZvq4&list=PLfI5DBI4tNyLBYGNhf1Ee8cgdmMtiilps) made by [Anthroyd](https://www.youtube.com/channel/UCQF_14PVCYMRQv3pJVe13OA), below are notes taken in the process of learning.


### Tutorial 7: The Basics of Scripting (*[Vide link](https://www.youtube.com/watch?v=wJw4tz0kcAI)*)

#### Tools
-   [**XSE - eXtreme Script Editor**](https://github.com/Gamer2020/Unofficial_*XSE*/releases) by HackMew: known as "The best scripting tool for Gen III binary hacks."
-   [**HxD**](https://mh-nexus.de/en/downloads.php?product=HxD): A straightforward hex editor.
-   [**AdvancedMap (1.92)**](http://ampage.no-ip.info/index.php?seite=home) by LU-HO: Map editor for Gen III binary hacks.

<br>

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

-   To add comment in the script, simply put `' ` at the end of line and add comments after.

-   Note that using key `F1` or going to `Help > Command Help` in *XSE* brings us a list of all known commands will be displayed.

![Scripted Dialogue Example](/docs/tute7-dialogue.png)


<br>

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

<br>

#### Hexadecimal Data Format
Opening the game ROM in a hex editor shows all game data in hex format, this is how the game data is normally saved. (Be careful when dealing with this file since you can accidentally break things and damage the entire ROM unintentionally.)

![Hxd Editor with Emerald ROM Loaded](/docs/hxd-view.png)

Search for `FFFFFFFFFF` which indicates a whole lot of empty space in the ROM and this will potentially be the memory blocks that we would be manipulating over, but not through a hex editor, instead through scripting in *XSE* (at this stage).

It is worth mentioning that *Emerald* is known with a lot less free space while *FireRed* is known to have a lot more empty space.

# FAQ

---

An answer for every question you have!

---
## How do I add an item to my inventory/sack?
```
set inv/1/items "an item name" 100
        |            |          |
        |            |          |--> optional character level override
        |            |
        |            |--> new item name
        |
        |--> First sack (but considered "second inventory")
```

This will generate the item you named and place it into the first inventory
sack. The command usually generates an item with affixes that's suitable for
your character's level. However, you can also ask the editor to pretend your
character is at another level by adding an optional number to the end, as is
done in this example. This makes it so the editor will generate an item
suitable for a character of level 100.

!!! Note
    If the inventory/sack you specified doesn't have the space to hold the
    item, the editor will complain loudly. You might want to try another sack
    then.

---
## How do I change the stack count on an item?
You'll have to know exactly *where* that item is to change the stack count. You
may want to look through your inventory with the 'show' command like so:
```
show inv/1/items
```

After you've located the item you want to alter, do:
```
set inv/1/items/0/count 99
        |       |       |
        |       |       |--> new item stack count
        |       |
        |       |--> First item
        |
        |--> First sack (but considered "second inventory")
```

---
## How do I respec my character?

You want the "respec" command!

It's as simple as running:
```
respec
```

---
## How to I change my faction alignment?

To see your faction values:
```
show faction-values
```

To set them:
```
set faction-values/<faction index>/faction-value <new faction value here>
```
where the faction index is a number from 0-21. The "show" command should have
shown you which faction corresponds to which index.


---
## How do I change my character name?

```set character-name <new character name>```

After this, just use the "w" command to write out your character file and
complete the rename.

```set character-name Thor``` changes your character name to Thor.

```set character-name "God of Thunder"``` changes your character name to a fancy name
that includes spaces!

!!! Note

    I have not tested if there is a limit to the length of the name. If you find
    that your new character name crashes the game... Well, make it shorter? =)


---
## How do I make a copy of my character?
```write <new character name>``` will make a copy of your character with a
different name.

Be sure to restart steam (if you're using steam and cloud saves) to have the new
character be recognized.

---
## How do I revive my hardcore mode character?
The game tracks how many times your character has died. For hardcore mode characters, once
this number becomes 1, the game will not let you load up the character anymore.

This also means you can revive your character by having your save file report "this character
has never died" with this command:

```set death-count 0```

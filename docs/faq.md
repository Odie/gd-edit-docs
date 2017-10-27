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

```
set death-count 0
```

---
## How do I change choices I've made in a quest?
Unfortunately, this editor is not able to do anything regarding your quest
progress or choices.

Your quest progress lives in a "quest.gdd" file in your character's save
directory on a per-"world" and per-difficulty basis. The editor simply does
not understand the file and its contents.


---
## Can I edit the stats of item in my inventory?
This isn't possible due to the way GD deals with items and their variations.
An item doesn't record any of the stats it has. Instead, it references 1 or more
records from the game's database file. The database records themselves store
the exact stat and/or ranges.

As seen in the game, an item consists of a "base item" with additional effects
that may come from a "prefix" and/or a "suffix". This is exactly how an item is
stored in the character file also.

An item is stored as a reference to a "base item" db record, "prefix" db record,
and "suffix" db record. When the game runs, it presumably grabs all the interesting
fields from these records and figures out the final combined stats for the item.
Variations to the stats are provided by adding a bit of randomness into each of the
stats. The "randomness" is recorded as a "random-seed" field with the item.

Since an item consists of (roughly) a "base item", "prefix", "suffix", and a
"random seed". There is no room to simply tweak an item by, for example, adding
+100 melee damage directly onto an item. It is possible to generate variations of
the same item by tweaking the random seed on an item, though it's not clear *how*
a changed random seed might affect the item.

---
## Can the editor help find the "best version" of an item by trying different random seeds?
The short answer is no.

Due to the sheer number of possible random seeds, exhaustively trying every
possible value is impractical. An item's seed is stored internally as a 32 bit
integer, giving it roughly 4 billion (2^32) item variations.

*If* we can manage to get the editor to perform the exact calculations the game
does to derive the final stats of an item, given a seed, AND the editor is
running on a decently fast computer that can look through each variation in about 1ms,
it will still take roughly

```
(2^32 / 1000ms/sec / 60sec/min / 60min/hour / 24hr/day / 30day/month)
=> 1.65 months
```

to locate the "best variation".

There are various other issues like having to replicate the exact logic and RNG
to derive an item's exact stats in the first place. There is also the fact that
"best stats" is subjective/relative to a character build.

Practically, if you really *really* **really** cared about getting a "good" version of
an item, you might want to try a few different seeds just to make sure you
didn't get one of the worst rolls. But beyond that, your time is probably better
spent enjoying the game. =)

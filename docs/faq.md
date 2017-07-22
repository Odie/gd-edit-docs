# FAQ

---

An answer for every question you have!

---
## How do I add an item to my inventory/sack?
```sh
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
```sh
show inv/1/items
```

After you've located the item you want to alter, do:
```sh
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
```sh
respec
```

---
## How to I change my faction alignment?

To see your faction values:
```sh
show faction-values
```

To set them:
```sh
set faction-values/<faction index>/faction-value <new faction value here>
```
where the faction index is a number from 0-21. The "show" command should have
shown you which faction corresponds to which index.

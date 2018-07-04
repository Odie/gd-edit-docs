# Commands

---

The things that makes the editor go!

---

A commandline program usually gives you a bunch of commands that are somewhat
"low-level". You can string together multiple commands and change/edit things
the way you want.

So! If you somehow can't find a *single* command that does exact what you want,
you probably want to try breaking that task down to multiple commands and
executing them one at a time.


!!! Note

    If anything in the documentation starts sounding like gibberish, don't
    worry! The explanations are there for the sake of completeness. You can use
    the editor just fine without knowing all the nitty-gritty details!

## Basic commands

### command: _show_

Explore your data in your save file like a directory path.

!!! Tip
    If you're able to see the field using "show", you can change the field using "set".

#### Usage
```no-highlight
show <field path>
```

#### Examples

``` show inventory-sacks ``` shows content of "inventory-sacks" field

``` show inv ``` shows the content of the only field that matches "inv" which
happens to be "inventory-sacks"

``` show inv/0 ``` shows content of the first inventory

``` show inv/0/items ``` shows all items in the first inventory

``` show inv/0/items/4 ``` shows 5th item in the first inventory

#### Details
The character file holds quite a bit of binary data that's typically hard to
explore. This command lets you look around the contents of the file almost as if
you're exploring directories.


Each path component in the field path needs to be separated by a '/'.

##### Partial field matching
Each component is also used to partially match against potential fields.

For example, "**in**" will match against "level-**in**-bio", "sh**in**e", "skill-po**in**ts", and more.

As long as the component uniquely identifies a single field and the field is a
collection, the editor will allow you to continue navigating deeper into the data
hierarchy by chaining additional field components.

--

### command: _set_

Set fields in the save file

#### Usage
```no-highlight
set <field path> <new value here>
set <field path to inventory items> <item name> <optional character level>
set <field path to inventory items collection> <item name> <optional character level>
```

#### Examples
``` set iron 1000000 ``` gives your character 1,000,000 iron for all your shopping needs

``` set character-name Morty``` changes your character name to Morty

``` set inv/0/items "sacred hammer of eternal wrath"``` puts a new fancy item into your inventory

#### Details

The 'set' command is actually 2 commands rolled into 1.

##### Setting values
``` set <field path> <new value here> ```

In the case where the field path refers to an actual value such as: integer, floats, strings, and booleans. It will just set the value to the new value you provided. It will do its best to coerce the value you entered to the correct type.

##### Item creation
```
set <field path to inventory item> <item name> <optional character level>
set <field path to inventory items collection> <item name> <optional character level>
```

In the case where the field path refers to an item or an item collection, the editor will attempt to generate an item that matches the <item name> you entered. The editor does this by looking at the available affixes in the game. When possible, it will use "legal" affixes for the base item you asked for.

If you gave the editor an item collection, such as 'inv/0/items', it will try to find some empty spot in the inventory to place the item into.

!!! Error ""

    The editor needs access to the game database for item creation to work. If
    the editor throws an error during item creation, it is very likely that you
    need to configure the gamedir correctly.

--

### command: _find_

Find/locate an item, equipment, skills, devotions, and factions by name.

#### Usage
```no-highlight
find <partial name>
```

#### Examples
``` find tonic ``` Find some item that contains the word tonic

``` find "Maiven's Sphere of Protection" ``` Find a specific skill by name

#### Details

There can be a lot of data in your character file. Although the editor allows you to
to edit any item on a field-by-field basis, it is difficult to find the item you may want
to edit in the first place.

The ```find``` command solves this problem by searching through every piece of data in the
file and printing the location of things that partially matches the entered name. It is
then possible to use other commands to for targetted edits.

For example, if you're a bit low on health potions, you might do:

```no-highlight
> find tonic

Tonic of Mending: inventory-sacks/0/inventory-items/0

> set inventory-sacks/0/inventory-items/0/stack-count 99
```

In this example, we first the item entry for the tonic of mending, then change the stack
count so we have 99 of it in the stack.

--

### command: _find all_

Apply the ```find``` command to all characters without loading them. This returns a list of found items, equipment, skills, devotions, and factions for each character.

#### Usage
```no-highlight
find all <partial name>
```

#### Examples
``` find all tonic ``` Find some item that contains the word tonic

``` find all "Maiven's Sphere of Protection" ``` Find a specific skill by name

#### Details

This is a quick way to apply the ```find``` command to each character without having to load them first.

For example, if you need a specific crafting material for one character, you might do:

```no-highlight
> find all "Ancient Heart"

character:  [save directory]\main\_FirstCharacter\player.gdc
Ancient Heart: inventory-sacks/0/inventory-items/36

character:  [save directory]\main\_SecondCharacter\player.gdc
Ancient Heart: inventory-sacks/2/inventory-items/6
```

This gives you a list of characters that have the item you're looking for, including the storage location.

--

### command: _swap-variant_

Swap between variants of an item's basename, prefix, or suffix

#### Usage
```no-highlight
swap-variant <item path> 
swap-variant <item path> basename
swap-variant <item path> prefix
swap-variant <item path> suffix
```

#### Examples
``` swap-variant weapon-sets/0/items/0  ``` Change the basename/base item of
the currently equipped weapon

#### Details

The editor's 'set' command can be used to generate items. It doesn't, however,
allow for you to pick the exact variant you might want. For example, the
legendary helm "Ravager's Dreadgaze" has 3 different variants, each with very
different boosts attached. When the 'set' command goes about generating that
item, it really does not know which one you want specifically. 

```swap-variant``` solves this problem by letting you customize the item after
item generation. The command can deal with swapping the base item (basename),
prefix, or suffix.

When the command runs, it looks through the game db for items/affixes of with
the same name, then presents the found items in an on-screen menu. The menu
works the same way as the character selection menu when the editor first starts
up. You can make your selection by inputing a number and hitting enter.

The editor will try to present only "interesting" fields in the item/affix. In
this case, "interesting" means fields that are unique amongst all the variants.
This means that the menu will not present the full list of boosts for the
item/affix. But it should present enough information to make picking the
desired variant possible.

--

### command: _write_

Writes out the character that is currently loaded

#### Usage
```no-highlight
write
write <new character name>
```

#### Details

``` write ``` writes out the currently loaded character. A backup is always made
first so you can try to go back to a previous save file if anything should go
wrong. If you changed your "character-name" at some point, the editor will also
move your save files to the matching directory so the game can find it.

!!! Warning

    While the editor does its best to try **not** clobbering your save file,
    it would be advisable to make periodic backups on your own, **just in case**.

``` write <new character name> ``` writes out a new copy of the loaded character
after renaming the character.


--

### command: _load_

Load from a save file

#### Syntax
```no-highlight
load
```

#### Details
This is an odd command. This is the only command that will take you to a
different menu. It will actually unload your current character, if any, then
show you the character selection menu.

You probably expected the command look something like ``` load <character name>
``` That's not really ideal because:

- You may have several savedir configured and have character of the same name
- It's not clear which characters have been found and *can* be loaded without a menu

--

### command: _update_

Update to the latest version of gd-edit

#### Details
The editor will check for a new version if it hasn't done so in the past 24
hours. If a new version is found, a prompt should be shown shortly after the
editor is started.

Running the ```update``` command actually starts the download and relaunches the
editor, if possible.

--

### command: _exit_

Just exits the program. Pretty straightforward!

---

## Convenience commands
These are commands that typically require complicated operations on the character to complete. While the editor gives you a lot of tools to dig around the game db and alter your save file, sometimes, you just need a bit more oomph out of the editor.

### command: _level_

Set the level of the loaded character to a new value

#### Usage
``` level <new level> ```

#### Details
You can level the character both up and down. This command will update the following fields:

- character level (3 separate fields)
- attribute points
- skill points
- experience points

--

### command: _respec_

Respecs the loaded character

#### Usage
```
respec
respec attributes
respec devotions
respec skills
respec all
```

#### Details
```respec```, by itself, is the same ```respec all```.

This is useful when you want to turn your character into another build
completely. Alternatively, you can also respec only attributes, devotions, or
skills, if you just want to tweak your build a bit and avoid having to pick
everything all over again.

!!! Note

    In case it isn't clear, the editor doesn't provide any way of actually
    picking devotions and skills. You'll have to jump back into the game to do
    that.

---

## Configuration


### command: _savedir_

Sets the save game directory to a path

#### Usage
```
savedir
savedir <path to save directory>
```

#### Details
The editor already looks in the following directory by default:

- Steam cloud save directory (fetched from windows registry)
- Documents\My Games\Grim Dawn\save\main

If, for some reason, the save directory doesn't reside in either of these
locations, you can use this command to add yet another path for the editor to
look into.

!!! Note

    Please point to the save\main directory. Because... reasons?

--

### command: _savedir clear_

Removes the previous set game directory

--

### command: _gamedir_

Sets the game installation directory to a path

!!! Warning

    A lot of the editor commands will not work unless the gamedir is configured!

#### Usage
```
gamedir
gamedir <path to game install directory>
```

#### Details
The editor will fetch the steam install directory, then look into
"\steamapps\common\Grim Dawn" sub-directory. If you're not using steam or if
you've installed Grim Dawn to another directory, you'll need to use this command
to help the editor find the installation directory.

The editor needs "database\database.arz" and "resources\Text_EN.arc" to resolve
affixes, their readable names, among other pieces of data required by various
commands. Setting this isn't *strictly* required if you're only using the 'show'
and 'set' command of the editor. However, it's very likely you'll soon want to
create items by name and respec your character, etc etc.

--

### command: _gamedir clear_

Removes the previously set game installation directory

--

### command: _mod_

Displays the mod currently selected

--

### command: _mod pick_

Picks an installed mod to activate

#### Usage
``` mod pick ```

#### Details
This is another command that will not accept any parameters but take you to a
selection menu. The menu will show the game mods you have installed and allows
you to pick one for "activation".

When a mod is "active", the editor will bring the contents of the mod's
database.arz file. From that point on, respecing, leveling, mastery/class
modifications will be take into consideration the data from the mod.

--

### command: _mod clear_

Deselect the currently selected mod

---

## Class manipulation

### command: _class_

Displays the classes/masteries of the loaded character

--

### command: _class add_

Add a class/mastery by name

#### Usage
```class add <mastery name>```

The editor is able to accept any mastery name that is listed in ```class
list```, which should include all masteries added by mods.

The editor will try to remove 1 skill point from the character if possible.
This is done for the sake of keeping skill point use consistent. If the
character has no skill points left to use, the editor will throw up a prompt
to let you do it anyway, if you'd like.

This command is mostly included for the sake of completeness, as you'll most
likely pick your mastery directly from within the game.

--

### command: _class list_

Display classes/masteries known to the editor.

This includes masteries added by mods. Please see the 'mod' commands to
configure which GD mod the editor should consider when manipulating character
masteries.

--

### command: _class remove_

Remove a class/mastery by name

#### Usage
```class remove <mastery name>```

The editor is able to accept any mastery name that is listed in ```class
list```, which should include all masteries added by mods.

Removing a mastery will *not* skills associated with the mastery. Any bonuses you
gain from those skills should continue to be active. However, you'll loose the ability
to put points into those skills from the game's UI.

Removing a mastery will refund any skill points you've put into the mastery.

---

## Database exploration

### command: _db_

Explore the database interactively

#### Usage
``` db <record path> ```

#### Example
``` db records/items/gearfeet ``` shows all known boots/foot-wear in the game

``` db records/items/gearfeet/d010 ``` shows the database record for "Earthshatter Treads"

#### Details
This command is similar to the "show" command. Whereas the "show" command lets
you explore the character file like a directory structure, the "db" command
allows you to explore the game database like a directory structure.

Partial path matching rules also apply.

--

### command: _q_

Query the database

#### Usage
``` q <match condition> <match condition> ... ```

#### Example
``` q key~offensivePhysicalMin``` find all records that has some field key that
partially matches "offensivePhysicalMin"

``` q offensivePhysicalMin>50 ``` find all records/items/affixes that causes
minimum physical damage of 50

``` q offensivePhysicalMin>50 levelRequirement<20``` find all
records/items/affixes that causes minimum physical damage of 50 and can be
equipment by characters lower than level 20

``` q recordname~axe ``` finds all records where the name partially matches the
string "axe"

#### Details
Uh... Are you really reading this? Let me just say... it's very unlikely that
you're going to need this command.

The game db holds **a bit** of data. There are some 30k records the last time I
looked. This is just a *little* too much to manually search through. This
command lets you sift through the database and narrow down the results to
something that can be examined manually.

The game's database records are kept in what's known as "key/value" pairs aka
hashtable/dictionaries. The command accepts a number of match condition that
performs a single test on the key or the value or both. If a record passes all
the listed conditions in the "query", then the record will be collected and
displayed sometime in the future.


#### Match condition
A match condition takes the form of
```<match target> <operator> <match value>```

A ```<match target>``` can be the string "recordname", "key", "value", or the
partial name of a key.

```<operator>``` needs to be one of the following:

| operator | meaning                                         |
| -------- | ----------------------------------------------- |
| ~        | partial string match against <match value>      |
| *=       | partial string match against <match value>      |
| =        | exact match against <match value>               |
| !=       | not equal                                       |
| >        | greater than                                    |
| >=       | greater than or equal to                        |
| <        | less than                                       |
| <=       | less than or equal to                           |

A ```<match value>``` can be a integer, a float, a quoted string, or any string

#### Result ordering
The editor will display the first 10 matched records, ordered by their
recordname. You can alter the ordering by adding a clause in the form of "order
<partial field name>".

The editor will always order in descending order if an order clause is provided.

--

### command: _qshow_

Show the next page in the current query result

#### Details
Often a query will return **many** results. Each record is displayed in The
editor only shows 10 records at a time. Running this command will cause the next
10 command to be displayed.

If the last result has been shown, running this command again will "wrap-around"
and start showing from the first matched record again.

--

### command: _qn_

It's short for "query next". Does the same thing as 'qshow'. This is just an
alias to be able to see the "next" set of results.

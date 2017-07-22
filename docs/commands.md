# Commands

The things that makes the editor go!

---

## The Basics

### show

Explore your data in your save file like a directory path.

#### Syntax
```no-highlight
show <field path>
```

#### Examples

``` show inventory-sacks ``` shows content of "inventory-sacks" field

``` show inv ``` shows the content of the only field that matches "inv" which happens to be "inventory-sacks"

``` show inv/0 ``` shows content of the first inventory

``` show inv/0/items ``` shows all items in the first inventory

``` show inv/0/items/4 ``` shows 5th item in the first inventory

--

### set

Set fields in the save file

#### Syntax
```no-highlight
set <field path> <new value here>
set <field path to inventory items> <item name> <optional character level>
set <field path to inventory items collection> <item name> <optional character level>
```

--

### write

Writes out the character that is currently loaded

#### Syntax
```no-highlight
write
write <new character name>
```

--

### load

Load from a save file

#### Syntax
```no-highlight
load
```

#### Explanation
This is an odd command. It will actually unload your current character, if any.
It then shows you the character selection menu.

!!! Warning
    You probably expected the command look something like. ``` load <character name> ```



--

### update

Update to the latest version of gd-edit

--

### exit

Just exits the program. Pretty straightforward!

---

## Convenience commands
### level

Set the level of the loaded character to a new value


### respec

Respecs the loaded character

---

## Configuration


### savedir

Sets the save game directory to a path

### savedir clear

Removes the previous set game directory

### gamedir

Sets the game installation directory to a path

### gamedir clear

Removes the previously set game installation directory

### mod

Displays the mod currently selected

### mod pick

Picks an installed mod to activate

### mod clear

Unselect the currently selected mod

---

## Class manipulation

### class

Displays the classes/masteries of the loaded character

### class add

Add a class/mastery by name

### class list

Display classes/masteries known ot the editor

### class remove

Remove a class/mastery by name

---

## Database exploration

### db

Explore the database interactively


### q

Query the database


### qshow

Show the next page in the current query result


### qn

It's short for "query next". Does the same thing as 'qshow'. This is just an
alias to be able to see the "next" set of results.

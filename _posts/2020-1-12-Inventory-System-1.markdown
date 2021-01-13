---
layout: post
title:  "Inventory System Revision, Part 1"
date:   2020-1-12 23:17:15 -0500
excerpt_separator: <!-- more -->
banner: /assets/inventory-menu-par1.gif
---
**Written by Jiayan Li**

### What’s Required

&nbsp;&nbsp;&nbsp;&nbsp;As a mash-up game with RPG elements, an inventory system is necessary for players to arrange their own equipment and items received as rewards at the end of each level or bought from  shopkeepers. Beyond that,  save and load functions are also required to keep track of any progress made by  players. Essentially, the entire inventory system would involve these parts:

<!-- more -->

* A backpack that can store items.
* Somes slots for specific equipment like weapons, helmets and armors.
* A trading system.
* Save / load system.
* User Interface elements that can support all the interaction in this inventory system.

### Item Class Design Revision

&nbsp;&nbsp;&nbsp;&nbsp;In our prototype version, the items were based on the Unity Monobehaviour class, which most of Unity users are familiar with. A Monobehaviour class is required to be attached to a certain GameObject to be compiled into the game. Originally, we created a single script to represent all weapon behaviors and properties. However, by using a Monobehaviour based class, the weapon’s data as well as other equipment’s data cannot be saved across various scenes unless it’s attached to an Object that won’t be destroyed when loading another scene. This would force us to either (A) Create a special prefab that simply holds all the weapon property values with multiple weapon property instances, or (B) Double-check every instance of every weapon script to make sure the values match up. Both solutions are tedious, so we decided to abandon using a script that extends the base Monobehaviour script. In that case, we decided to revise all the item classes we have in our current build and use the ScriptableObject class as the base class instead.


&nbsp;&nbsp;&nbsp;&nbsp;According to the document from Unity, this is a useful class when creating assets meant for storing data. In fact, in a built version of our game, each ScriptableObject only takes up around 1KB or storage data or even less, which is more than acceptable for large numbers of items. 

[![ScriptableObject Items](/assets/inventory-system-part1.png)]

&nbsp;&nbsp;&nbsp;&nbsp;In our current build, there is a base “Item” class, and there are several other classes (like the “Weapon” class and “Equipment” class) that inherit from the base class. In the future, this “Item” base class will be the foundation for us to add different item types.

### Next Time
&nbsp;&nbsp;&nbsp;&nbsp;For the next blog, I will describe the item database as well as the inventory system, which involves Unity's serialization used in our save/load system.
 
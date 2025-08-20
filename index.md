# Season 6: New Mechanics & Sandwhisper


![layers](/intro.png)

Season 6 will be available on **October 4**! The test server for founders members will be open starting **September 13**.

This season will include major changes such as:
- A new map system with pathfinding and a layer-based world structure  
- Multi-character boss combat (on the same account)


These systems will be fully leveraged on **Sandwhisper Isle**, the new level 40 to 50 zone accessible by taking a boat south of the forest!

---

## Map System

One of the big upcoming changes is the complete rework of the map system. This will bring major changes to how you navigate in **Artifacts**.

---

### Goodbye Rectangular World, Welcome Pathfinding (A*)!

Maps now have an `access` property, making the world more coherent: with water, mountains, and obstacles. Not all maps will be accessible anymore.

Some maps will require teleportation (via potion), while others will have specific conditions — such as owning an item or reaching a certain level.

**Added to the `MapSchema` model:**
```json
"access": {
  "type": "standard/blocked/conditional/teleportation",
  "conditions": []
}
```

Logs and responses will now include the path your character used, helping you optimize travel.

---

### A World Made of Layers!

The world is now made up of **three layers**:

- `interior`  
- `overworld`  
- `underground`


Characters using the `Move` endpoint will move within a single layer. 

The new Transition endpoint `/my/{character}/action/transition` allows characters to "teleport" between layers, as well as between different maps within the same layer.  
It will be used for doors, portals, boats, stairs, and similar means of travel.

Transitions, like maps, can have conditions.  
New condition types such as `cost` ,`has_item` ,`achievement_completed` will be introduced.  
A full page on the condition system will be added to the documentation.

**Example map object:**
```json
{
  "data": {
    "map_id": 1231,
    "name": "Sandwhisper Isle",
    "skin": "desertisland_10",
    "x": -3,
    "y": 19,
    "layer": "overworld",
    "access": {
      "type": "standard",
      "conditions": []
    },
    "interactions": {
      "content": {
        "type": "monster",
        "code": "sandwarden"
      },
      "transition": {
        "map_id": 1233,
        "x": -3,
        "y": 19,
        "layer": "interior",
        "conditions": [
          {
            "code": "sandwhisper_key",
            "operator": "cost",
            "value": "1"
          }
        ]
      }
    }
  }
}
```

These changes introduce a **map id**, which uniquely identifies each map — since different layers can share the same coordinates.  
The existing endpoints will remain compatible with coordinates, now with layer precision, and new endpoints will also support map IDs.

This is a **big transition**, but I’m really excited to introduce it.  
Le monde sera graduellement mise à jour pour profiter totalement de ses mécanismes.

---

## Multi-Character Combat

One of the most requested features is finally coming: **multi-character combat**, arriving with season 6!

This required several fundamental changes to the combat system.

---

### Changes to the Combat System

Gone are the days of your character always starting first. Introducing a new stat: **initiative** — determining turn order between characters and monsters.


A second new stat is also introduced: **threat** — relevant only for multi-character battles, as it affects which character the monster is likely to attack.

---

### Monster Types

Monsters now fall into three categories:

- `normal`  
- `elite`: Stronger than normal monsters of the same level, protecting rare resources (e.g., Lich, Rosenblood)  
- `boss`: The only monsters that can be fought in groups

---

### How Multi-Character Combat Works

Group fights are as simple as regular ones: all characters must be on the monster’s map. Le endpoint Fight vous permettra maintenant de specify which characters will join the fight. You can engage with **up to 3** characters from your account lors de combats de boss.

The turn order of characters and the monster will be determined by initiative, and the monster's attacks will mainly depend on the threat each character generates.

There will be consumables and runes that allow you to assign more **specialized roles** to your characters, such as **healing or tanking**.


![bosses](/bosses.png)

Certain monsters such as Lich and Rosenblood have been reworked and are now multi-character bosses. Here is the list of bosses available at the launch of Season 6:

- King Slime (Lvl 15)
- Lich (Lvl 30)
- Rosenblood (Lvl 40)
- Duskworm (Lvl 50)
- Sandwhisper Empress (Lvl 50)


---

## Sandwhisper


**Sandwhisper Isle** is the new zone for characters **level 40 to 50**. It features **new resources**, **six new monsters** (including **two group bosses**), and will be the first area to fully utilize the new map mechanics.

![island](https://i.imgur.com/K19j71Y.png)  
![island](https://i.imgur.com/Yj2Roj9.png)  
![layer](https://i.imgur.com/MEWeqUS.png)

The island will cost **1000 gold per trip**, so you might want to find a way to access its bank.  
Apparently, you’ll need to **earn its trust** before they’ll let you in...

Too many pirates have been trying to loot the island...

---

 
See you in **September** for the test server!

Thanks,  
**muigetsu**

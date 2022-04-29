---
title: "How to make a custom boss"
sidebar_position: 2
---

## Default config
The default ecobosses.yml can be found here:

[GitHub](https://github.com/Auxilor/EcoBosses/blob/master/eco-core/core-plugin/src/main/resources/ecobosses.yml)

## Breakdown of ecobosses.yml
ecobosses.yml is where the configs for all bosses are. It may initially seem daunting however it is very easy to configure and make your own bosses.

Simply, ecobosses.yml looks like this:
```yaml
bosses:
  - <boss 1>
  - <boss 2>
```

It's a big array of objects, each element in the list being a boss. You can add and remove bosses by creating and removing elements from the list.

## Typical Boss Config

```yaml
id: steel_golem
# A base mob and modifiers
# View an explanation for this system here: https://plugins.auxilor.io/all-plugins/the-entity-lookup-system
mob: iron_golem attack-damage:90 movement-speed:1.5 follow-range:16 health:1200
# Supported placeholders: %health%, %time% (formats as minutes:seconds, eg 1:56)
displayName: "&8Steel Golem &7| &c%health%♥ &7| &e%time%"
influence: 40 # The distance at which effects will be applied to players
customai: # Custom mob AI using the entity goal system.
  enabled: false # If custom AI should be enabled, this will override the vanilla mob behaviour.
  target-goals: [ ] # How the boss decides who to attack, if the target mode isn't being used.
  ai-goals: [ ] # How the boss should behave.
effects: [ ] # Effects are done from the player's perspective: to treat the player as the victim, use the self_as_victim option in args
conditions: [ ] # Conditions to apply effects to players; useful if you don't want to affect low-level players
lifespan: 120 # The lifespan of the boss before it despawns, in seconds. Set to a massive number to disable.
defence:
  preventMounts: true # If the boss shouldn't be able to get into boats, minecarts, etc
  explosionImmune: true # If the boss should be immune to explosions
  fireImmune: true # If the boss should be immune to fire damage
  drowningImmune: true # If the boss should be immune to drowning damage
  suffocationImmune: true # If the boss should be immune to suffocation

  meleeDamageMultiplier: 0.8 # Incoming melee damage will be multiplied by this value. Set to 0 to render immune against melee
  projectileDamageMultiplier: 0.2 # Same as melee multiplier, but for projectiles

  teleportation: # Teleport every x ticks in order to avoid being caged in obsidian or similar
    enabled: true # If the boss should teleport
    interval: 100 # Ticks between teleportation attempts
    range: 20 # The range that the boss should check for safe teleportation blocks.
rewards:
  xp: # Experience will be randomly generated between these values
    minimum: 30000
    maximum: 60000
  topDamagerCommands:
    # You can specify as many ranks as you want (adding 4, 5, etc)
    # You can use %player% as a placeholder for the player name
    1:
      - chance: 100 # As a percentage
        commands:
          - eco give %player% 10000
    2: [ ]
    3: [ ]
  nearbyPlayerCommands:
    # Commands to be executed for all players near the boss death location
    radius: 10
    # Uses the same syntax as top damager commands (chance and a list of commands, can use %player%)
    commands: [ ]
  # You can specify as many drops as you want, and group several drops together under one chance
  drops:
    - chance: 100
      items:
        - diamond_sword unbreaking:1 name:"Example Sword"
target:
  # How the boss should choose which player to attack, choices are:
  # highest_health, lowest_health, closest, random
  mode: highest_health
  # The distance to scan for players
  range: 40
bossBar:
  # If the boss should have a boss bar
  enabled: true
  color: white # Options: blue, green, pink, purple, red, white, yellow
  style: progress # Options: progress, notched_20, notched_12, notched_10, notched_6
  radius: 120 # The distance from the boss where the boss bar is visible
spawn:
  # A list of conditions required for a player to be able to spawn a boss, useful to set
  # minimum skill levels, etc
  conditions: [ ]
  autospawn:
    # Spawn the boss automatically every x ticks. Picks a random location, but will only
    # ever spawn in a world if there are no other bosses of that type in the world.
    interval: -1 # The interval in ticks, set to -1 to disable
    locations: # Add as many locations as you want
      - world: world
        x: 100
        y: 100
        z: 100
  totem:
    enabled: false # A spawn totem is a set of 3 blocks on top of each other to spawn a boss (like a snow golem)
    top: netherite_block
    middle: iron_block
    bottom: magma_block
    notInWorlds: [ ] # If spawn totems should be disallowed in certain worlds, specify them here
  egg:
    enabled: true # If the boss should have a spawn egg
    item: evoker_spawn_egg unbreaking:1 hide_enchants name:"&8Steel Golem&f Spawn Egg"
    lore:
      - ""
      - "&8&oPlace on the ground to"
      - "&8&osummon a &8Steel Golem"
    craftable: true
    recipe:
      - iron_block
      - netherite_block
      - iron_block
      - air
      - ecoitems:boss_core ? nether_star
      - air
      - iron_block
      - netherite_block
      - iron_block
messages:
  # For each category, you can add as many messages as you want, each with their own radius.
  # Radius is the distance from the boss where the player will be sent the message
  # Set to -1 to broadcast globally to all players online

  # Supported placeholders: %x%, %y%, %z% (coordinates)
  spawn:
    - message:
        - ""
        - "&fA &8&lSteel Golem&r&f has been spawned!"
        - "&fCome fight it at &8%x%&f, &8%y%&f, &8%z%&f!"
        - ""
      radius: -1

  # Supported placeholders: %damage_<x>_player%, %damage_<X>%
  # You can include as many ranks as you want - if there is no player at a certain rank,
  # it will be replaced with N/A (change in lang.yml)
  kill:
    - message:
        - ""
        - "&fThe &8&lSteel Golem&r&f has been killed!"
        - "&fMost Damage:"
        - "&f - &8%damage_1_player%&f (%damage_1% Damage)"
        - "&f - &8%damage_2_player%&f (%damage_2% Damage)"
        - "&f - &8%damage_3_player%&f (%damage_3% Damage)"
        - ""
      radius: -1
  despawn:
    - message:
        - ""
        - "&fYou ran out of time to kill the &8&lSteel Golem&r&f!"
        - ""
      radius: -1
  injure: [ ]

# All sounds will be played together at the same time
# A list of sounds can be found here: https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Sound.html
# Volume functions as the distance at which the sound will be heard
# Pitch is any value between 0.5 and 2
# If you don't want the vanilla mob sounds, add 'silent' as an option to the mob
sounds:
  spawn:
    - sound: entity_iron_golem_death
      pitch: 0.8
      volume: 100
    - sound: entity_iron_golem_hurt
      pitch: 0.5
      volume: 100
    - sound: entity_ender_dragon_growl
      pitch: 0.5
      volume: 100
  kill:
    - sound: entity_ender_dragon_death
      pitch: 1.8
      volume: 100
    - sound: entity_wither_death
      pitch: 1.2
      volume: 100
  despawn:
    - sound: entity_ender_dragon_ambient
      pitch: 0.5
      volume: 50
    - sound: entity_enderman_death
      pitch: 0.5
      volume: 50
  injure:
    - sound: entity_iron_golem_damage
      pitch: 0.7
      volume: 10
```

### Effects, Conditions, and Influence
The functionality of a boss is dictated by the effects it has. Effects are applied to all players within a distance of the boss - specify the distance by setting the **influence** value. Effects will only be applied to players if they meet all the conditions, which is helpful if you don't want low-level players to be affected by boss fights.

See this page for how to configure effects:

[Configuring an Effect](https://plugins.auxilor.io/all-plugins/configuring-an-effect)

Because effects are applied to players, and assuming you want to negatively affect players near a boss, you will want to set `self_as_victim: true` under args: in the effects, which will mark the player as the victim, useful in order to damage nearby players, strike them with lightning, give them potion effects, et cetera.

### Custom AI goals
Check how to configure custom entity AI here:

[Custom Entity AI](https://plugins.auxilor.io/all-plugins/custom-entity-ai)
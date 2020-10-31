![atywFWQ|690x230, 75%](https://forum.cfx.re/uploads/default/original/4X/3/e/8/3e844dba6b4610b4c4a48f26454e24136378ef53.png) 

### Smart Player Blip Syncing

Bad Blips offers support for the "normal" onesync, which is infinity and beyond. It works by syncing blips to the client based on a easy to use group syntax configuration, allowing integration into any framework without being dependent on roles or features.

## Features
* Easy addition to groups for players by source
* Group/Channel based blips with easy customization
* Works with onesync's grid system
* **Hot-switches** between using a client networked player and set coords from the server, allowing you to still have smooth blips when in range
* Offers the ability to set a player name without being dependent on a framework
* Allows you to set `color`, `sprite` (type), `scale`, `alpha`, `show_off_screen`, and `show_local_direction` on a per group basis, allowing a variety of different blip types
* (One way blip support) Allow a specific group to see another groups blips, but not allow the target group to see you or your group with `_can_see` configuration value
* Smart cleanup of blips and cached player data

## Things to think about
* This has not yet been tested with a large amount of players, the loop may need to be optimized in this regard, and servers may face issues with large amounts of group configurations + high amounts of players per group
* In addition to low player count testing, there may be features that have not been fully tested so please report issues as seen fit
* I am not 100% sure the way I am loop is the most efficient in lua, if anyone would like to improve the method please do

## Known Issues
* When a player can see another player that is in two blip groups in which the original player can see, only one of the groups will show
* "Friend" seems to get in the way of showing player names sometimes

## Config Example
<details>

  ```lua
config = {

    -- How often do we want to send updates to the clients show the blips have updated
    client_update_interval = 2000,

    -- How long should we wait before iterating on the next group
    -- This value should be nowhere close to the value above, and should be less then
    wait_between_group_in_thread = 100,

    -- Toggle showing your own blip on the map
    hide_own_blip = false,

    -- This is where you can create custom blip types
    -- Colors - https://runtime.fivem.net/doc/natives/?_0x03D7FB09E75D6B7E
    blip_types = {
        ['police'] = {
            -- _can_see means the members of this group will be able to see
            -- both "doc" and "ems"
            _can_see = { 'doc', 'ems' },
            -- These settings will take default options, unless specified per group
            _color = 3,
            _type = 1,
            _scale = 0.85,
            _alpha = 255,
            _show_off_screen = false,
            _show_local_direction = false,
        },
        ['ems'] = {
            _color = 1,
        },
        ['doc'] = {
            _color = 5,
        },
    },

    -- Default settings for a group when one can not be found in the predefined list
    -- These options will be used when creating a "custom blip channel"
    default_type = {
        _color = 0,
        _type = 1,
        _scale = 0.85,
        _alpha = 255,
        _show_off_screen = false,
        _show_local_direction = false,
    }
}
  ```
</details>

## Usage Guide

### Register source name (What will show on the blip)
This should be done when a "character" is selected.
```lua
TriggerEvent('badBlips:server:registerSourceName', source, 'this is a player name')
```

### Add a player to a group
```lua
TriggerEvent('badBlips:server:registerPlayerBlipGroup', source, 'police')
```

### Remove player from a group
```lua
TriggerEvent('badBlips:server:removePlayerBlipGroup', source, 'police')
```

### Add a player to a group (timed)
Time should be provided in ms
```lua
TriggerEvent('badBlips:server:registerPlayerTempBlipGroup', source, 'police', 10000)
```

## Download
https://github.com/rizzdev/badblips
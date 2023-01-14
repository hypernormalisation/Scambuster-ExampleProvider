# Scambuster-ExampleProvider
An example provider addon that extends Scambuster with data.

## How to use this template to extend Scambuster

First, clone the contents of this repository into a new repository, preferably "Scambuster-[somename]", e.g. Scambuster-Golemagg.

Then there are three files that need configured.

### 1. Table of Contents (.toc) file

The WoW table of contents files have a strict requirement that your addon's root folder must be the same as the addon's name in the toc file.

As an example, for the Golemagg EU discord we have:

`Addons/Cutpurse-GolemaggEU/Cutpurse-GolemaggEU.toc`

```
## Interface: 30400
## Title: Cutpurse-GolemaggEU
## Notes: Cutpurse list of scammers curated by the GolemaggEU discord.
## Author: Swedge
## Version: 1.0.0
## DefaultState: enabled
## Dependencies: Cutpurse
## OptDeps: LibStub

LibStub.lua
settings.lua
list.lua
core.lua
```

The bottom part of this file determines the load order of the file, and should should never be altered.

For more information on toc files see: https://wowpedia.fandom.com/wiki/TOC_format

### settings.lua

This very simple file contains the variables describing your organisation and your list.
e.g. for Golemagg:

`settings.lua`
```lua
local _, t = ...
t.addon_name = "Cutpurse-GolemaggEU"
t.my_realm = "Golemagg"
t.my_provider = "Golemagg EU Discord"
t.my_description = "Cutpurse list of scammers curated by the Golemagg EU discord."
t.my_url = "https://discord.gg/golemagg"
```

### list.lua

This file contains a table of "case data", where each case records an incident that occurred with a scammer.

e.g.

`list.lua`
```lua
local _, t = ...
t.case_table = {
    -- The below are all valid configurations.
    [1] = {
        name = "Arthas",
        guid = "Player-some-guid123",
        category = "raid",
        description = "Got a bit too upset at demons and had a strop.",
        url = "https://discord.com/channels/some_discord/channel2",
        class = "DEATHKNIGHT"
    },

    [2] = {
        name = "Sylvanas",
        category = "harassment",
        faction = "Horde",
        url = "https://some_other_url.com/war_of_the_thorns",
    },

    [3] = {
        name = "Gallywix",
        description = "Did not honor payment for boat travel services.",
        url = "https://www.wowhead.com/guide/story-goblin-starting-area-lore",
    },
```

The required fields for each case are:
- `name`: the name of the scammer.
- `url`: a link to the url of the evidence against them, usually a thread/message/channel on discord.

At least *one of* the following two are additionally required.
- `description`: a short description of the incident, what type of scam was perpetrated, etc.
- `category`: a category, should be one of "dungeon", "raid", "trade", "gdkp", "harassment". If none of these, is ignored.
  
The following fields are optional, and can help Cutpurse better track scammers, and help your users tailor their experience using the addon more closely:
- `guid`: the player's GUID. This is *highly recommended* to include, as paid name changes retain the same GUID. This will also stop a false-positive from pinging if a scammer renames and someone else takes up the now-vacant name.
- `class`: the player's class, in allcaps, no spaces English, e.g. DRUID, DEATHKNIGHT, WARLOCK etc. For more info see: https://wowpedia.fandom.com/wiki/API_UnitClass
- `faction` - the player's faction. "Horde" or "Alliance".
- `aliases` - an array of the previous names the scammer has gone under.

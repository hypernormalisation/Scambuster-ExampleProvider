# Cutpurse-TestList
A test list for Cutpurse, showing how to curate your own blocklist via an extension addon for Cutpurse.

## How to use this template to extend Cutpurse

First, clone the contents of this repository into a new repository, preferably "Cutpurse-[somename]", e.g. Cutpurse-GolemaggEU.
Then there are three files that need tweaks.

### Table of Contents (toc) file.

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
t.my_provider = "GolemaggEU Discord"
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
    [0] = {
        -- These fields are always required. The case will be ignored by
        -- Cutpurse without them.
        name = "Thrall",  -- The last known name of the listed toon.
        url = "https://discord.com/channels/some_discord/channel1",  -- A URL to the evidence against this player.

        -- It is required to have ONE OF category or description, but 
        -- including both is very helpful for users to tailor their alerts.
        description = "Stole all the gold from a gdkp.",  -- A short verbal description of the case and offence.
        category = "gdkp",  -- A category, should be one of "dungeon", "raid", "trade", "gdkp", "harassment"
                            -- If the given category is none of these, it will be ignored.

        -- These fields are optional.
        guid = "Player-some-guidabc",   -- The GUID of the player. This is a more powerful identifier than the toon's name,
                                        -- and persists through most types of reroll.
        aliases = {"Warchief"},  -- a list of the previous aliases this player has gone under.
        class = "WARRIOR",  -- The unit class, in allcaps english, no spaces, sometimes called the classFilename.
                            -- See https://wowpedia.fandom.com/wiki/API_UnitClass for more info.

    },

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
        url = "https://some_other_url.com/war_of_the_thorns",
    },

    [3] = {
        name = "Gallywix",
        category = "trade",
        description = "Did not honor payment for boat travel services.",
        url = "https://www.wowhead.com/guide/story-goblin-starting-area-lore",
    },
```
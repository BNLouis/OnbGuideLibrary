# Characters' entry.lua

Next, navigate to the bunny folder and open entry.lua. In this file, enemy properties will be specified.

```lua
local shared_package_init = include("./character.lua")
function package_init(character)
    local character_info = {
        name = "Bunny",
        hp = 40,
        damage = 15,
        palette = _folderpath .. "V1.png",
        height = 44,
        frames_between_actions = 34,
        fast_hop_frames = 4,
    }
    if character:get_rank() == Rank.SP then
        character_info.hp = 220
        character_info.damage = 150
        character_info.palette = _folderpath .. "SP.png"
        character_info.frames_between_actions = 14
        character_info.fast_hop_frames = 4
    end
    if character:get_rank() == Rank.NM then
        character_info.damage = 250
        character_info.palette = _folderpath .. "NM.png"
        character_info.hp = 500
        character_info.frames_between_actions = 8
        character_info.fast_hop_frames = 2
    end
    shared_package_init(character, character_info)
end
```

This code defines some base properties that will be used later on in the file character.lua
Some of these properties like name and hp may be self explanatory, but other properties may be specific to bunny and we'll need to check out the package init to see how these properties are used.

!!! note
    The rank of the character is being retrieved by using character:get_rank(). This allows us to define different values for other ranks of enemies, such as increased hp/damage/speed, different palettes, etc. If you had a character that does not change name, you could have defined all of the versions here.

If you look at TuffBunny/entry.lua and MegaBunny/entry.lua you'll see very similar code, minus the extra versions info.

All of them import **../Bunny/character.lua**, which will be covered in part 3.

# Character.lua

```lua title="Character.lua"
-- Includes
local battle_helpers = include("battle_helpers.lua")
local zapring = include("ZapRing/entry.lua")

-- Animations and Textures
local CHARACTER_ANIMATION = _folderpath .. "battle.animation"
local CHARACTER_TEXTURE = Engine.load_texture(_folderpath .. "battle.greyscaled.png")
local TELEPORT_TEXTURE = Engine.load_texture(_folderpath .. "teleport.png")
local TELEPORT_ANIM = _folderpath .. "teleport.animation"

--possible states for character
local states = { DEFAULT = 1, SEEK = 2, ATTACK = 3 }
```

As always we'll start the file with any resources that we need. We'll also include two additional lua files, battle_helpers.lua and ZapRing.lua which contain code that will be needed. We will also define the three states bunny can be in. More on that later.

## Package init

Next, we'll use the character_info that we defined in entry.lua and apply it to the entity. Self is a reference to the actual
entity, so this is where the engine actually sets up those properties through the use of set functions such as self:set_name, self:set_health, etc.

In cases such as self.frames_between_actions = character_info.frames_between_actions, this is just assigning a variable to be used later on in the script.

```lua
-- Load character resources
---@param self Entity
function package_init(self, character_info)
    -- Required function, main package information
    local base_animation_path = CHARACTER_ANIMATION
    self:set_texture(CHARACTER_TEXTURE)
    self.animation = self:get_animation()
    self.animation:load(base_animation_path)
    -- Load extra resources
    -- Set up character meta
    -- Common Properties
    self:set_name(character_info.name)
    self:set_health(character_info.hp)
    self:set_height(character_info.height)
    self:set_palette(Engine.load_texture(character_info.palette))
    self.damage = (character_info.damage)

    -- Bunny Specific
    self.fast_hop_frames = (character_info.fast_hop_frames)
    self.frames_between_actions = character_info.frames_between_actions
    self:set_element(Element.Elec)

    --Other Setup
    self:set_explosion_behavior(4, 1, false)
    self.animation:set_state("SPAWN")
    self.frame_counter = 0
    self.started = false
    self.defense = Battle.DefenseVirusBody.new()
    self:add_defense_rule(self.defense)
    self.move_counter = 0
```

## update_func

To better understand the rest of the code, lets skip to the bottom and look at update_func.

```lua
  --utility to set the update state, and reset frame counter
    ---@param state number
    self.set_state = function(state)
        self.state = state
        self.frame_counter = 0
    end
    -- stores the state functions
    local actions = { 
    [1] = self.action_default, 
    [2] = self.action_seek, 
    [3] = self.action_attack }
    self.update_func = function()
        -- count frames
        self.frame_counter = self.frame_counter + 1
        if not self.started then
            self.started = true
            -- set initial state to default.
            self.set_state(states.DEFAULT)
        else
            -- get and call the function for currrent state
            local action_func = actions[self.state]
            action_func(self.frame_counter)
        end
    end
    end
```

Update func is code that runs on each frame that the game runs. (A frame is about 1/60 of a second.)
To better organize the code, I have split the code into three functions that will run depending on the state of Bunny. Due to the behavior of Bunny, three states are needed.

- **Default:** The bunny is hopping around
- **Seek:** The bunny is moving faster and seeking the enemy
- **Attack:** Bunny throwing a zapring to attack.

Based on certain conditions the bunny will automatically transition between these states.

The update functions for each states are stored in the **actions** table, so the correct function for the current state is always called. We also pass in a frame counter to the function.

## todo:

- explain code in states
- explain battle_helpers
- explain zapring

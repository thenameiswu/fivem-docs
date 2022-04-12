# Conventions

## Syntax

- Use tabs for indentation.
- Conditational statements should be wrapper within `()`. `if (someVaraibles) then`

**Casing**
- Use `PascelCasing` when outside of a function's scope.
- Use `camelCasing` when within a function's scope & for indecies.
- Use `UPPERCASE_SNAKE_CASING` for global variables.
- Use *underscore-prefixed* `_camelCasing` for taken names.

**Padding**
- Use padding inside of tables/arrays, *excluding keys/indecies*. `{ key = value }`
- Use padding for concatenation. `'string one' .. someString`

**Typography**
- Use single quotes unless specified otherwise.
- Use full sentences (with periods) when writing comments, it will make it easier to distinguish it from normal code. `-- This is a comment.`
- Use consistant and descriptive variable names across all scripts. `playerId = source`

**Scope**
- Use correct scope when handling varaibles. *(If a variables is within a function, and it is not re-defining a variable outside of the function's scope, it should be a local variable)* `local playerId = source`

## Resource Structure

- Prefix client, server and shared scripts using `cl_`, `sv_` and `sh_`, respectively. Followed by the script name, resource name or `config`.
- Large resources, *( more than 5 scripts )*, should be split into sub-folders, respective to their type (and should not use prefixes). `server/*.lua`
- Resource categories should be wrapped with `[]` and should always be lowercase. `[assets-maps]`
- Resource names should always be lowercased. `bh, bh-core, bh-inventory, ...`

## Comments
- Categorize groups of code, that are within the global namespace, with comments.
```lua
--[[ Global Instance ]]
--[[ Threads ]]
--[[ Events ]]
--[[ NUI Callbacks ]]
```

- Try to annotate localized code using, descriptive and complete sentences.
```lua
local playerId = source

-- Check the player is active.
if (not self.active[playerId]) then return end

-- Set the player as no longer active.
self.active[playerId] = nil
```

- Use a global instance for helper functions/global variables.
- Citizen methods should not be prefixed by `Citizen.`, instead just call it using it's method. `Wait(), CreateThread(), SetTimeout()`
- The citizen `Wait()` method is based on msec(s), so making `Wait(0) and Wait(1)` are the same and there is no performance increase.
- Events & NUI callbacks should be named based on the resource name, duplicity version and a description using `PascelCasing`. ( `Core:Server:UpdatePlayer` instead of `bh-core:updateplayerinfo` )

```lua
--[[ Class Instance ]]
Core = {
    PlayersBySource = {}

    AddPlayerToArray = function(self, playerId, playerInfo)
        if (not playerId) then return end

        self.PlayersBySource[playerId] = playerInfo
    end,
    ArrayContainsPlayer = function(self, playerId)
        
        for (_playerId, playerInfo in self.PlayersBySource) do
            if (_playerId == playerId) then
                return playerInfo
            end
        end

        return false
    end
}

--[[ Thread(s) ]]

CreateThread(function()

    -- Checking if the player is within the array.
    local arrayContainsPlayer = Core:ArrayContainsPlayer(2)
    if (arrayContainsPlayer) then return end

    -- Adding the player to the array, if they're not already in it.
    Core:AddPlayerToArray(2, { playerName = 'test2', playerId = 2 })

    Wait(200)
end)

--[[ Event(s) ]]

AddEventHandler('Core:Client:UpdatePlayer', function()
    -- Checking if the player is within the array.
    local arrayContainsPlayer = Core:ArrayContainsPlayer(2)
    if (arrayContainsPlayer) then return end

    -- Adding the player to the array, if they're not already in it.
    Core:AddPlayerToArray(2, { playerName = 'test2', playerId = 2 })
end)

--[[ NUI Callback(s) ]]

RegisterNUICallback('Core:NUI:PlayerInfoChanged', function(data, callback)
    -- Checking if the player is within the array.
    local arrayContainsPlayer = Core:ArrayContainsPlayer(2)
    if (arrayContainsPlayer) then return end

    -- Adding the player to the array, if they're not already in it.
    Core:AddPlayerToArray(2, data.playerInfo)
end)
```

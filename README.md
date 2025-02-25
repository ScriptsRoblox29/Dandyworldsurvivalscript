local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
 
 
local Window = Rayfield:CreateWindow({
    Name = "Dandy's World Survival",
    Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
    LoadingTitle = "Loading...",
    LoadingSubtitle = "by isssacque1234",
    Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes
 
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface
 
    ConfigurationSaving = {
       Enabled = true,
       FolderName = skibidi, -- Create a custom folder for your hub/game
       FileName = "skibidi script"
    },
 
 
    KeySystem = true, -- Set this to true to use our key system
    KeySettings = {
       Title = "Key",
       Subtitle = "Key System",
       Note = "Dm for key (Tiktok: Mega_cat_studios)", -- Use this to tell the user how to get a key
       FileName = "skibidi key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
       SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
       GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
       Key = {"Key_9120", "Key_4045"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
    }
 }) 
 
 
 local Aimbot = loadstring(game:HttpGet("https://raw.githubusercontent.com/Exunys/Aimbot-V3/main/src/Aimbot.lua"))()
 local chams = loadstring(game:HttpGet("https://raw.githubusercontent.com/Stratxgy/Roblox-Chams-Highlight/refs/heads/main/Highlight.lua"))()
 local targethud = loadstring(game:HttpGet("https://raw.githubusercontent.com/Stratxgy/Lua-TargetHud/refs/heads/main/targethud.lua"))()
 local speed = loadstring(game:HttpGet("https://raw.githubusercontent.com/Stratxgy/Lua-Speed/refs/heads/main/speed.lua"))()
 
 
 
 
 local aimbotTab = Window:CreateTab("Main", "crosshair")
 
 local Section = aimbotTab:CreateSection("Main")
 
 
 
 local Toggle = aimbotTab:CreateToggle({
    Name = "Remove safe zone",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        if Value then
            local m = workspace:FindFirstChild("CurrentMap")
            if m then
                local f = m:FindFirstChild("SpawnsAndOthers")
                if f then
                    f:Destroy()
                end
            end
        end
    end,
})
 
 
 
 
 
 local visualsTab = Window:CreateTab("Visuals", "crosshair")
 
 local Section = visualsTab:CreateSection("This is visual, meaning it doesn't appear to the entire server, just you.")
 
 
 local Toggle = visualsTab:CreateToggle({
    Name = "ESP players",
    CurrentValue = false,
    Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
    Callback = function(Value)
        getgenv().chams.enabled = Value
    end,
 })
 

 
 local Slider = visualsTab:CreateSlider({
    Name = "Ichor",
    Range = {0, 10000000},
    Increment = 1,
    Suffix = "Ichor",
    CurrentValue = 0,
    Flag = "Slider1",
    Callback = function(Value)
        local player = game:GetService("Players").LocalPlayer
        local ichor = player:FindFirstChild("Ichor")
        if ichor then
            ichor.Value = Value
        end
    end,
})



local Toggle = visualsTab:CreateToggle({
    Name = "Anti-Staff (click me to check)",
    CurrentValue = false,
    Flag = "Toggle1", -- A flag is the identifier for the configuration file
    Callback = function(Value)
        if Value then
            local player = game.Players.LocalPlayer
            if player then
                local targetPlayers = {"LoddylDev", "Masongamerobloxalt", "alifDan_azkaAltLol", "KasperWGardenDev"}
                for _, targetName in pairs(targetPlayers) do
                    local targetPlayer = game.Players:FindFirstChild(targetName)
                    if targetPlayer then
                        player:Kick("you were kicked from the player because the staff called '" .. targetName .. "' entered the game.")
                        return
                    end
                end
            end
        end
    end,
})



local Toggle = visualsTab:CreateToggle({
    Name = "Remove Fog",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        if Value then
            game.Lighting.FogEnd = 100000
        else
            game.Lighting.FogEnd = 1000
        end
    end,
})
 
 
 
 local playerTab = Window:CreateTab("Player", "crosshair")
 
 local Section = playerTab:CreateSection("I think you already know")
 
 
 local Toggle = playerTab:CreateToggle({
    Name = "activate Speed",
    CurrentValue = false,
    Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
    Callback = function(Value)
        getgenv().speed.enabled = Value
    end,
 })
 
  local Slider = playerTab:CreateSlider({
    Name = "Speed",
    Range = {0, 1000},
    Increment = 1,
    Suffix = "Speed",
    CurrentValue = 0,
    Flag = "Slider1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
    Callback = function(Value)
        getgenv().speed.speed = Value
    end,
 })


local Toggle = playerTab:CreateToggle({
    Name = "Let's go jump!",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        if player and player.Character then
            local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.JumpPower = Value and 50 or humanoid.JumpPower
            end
        end
    end,
})
 
 
 getgenv().speed = {
    enabled = false,       -- Enable or disable the speed boost
    speed = 16,          -- Desired walk speed
    control = false, -- Enable enhanced control
    friction = 2.0,       -- Custom friction factor for more control
    keybind = Enum.KeyCode.KeypadDivide -- yes.. i put it as divide.. on the keypad
}
 
 
 Rayfield:Notify({
    Title = "Ready!",
    Content = "Script loaded.",
    Duration = 6.5,
    Image = 4483362458,
 })

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
 
 
    KeySystem = false, -- Set this to true to use our key system
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


local Toggle = aimbotTab:CreateToggle({
    Name = "Kill farm (bugged, you can't disable it.)",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        local teleportActive = Value
        if player and player.Character then
            if teleportActive then
                spawn(function()
                    while teleportActive do
                        wait(0.1)
                        for _, obj in pairs(workspace:GetChildren()) do
                            if obj:IsA("Model") and obj ~= player.Character then
                                local toon = obj:FindFirstChild("Toon")
                                if toon and toon:IsA("BoolValue") then
                                    local hrp = obj:FindFirstChild("HumanoidRootPart")
                                    if hrp then
                                        player.Character:SetPrimaryPartCFrame(hrp.CFrame)
                                    end
                                end
                            end
                        end
                    end
                end)
            else
                teleportActive = false
            end
        end
    end,
})


local Button = aimbotTab:CreateButton({
    Name = "make a random machine (not tested)",
    Callback = function()
        local player = game.Players.LocalPlayer
        local currentMap = workspace:FindFirstChild("CurrentMap")
        if currentMap then
            local generators = currentMap:FindFirstChild("Generators")
            if generators then
                local teleportDone = false
                for _, machine in pairs(generators:GetChildren()) do
                    if machine:IsA("Model") then
                        local machinePart = machine:FindFirstChild("Machine")
                        if machinePart then
                            local machineDone = machinePart:FindFirstChild("MachineDone")
                            if machineDone and machineDone:IsA("BoolValue") then
                                if not machineDone.Value then
                                    local promptPart = machinePart:FindFirstChild("Prompt")
                                    if promptPart and promptPart:IsA("Part") then
                                        local proximityPrompt = promptPart:FindFirstChild("ProximityPrompt")
                                        if proximityPrompt and proximityPrompt.Enabled then
                                            local character = player.Character
                                            if character then
                                                local canTeleport = true
                                                for _, part in pairs(character:GetChildren()) do
                                                    if part:IsA("BasePart") and part.Anchored then
                                                        canTeleport = false
                                                        break
                                                    end
                                                end

                                                if canTeleport then
                                                    character:SetPrimaryPartCFrame(promptPart.CFrame)
                                                    proximityPrompt:InputHoldBegan()
                                                    wait(0.2)
                                                    proximityPrompt:InputEnded()
                                                    teleportDone = true
                                                    break
                                                end
                                            end
                                        end
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end
    end,
})


local Button = aimbotTab:CreateButton({
    Name = "Collect Pops",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        if not character or not character.PrimaryPart then return end

        local currentMap = workspace:FindFirstChild("CurrentMap")
        if not currentMap then return end

        local capsules = currentMap:FindFirstChild("Capsules")
        if not capsules then return end

        local pops = {}

        for _, pop in pairs(capsules:GetChildren()) do
            if pop:IsA("Model") and pop.Name == "Pop" then
                local promptPart = pop:FindFirstChild("Prompt")
                if promptPart and promptPart:IsA("Part") then
                    local proximityPrompt = promptPart:FindFirstChild("ProximityPrompt")
                    if proximityPrompt and proximityPrompt.Enabled then
                        table.insert(pops, {part = promptPart, prompt = proximityPrompt})
                    end
                end
            end
        end

        if #pops == 0 then return end

        for _, pop in ipairs(pops) do
            character:SetPrimaryPartCFrame(pop.part.CFrame)
            fireproximityprompt(pop.prompt)
            wait(0.5)
        end
    end,
})


local Button = aimbotTab:CreateButton({
    Name = "Collect Chocolates",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        if not character or not character.PrimaryPart then return end

        local currentMap = workspace:FindFirstChild("CurrentMap")
        if not currentMap then return end

        local capsules = currentMap:FindFirstChild("Capsules")
        if not capsules then return end

        local chocolates = {}

        for _, chocolate in pairs(capsules:GetChildren()) do
            if chocolate:IsA("Model") and chocolate.Name == "Chocolate" then
                local promptPart = chocolate:FindFirstChild("Prompt")
                if promptPart and promptPart:IsA("Part") then
                    local proximityPrompt = promptPart:FindFirstChild("ProximityPrompt")
                    if proximityPrompt and proximityPrompt.Enabled then
                        table.insert(chocolates, {part = promptPart, prompt = proximityPrompt})
                    end
                end
            end
        end

        if #chocolates == 0 then return end

        for _, chocolate in ipairs(chocolates) do
            character:SetPrimaryPartCFrame(chocolate.part.CFrame)
            fireproximityprompt(chocolate.prompt)
            wait(0.5)
        end
    end,
})


local Button = aimbotTab:CreateButton({
    Name = "Collect All Items",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        if not character or not character.PrimaryPart then return end

        local currentMap = workspace:FindFirstChild("CurrentMap")
        if not currentMap then return end

        local capsules = currentMap:FindFirstChild("Capsules")
        if not capsules then return end

        local items = {}

        for _, item in pairs(capsules:GetChildren()) do
            if item:IsA("Model") and (item.Name == "ResearchCapsule" or item.Name == "HealthKit" or item.Name == "Chocolate" or item.Name == "Pop" or item.Name == "Bandage") then
                local promptPart = item:FindFirstChild("Prompt")
                if promptPart and promptPart:IsA("Part") then
                    local proximityPrompt = promptPart:FindFirstChild("ProximityPrompt")
                    if proximityPrompt and proximityPrompt.Enabled then
                        table.insert(items, {part = promptPart, prompt = proximityPrompt})
                    end
                end
            end
        end

        if #items == 0 then return end

        for _, item in ipairs(items) do
            character:SetPrimaryPartCFrame(item.part.CFrame)
            fireproximityprompt(item.prompt)
            wait(0.5)
        end
    end,
})


local Button = aimbotTab:CreateButton({
    Name = "Anti-Void (you will die if you go into the classic void of Roblox)",
    Callback = function()
        local mapsWB = workspace:FindFirstChild("Maps_WB")
        if mapsWB then
            mapsWB:Destroy()
        end
        
        local workspaceStuff = workspace:FindFirstChild("WorkspaceStuff")
        if workspaceStuff then
            workspaceStuff:Destroy()
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
                local targetPlayers = {"LoddylDev", "Masongamerobloxalt", "alifDan_azkaAltLol", "KasperWGardenDev", "xivannetta", "NO41C", "FakeDWSAdmi"}
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
                if Value then
                    humanoid.JumpPower = 50
                else
                    humanoid.JumpPower = 0
                end
            end
        end
    end,
})


local Toggle = playerTab:CreateToggle({
    Name = "platform",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        local head = character:FindFirstChild("Head")

        if character and humanoidRootPart and head then
            if Value then
                local platform = Instance.new("Part")
                platform.Size = Vector3.new(5, 1, 5)
                platform.Anchored = true
                platform.BrickColor = BrickColor.new("White")
                platform.Transparency = 0.5
                platform.Name = player.Name .. "_Platform"
                platform.Parent = workspace

                local platformHeight = head.Position.Y + 3
                platform.Position = Vector3.new(head.Position.X, platformHeight, head.Position.Z)

                humanoidRootPart.CFrame = CFrame.new(humanoidRootPart.Position.X, platformHeight + 2, humanoidRootPart.Position.Z)

                getgenv().FollowPlatform = true
                task.spawn(function()
                    while getgenv().FollowPlatform and platform do
                        platform.Position = Vector3.new(
                            humanoidRootPart.Position.X,
                            platformHeight,
                            humanoidRootPart.Position.Z
                        )
                        task.wait(0.05)
                    end
                end)
            else
                getgenv().FollowPlatform = false
                local platform = workspace:FindFirstChild(player.Name .. "_Platform")
                if platform then
                    platform:Destroy()
                end
            end
        end
    end
})


local Toggle = playerTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        if player and player.Character then
            local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
            
            if humanoidRootPart then
                if Value then
                    humanoidRootPart.CanCollide = false
                else
                    humanoidRootPart.CanCollide = true
                end
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

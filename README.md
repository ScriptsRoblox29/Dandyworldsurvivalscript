local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Fluent " .. (Fluent.Version or "Unknown"),
    TabWidth = 160, 
    Size = UDim2.fromOffset(580, 460),
    Theme = "Dark"
})

local Tabs = {
    Main = Window:AddTab({ Title = "Main" }),
    Teleports = Window:AddTab({ Title = "Teleports" }),
    ESP = Window:AddTab({ Title = "ESP" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

Fluent:Notify({ Title = "Notification", Content = "Thanks for using my script! Made by isssacque1234." })

Tabs.Main:AddButton({ 
    Title = "Remove the safe zone (Twisted and Toons)", 
    Callback = function() 
        local m = workspace:FindFirstChild("CurrentMap")
        if m then
            local f = m:FindFirstChild("SpawnsAndOthers")
            if f then 
                f:Destroy() 
            end
        end 
    end 
})

Tabs.Main:AddButton({ 
    Title = "Remove Fog", 
    Callback = function() 
        local lighting = game:GetService("Lighting")
        lighting.FogEnd = 100000
    end 
})

Tabs.Main:AddButton({ 
    Title = "Anti-Lag", 
    Callback = function() 
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Decal") or obj:IsA("Texture") then
                obj:Destroy()
            elseif obj:IsA("BasePart") then
                obj.Material = Enum.Material.Plastic
            end
        end
    end 
})

Tabs.Teleports:AddButton({ 
    Title = "teleport to someone random", 
    Callback = function() 
        local players = game:GetService("Players"):GetPlayers()
        local localPlayer = game:GetService("Players").LocalPlayer
        if #players > 1 then
            local target
            repeat
                target = players[math.random(1, #players)]
            until target ~= localPlayer
            if target and target.Character and localPlayer.Character then
                localPlayer.Character:SetPrimaryPartCFrame(target.Character:GetPrimaryPartCFrame())
            end
        end
    end 
})

Tabs.ESP:AddButton({ 
    Title = "ESP Toons and Twisteds", 
    Callback = function() 
        local players = game:GetService("Players"):GetPlayers()
        local function createESP(player) 
            if player.Character then
                local hasKillBox = player.Character:FindFirstChild("KillBox") ~= nil
                for _, part in ipairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        local highlight = Instance.new("Highlight", part)
                        highlight.FillTransparency = 0.5
                        highlight.OutlineTransparency = 0
                        highlight.FillColor = hasKillBox and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 255, 0)
                    end
                end
            end
        end
        for _, player in ipairs(players) do
            if player.Character then
                createESP(player)
            end
        end
        game:GetService("Players").PlayerAdded:Connect(function(player)
            player.CharacterAdded:Connect(function()
                createESP(player)
            end)
        end)
    end 
})

Tabs.ESP:AddButton({ 
    Title = "ESP Machines", 
    Callback = function() 
        local function createESPForMachine(model) 
            if model:IsA("Model") and model.Name == "Machine" then
                for _, part in ipairs(model:GetDescendants()) do
                    if part:IsA("BasePart") then
                        local highlight = Instance.new("Highlight", part)
                        highlight.FillTransparency = 0.5
                        highlight.OutlineTransparency = 0
                        highlight.FillColor = Color3.fromRGB(0, 0, 255)
                    end
                end
            end
        end
        for _, model in ipairs(workspace:GetDescendants()) do
            if model:IsA("Model") then
                for _, folder in ipairs(model:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, obj in ipairs(folder:GetChildren()) do
                            createESPForMachine(obj)
                        end
                    end
                end
            end
        end
        workspace.DescendantAdded:Connect(function(object)
            if object:IsA("Model") then
                for _, folder in ipairs(object:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, obj in ipairs(folder:GetChildren()) do
                            createESPForMachine(obj)
                        end
                    end
                end
            end
        end)
    end 
})

local SpeedSlider = Tabs.Main:AddSlider("SpeedSlider", {
    Title = "Speed of player",
    Description = "Change the player's speed.",
    Default = 2,
    Min = 0,
    Max = 5,
    Rounding = 1,
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        if player and player.Character then
            local humanoid = player.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = Value * 16
            end
        end
    end
})

SpeedSlider:OnChanged(function(Value)
    local player = game.Players.LocalPlayer
    if player and player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = Value * 16
        end
    end
end)

SpeedSlider:SetValue(3)

local IchorSlider = Tabs.Main:AddSlider("IchorSlider", { 
    Title = "Ichor (visual)", 
    Description = "Change the Ichor.", 
    Default = 20000, 
    Min = 0, 
    Max = 100000000000, 
    Rounding = 0, 
    Callback = function(Value) 
        local player = game.Players.LocalPlayer 
        if player and player:FindFirstChild("Ichor") and player.Ichor:IsA("IntValue") then 
            player.Ichor.Value = Value 
        end 
    end 
})

IchorSlider:OnChanged(function(Value) 
    local player = game.Players.LocalPlayer 
    if player and player:FindFirstChild("Ichor") and player.Ichor:IsA("NumberValue") then 
        player.Ichor.Value = Value 
    end 
end)

IchorSlider:SetValue(3)
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

Window:SelectTab(1)

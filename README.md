local HttpService = game:GetService("HttpService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local GuiService = game:GetService("GuiService")
local Players = game:GetService("Players")
local Camera = workspace.CurrentCamera

local configFile = "voltex_config.json"

local function SaveConfig(settings)
    pcall(function()
        writefile(configFile, HttpService:JSONEncode(settings))
    end)
end

local function LoadConfig()
    if isfile(configFile) then
        local success, data = pcall(function() return HttpService:JSONDecode(readfile(configFile)) end)
        if success then
            return data
        else
            print("Error reading config file.")
            return {}
        end
    end
    return {}
end

local settings = LoadConfig()

local function UpdateSetting(key, value)
    settings[key] = value
    SaveConfig(settings)
end

local VLib = loadstring(game:HttpGet("https://pastebin.com/raw/Mb49kKTP"))()

MAINTTL = "Free Voltex Hub"

local win = VLib:Window("Universal", Color3.fromRGB(196, 40, 28), {})

local homeTab = win:Tab("Home")
local mainTab = win:Tab("Main")
local supportedGamesTab = win:Tab("Supported Games")
local miscTab = win:Tab("Misc")
local clientTab = win:Tab("Client")
local uiSettingsTab = win:Tab("UI Settings")

homeTab:Label("Made By: Voltex")
homeTab:Button("Join Discord", function()
    setclipboard("https://discord.gg/9NMFBCq7")
    print("Discord link copied to clipboard!")
end)

uiSettingsTab:Button("Destroy Gui", function()
    game.CoreGui["Library"]:Destroy()
end)

local uiVisibilityButton = settings["UI_Visible"] or Enum.KeyCode.Unknown
uiSettingsTab:Dropdown("Choose Gui Toggle Button", {"Unknown", "F", "G", "H", "J", "K", "L", "M", "N", "O", "Quote"}, function(state)
    uiVisibilityButton = Enum.KeyCode[state]
    UpdateSetting("UI_Visible", uiVisibilityButton)
end, uiVisibilityButton.Name)

local aimbotEnabled = false
local povEnabled = false
local povSize = settings["POV_Size"] or 250
local noRecoil = false

mainTab:Toggle("Enable Aimbot", function(state)
    aimbotEnabled = state
    UpdateSetting("Aimbot_Enabled", state)
end, settings["Aimbot_Enabled"] or false)

mainTab:Toggle("Wall Check", function(state)
    UpdateSetting("WallCheck", state)
end, settings["WallCheck"] or false)

mainTab:Toggle("Highlight Target", function(state)
    UpdateSetting("HighlightTarget", state)
end, settings["HighlightTarget"] or false)

mainTab:Toggle("No-Recoil", function(state)
    noRecoil = state
    UpdateSetting("NoRecoil", state)
end, settings["NoRecoil"] or false)

mainTab:Slider("POV Size", 10, 400, povSize, function(value)
    povSize = value
    UpdateSetting("POV_Size", povSize)
end)

mainTab:Toggle("POV", function(state)
    povEnabled = state
    UpdateSetting("POV_Enabled", state)
end, settings["POV_Enabled"] or false)

mainTab:Toggle("Trigger Bot", function(state)
    UpdateSetting("TriggerBot_Enabled", state)
end, settings["TriggerBot_Enabled"] or false)

mainTab:Toggle("ESP Names", function(state)
    UpdateSetting("ESP_Names", state)
end, settings["ESP_Names"] or false)

mainTab:Toggle("ESP Boxes", function(state)
    UpdateSetting("ESP_Boxes", state)
end, settings["ESP_Boxes"] or false)

mainTab:Toggle("ESP Health", function(state)
    UpdateSetting("ESP_Health", state)
end, settings["ESP_Health"] or false)

mainTab:Toggle("ESP Tracers", function(state)
    UpdateSetting("ESP_Tracers", state)
end, settings["ESP_Tracers"] or false)

clientTab:Slider("Jump Height", 50, 300, 50, function(value)
    game.Players.LocalPlayer.Character.Humanoid.JumpHeight = value
end)

clientTab:Slider("Walk Speed", 10, 200, 16, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)

clientTab:Toggle("No-Clip", function(state)
    if state then
        for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    else
        for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
    UpdateSetting("NoClip", state)
end, settings["NoClip"] or false)

clientTab:Toggle("God Mode", function(state)
    local player = game.Players.LocalPlayer
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        if state then
            player.Character.Humanoid.MaxHealth = math.huge
            player.Character.Humanoid.Health = math.huge
        else
            player.Character.Humanoid.MaxHealth = 100
            player.Character.Humanoid.Health = 100
        end
    end
    UpdateSetting("GodMode", state)
end, settings["GodMode"] or false)

miscTab:Toggle("Fly", function(state)
    if state then
        loadstring(game:HttpGet("https://pastebin.com/raw/YWd9DQS7"))()
    end
end)

miscTab:Toggle("Bang", function(state)
    if state then
        loadstring(game:HttpGet("https://pastebin.com/raw/FMW71S5X"))()
    end
end)

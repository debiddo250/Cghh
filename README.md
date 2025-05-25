--// Carregar OrionLib (ou use outra lib se preferir)
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

local ESP = {
    Boxes = true,
    Tracers = true,
    Names = true,
    TeamCheck = true
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")

local function CreateESP(player)
    local Box = Drawing.new("Square")
    Box.Color = Color3.new(1, 0, 0)
    Box.Thickness = 2
    Box.Transparency = 1
    Box.Filled = false

    local Tracer = Drawing.new("Line")
    Tracer.Color = Color3.new(0, 1, 0)
    Tracer.Thickness = 1
    Tracer.Transparency = 1

    local Name = Drawing.new("Text")
    Name.Color = Color3.new(1, 1, 1)
    Name.Size = 14
    Name.Center = true
    Name.Outline = true

    RunService.RenderStepped:Connect(function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player ~= LocalPlayer then
            if ESP.TeamCheck and player.Team == LocalPlayer.Team then
                Box.Visible = false
                Tracer.Visible = false
                Name.Visible = false
                return
            end

            local pos, onScreen = Camera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            if onScreen then
                if ESP.Boxes then
                    Box.Size = Vector2.new(50, 100)
                    Box.Position = Vector2.new(pos.X - 25, pos.Y - 50)
                    Box.Visible = true
                else
                    Box.Visible = false
                end

                if ESP.Tracers then
                    Tracer.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                    Tracer.To = Vector2.new(pos.X, pos.Y)
                    Tracer.Visible = true
                else
                    Tracer.Visible = false
                end

                if ESP.Names then
                    Name.Position = Vector2.new(pos.X, pos.Y - 60)
                    Name.Text = player.Name
                    Name.Visible = true
                else
                    Name.Visible = false
                end
            else
                Box.Visible = false
                Tracer.Visible = false
                Name.Visible = false
            end
        else
            Box.Visible = false
            Tracer.Visible = false
            Name.Visible = false
        end
    end)
end

for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        CreateESP(player)
    end
end

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        CreateESP(player)
    end
end)

--// Interface
OrionLib:MakeWindow({Name = "Blox Fruits ESP", HidePremium = false, SaveConfig = true, ConfigFolder = "BloxFruitsESP"})

local Tab = OrionLib:MakeTab({Name = "ESP Settings", Icon = "rbxassetid://4483345998", PremiumOnly = false})

Tab:AddToggle({
    Name = "Boxes",
    Default = true,
    Callback = function(Value)
        ESP.Boxes = Value
    end    
})

Tab:AddToggle({
    Name = "Tracers",
    Default = true,
    Callback = function(Value)
        ESP.Tracers = Value
    end    
})

Tab:AddToggle({
    Name = "Names",
    Default = true,
    Callback = function(Value)
        ESP.Names = Value
    end    
})

Tab:AddToggle({
    Name = "Team Check",
    Default = true,
    Callback = function(Value)
        ESP.TeamCheck = Value
    end    
})

OrionLib:Init()# Cghh

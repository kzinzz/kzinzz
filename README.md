local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

local UI = Lib:Create{
   Theme = "Dark",
   Size = UDim2.new(0, 555, 0, 400)
}

local Main = UI:Tab{
   Name = "inicio"
}

local Divider = Main:Divider{
   Name = "inicio shit"
}

local QuitDivider = Main:Divider{
   Name = "sair"
}

local squadColor = Color3.new(1, 1, 1)
local enemyColor = Color3.new(1, 0, 0)
local lineColor = Color3.new(1, 1, 1)
local isEnabled = true

local function drawLine(player)
    local playerCharacter = player.Character
    if playerCharacter then
        local playerHead = playerCharacter:WaitForChild("Head")
        local playerScreenPos, onScreen = workspace.CurrentCamera:WorldToScreenPoint(playerHead.Position)
        if onScreen then
            local center = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y)
            local line = Drawing.new("Line")
            line.Visible = true
            line.From = center
            line.To = Vector2.new(playerScreenPos.X, playerScreenPos.Y + 50)
            line.Color = lineColor
        end
    end
end

local function updateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local playerCharacter = player.Character
            if playerCharacter then
                local playerHead = playerCharacter:WaitForChild("Head")
                local playerScreenPos, onScreen = workspace.CurrentCamera:WorldToScreenPoint(playerHead.Position)
                if onScreen then
                    local box = Drawing.new("Square")
                    box.Visible = true
                    box.Size = Vector2.new(50, 50)
                    box.Position = Vector2.new(playerScreenPos.X - 25, playerScreenPos.Y - 25)
                    if player.Team == game.Players.LocalPlayer.Team then
                        box.Color = squadColor
                    else
                        box.Color = enemyColor
                    end
                    drawLine(player)
                    
                
                    local playerNameLabel = Drawing.new("Text")
                    playerNameLabel.Visible = true
                    playerNameLabel.Text = player.Name
                    playerNameLabel.Position = Vector2.new(playerScreenPos.X, playerScreenPos.Y - 40)
                    playerNameLabel.Color = box.Color
                    
                    -- Calcular e exibir a distância
                    local playerDistance = (playerHead.Position - game.Players.LocalPlayer.Character.Head.Position).Magnitude
                    local distanceLabel = Drawing.new("Text")
                    distanceLabel.Visible = true
                    distanceLabel.Text = string.format("%.1f studs", playerDistance)
                    distanceLabel.Position = Vector2.new(playerScreenPos.X, playerScreenPos.Y - 60)
                    distanceLabel.Color = box.Color
                end
            end
        end
    end
end

game:GetService("RunService").RenderStepped:Connect(function()
    if isEnabled then
        updateESP()
    end
end)

game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Z then
        isEnabled = not isEnabled
    end
end)

# ShitEspMatcha

local pcall = pcall
local ipairs = ipairs
local pairs = pairs
local string_len = string.len
local Vector2_new = Vector2.new
local Color3_new = Color3.new

local playersService = game:GetService("Players")
local workspace = game:GetService("Workspace")
local localPlayerName = playersService.LocalPlayer.Name

local espObjects = {}
local avgCharWidth = 6.0

while true do
    pcall(function()
        local camera = workspace.CurrentCamera
        if not camera then return end

        local viewportSize = camera.ViewportSize
        local currentPlayers = {}

        for _, player in ipairs(playersService:GetChildren()) do
            if player.Name ~= localPlayerName then
                currentPlayers[player] = true
                local esp = espObjects[player]
                
                if not esp then
                    esp = {
                        text = Drawing.new("Text"),
                        textOutline = Drawing.new("Text"),
                        tracer = Drawing.new("Line")
                    }
                    esp.textOutline.Color = Color3_new(0, 0, 0)
                    esp.text.Color = Color3_new(1, 1, 1)
                    esp.tracer.Color = Color3_new(1, 0, 0)
                    esp.tracer.Thickness = 1
                    
                    esp.text.Visible = true
                    esp.textOutline.Visible = true
                    esp.tracer.Visible = true
                    
                    espObjects[player] = esp
                end

                local character = player.Character
                if character and character.Head and character.HumanoidRootPart then
                    local headPos, headOnScreen = WorldToScreen(character.Head.Position)
                    local rootPos, rootOnScreen = WorldToScreen(character.HumanoidRootPart.Position)
                    
                    if headOnScreen or rootOnScreen then
                        local textWidth = string_len(player.Name) * avgCharWidth
                        local textX = headPos.X - (textWidth / 2)
                        local textY = headPos.Y - 25
                        
                        esp.text.Text = player.Name
                        esp.textOutline.Text = player.Name
                        esp.text.Position = Vector2_new(textX, textY)
                        esp.textOutline.Position = Vector2_new(textX + 1, textY + 1)
                        
                        local torsoHeight = rootPos.Y - headPos.Y
                        local feetY = rootPos.Y + (torsoHeight * 1.1)
                        esp.tracer.From = Vector2_new(viewportSize.X / 2, viewportSize.Y)
                        esp.tracer.To = Vector2_new(rootPos.X, feetY)
                    end
                end
            end
        end

        for player, esp in pairs(espObjects) do
            if not currentPlayers[player] then
                esp.text:Remove()
                esp.textOutline:Remove()
                esp.tracer:Remove()
                espObjects[player] = nil
            end
        end
    end)
    end



______________________________________________


<img width="274" height="177" alt="image" src="https://github.com/user-attachments/assets/1e6be47a-831f-4402-bbd6-090e065f697a" />


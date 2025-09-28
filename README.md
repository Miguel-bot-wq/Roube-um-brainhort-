-- Painel com botão de menu
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer.PlayerGui)
ScreenGui.Name = "BrainhortMenu"

local OpenMenuBtn = Instance.new("TextButton", ScreenGui)
OpenMenuBtn.Size = UDim2.new(0, 60, 0, 40)
OpenMenuBtn.Position = UDim2.new(0, 10, 0, 10)
OpenMenuBtn.Text = "RB"
OpenMenuBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
OpenMenuBtn.TextColor3 = Color3.new(1,1,1)
OpenMenuBtn.Font = Enum.Font.SourceSansBold
OpenMenuBtn.TextSize = 20

-- Menu principal
local MenuFrame = Instance.new("Frame", ScreenGui)
MenuFrame.Size = UDim2.new(0, 200, 0, 120)
MenuFrame.Position = UDim2.new(0, 10, 0, 60)
MenuFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MenuFrame.Visible = false

-- Botão ESP
local EspBtn = Instance.new("TextButton", MenuFrame)
EspBtn.Size = UDim2.new(0, 180, 0, 40)
EspBtn.Position = UDim2.new(0, 10, 0, 10)
EspBtn.Text = "Ativar ESP"
EspBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
EspBtn.TextColor3 = Color3.new(1,1,1)
EspBtn.Font = Enum.Font.SourceSansBold
EspBtn.TextSize = 18

-- Abre/fecha menu
OpenMenuBtn.MouseButton1Click:Connect(function()
    MenuFrame.Visible = not MenuFrame.Visible
end)

-- Função ESP
local espAtivo = false
local espObjects = {}

local function ativarESP()
    espAtivo = true
    -- Limpa ESP anterior
    for _,v in pairs(espObjects) do
        v:Destroy()
    end
    espObjects = {}

    for _,player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local Billboard = Instance.new("BillboardGui")
            Billboard.Parent = player.Character.Head
            Billboard.Adornee = player.Character.Head
            Billboard.Size = UDim2.new(0,100, 0,30)
            Billboard.AlwaysOnTop = true

            local TextLabel = Instance.new("TextLabel", Billboard)
            TextLabel.Size = UDim2.new(1,0,1,0)
            TextLabel.BackgroundTransparency = 1
            TextLabel.Text = player.Name
            TextLabel.TextColor3 = Color3.new(1,0,0)
            TextLabel.Font = Enum.Font.SourceSansBold
            TextLabel.TextSize = 16

            table.insert(espObjects, Billboard)
        end
    end
end

EspBtn.MouseButton1Click:Connect(function()
    if not espAtivo then
        ativarESP()
        EspBtn.Text = "ESP Ativado"
    else
        espAtivo = false
        for _,v in pairs(espObjects) do
            v:Destroy()
        end
        espObjects = {}
        EspBtn.Text = "Ativar ESP"
    end
end)

-- Atualiza ESP quando players entram/saem
game.Players.PlayerAdded:Connect(function()
    if espAtivo then ativarESP() end
end)
game.Players.PlayerRemoving:Connect(function()
    if espAtivo then ativarESP() end
end)

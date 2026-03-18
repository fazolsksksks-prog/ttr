-- Services
local Workspace = game:GetService("Workspace")
local Debris = game:GetService("Debris")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Variables
local player = Players.LocalPlayer
local strength = 1000
local fling = false
local espEnabled = false
local espLabels = {}
local Camera = Workspace.CurrentCamera

-- Limpeza: Evita que a interface duplique se você injetar o script mais de uma vez
local guiName = "FlingGui_Corner"
if player:WaitForChild("PlayerGui"):FindFirstChild(guiName) then
    player.PlayerGui[guiName]:Destroy()
end

-- Paleta de Cores (Estilo Dark/Roxo Metálico)
local bgPreto = Color3.fromRGB(12, 12, 12)
local roxoMetalico = Color3.fromRGB(90, 25, 140)
local cinzaEscuro = Color3.fromRGB(35, 35, 35)

-- UI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = guiName
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player.PlayerGui

-- Main Frame (Canto Inferior Direito)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 180, 0, 175)
MainFrame.Position = UDim2.new(1, -195, 1, -190) 
MainFrame.BackgroundColor3 = bgPreto
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

-- Borda Roxo Metálico
local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 2
UIStroke.Color = roxoMetalico
UIStroke.Parent = MainFrame

-- Title
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 25)
Title.Text = "FLING HUB [F]"
Title.TextColor3 = roxoMetalico
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 14
Title.Parent = MainFrame

-- Toggle Button (Fling)
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.85, 0, 0, 30)
ToggleButton.Position = UDim2.new(0.075, 0, 0, 35)
ToggleButton.Text = "Fling: OFF"
ToggleButton.BackgroundColor3 = cinzaEscuro
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.GothamSemibold
ToggleButton.TextSize = 13
ToggleButton.AutoButtonColor = false -- Deixa a animação de clique por nossa conta
ToggleButton.Parent = MainFrame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 6)
btnCorner.Parent = ToggleButton

-- Borda do Botão
local BtnStroke = Instance.new("UIStroke")
BtnStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
BtnStroke.Color = cinzaEscuro
BtnStroke.Thickness = 1
BtnStroke.Parent = ToggleButton

-- ESP Button
local EspButton = Instance.new("TextButton")
EspButton.Size = UDim2.new(0.85, 0, 0, 30)
EspButton.Position = UDim2.new(0.075, 0, 0, 75)
EspButton.Text = "ESP: OFF"
EspButton.BackgroundColor3 = cinzaEscuro
EspButton.TextColor3 = Color3.new(1, 1, 1)
EspButton.Font = Enum.Font.GothamSemibold
EspButton.TextSize = 13
EspButton.AutoButtonColor = false
EspButton.Parent = MainFrame

local espBtnCorner = Instance.new("UICorner")
espBtnCorner.CornerRadius = UDim.new(0, 6)
espBtnCorner.Parent = EspButton

local EspBtnStroke = Instance.new("UIStroke")
EspBtnStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
EspBtnStroke.Color = cinzaEscuro
EspBtnStroke.Thickness = 1
EspBtnStroke.Parent = EspButton

-- Strength Input
local StrengthInput = Instance.new("TextBox")
StrengthInput.Size = UDim2.new(0.85, 0, 0, 30)
StrengthInput.Position = UDim2.new(0.075, 0, 0, 115)
StrengthInput.PlaceholderText = "Força (10-100000)"
StrengthInput.Text = tostring(strength)
StrengthInput.BackgroundColor3 = cinzaEscuro
StrengthInput.TextColor3 = Color3.new(1, 1, 1)
StrengthInput.Font = Enum.Font.Gotham
StrengthInput.TextSize = 13
StrengthInput.Parent = MainFrame

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 6)
inputCorner.Parent = StrengthInput

-- Instrução pequena
local Hint = Instance.new("TextLabel")
Hint.Size = UDim2.new(1, 0, 0, 15)
Hint.Position = UDim2.new(0, 0, 0, 152)
Hint.Text = "Aperte F para alternar Fling"
Hint.TextColor3 = Color3.fromRGB(120, 120, 120)
Hint.BackgroundTransparency = 1
Hint.Font = Enum.Font.Gotham
Hint.TextSize = 10
Hint.Parent = MainFrame

--- Logics ---

local function toggleFling()
    fling = not fling
    if fling then
        ToggleButton.Text = "Fling: ON"
        ToggleButton.BackgroundColor3 = roxoMetalico
        BtnStroke.Color = roxoMetalico
    else
        ToggleButton.Text = "Fling: OFF"
        ToggleButton.BackgroundColor3 = cinzaEscuro
        BtnStroke.Color = cinzaEscuro
    end
end

local function toggleESP()
    espEnabled = not espEnabled
    if espEnabled then
        EspButton.Text = "ESP: ON"
        EspButton.BackgroundColor3 = roxoMetalico
        EspBtnStroke.Color = roxoMetalico
    else
        EspButton.Text = "ESP: OFF"
        EspButton.BackgroundColor3 = cinzaEscuro
        EspBtnStroke.Color = cinzaEscuro
        -- Oculta todas as labels do ecrã quando o ESP é desativado
        for _, label in pairs(espLabels) do
            label.Visible = false
        end
    end
end

-- Efeito visual ao clicar no Fling
ToggleButton.MouseButton1Down:Connect(function()
    ToggleButton.Size = UDim2.new(0.83, 0, 0, 28)
end)
ToggleButton.MouseButton1Up:Connect(function()
    ToggleButton.Size = UDim2.new(0.85, 0, 0, 30)
    toggleFling()
end)

-- Efeito visual ao clicar no ESP
EspButton.MouseButton1Down:Connect(function()
    EspButton.Size = UDim2.new(0.83, 0, 0, 28)
end)
EspButton.MouseButton1Up:Connect(function()
    EspButton.Size = UDim2.new(0.85, 0, 0, 30)
    toggleESP()
end)

StrengthInput.FocusLost:Connect(function()
    local val = tonumber(StrengthInput.Text)
    if val then
        strength = math.clamp(val, 10, 100000000000000000000)
    end
    StrengthInput.Text = tostring(strength)
end)

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.F then
        toggleFling()
    end
end)

-- Original Logic Otimizada
Workspace.ChildAdded:Connect(function(model)
    if model.Name == "GrabParts" then
        local partInfo = model:WaitForChild("GrabPart", 1)
        if not partInfo then return end
        
        local weld = partInfo:WaitForChild("WeldConstraint", 1)
        if not weld or not weld.Part1 then return end
        
        local part = weld.Part1
        local velocity = Instance.new("BodyVelocity")
        velocity.Parent = part
        velocity.MaxForce = Vector3.zero
        
        local conn
        conn = model:GetPropertyChangedSignal("Parent"):Connect(function()
            if not model.Parent then
                if fling then
                    velocity.MaxForce = Vector3.new(1e9, 1e9, 1e9)
                    velocity.Velocity = Workspace.CurrentCamera.CFrame.LookVector * strength
                else
                    velocity.MaxForce = Vector3.zero
                end
                Debris:AddItem(velocity, 1)
                if conn then conn:Disconnect() end -- Limpa o evento da memória
            end
        end)
    end
end)

-- Lógica do ESP (RenderStepped)
local function getESPLabel(plr)
    if not espLabels[plr] then
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(0, 200, 0, 20)
        label.BackgroundTransparency = 1
        label.TextColor3 = Color3.fromRGB(255, 60, 60) -- Cor vermelha
        label.TextStrokeTransparency = 0 -- Contorno preto
        label.TextStrokeColor3 = Color3.new(0, 0, 0)
        label.Font = Enum.Font.GothamSemibold
        label.TextSize = 13
        label.Visible = false
        label.Parent = ScreenGui
        espLabels[plr] = label
    end
    return espLabels[plr]
end

RunService.RenderStepped:Connect(function()
    if not espEnabled then return end

    for _, plr in pairs(Players:GetPlayers()) do
        -- Verifica se o jogador é válido, tem personagem e se está vivo
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and plr.Character:FindFirstChild("Humanoid") and plr.Character.Humanoid.Health > 0 then
            local hrp = plr.Character.HumanoidRootPart
            local vector, onScreen = Camera:WorldToViewportPoint(hrp.Position)
            
            local label = getESPLabel(plr)

            if onScreen then
                local myHrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                local distance = 0
                if myHrp then
                    -- 1 Stud no Roblox equivale a cerca de 0.28 metros
                    distance = math.floor((hrp.Position - myHrp.Position).Magnitude * 0.28)
                end

                label.Text = string.format("%s [%dm]", plr.Name, distance)
                -- Posiciona o texto para ficar centrado em relação ao jogador
                label.Position = UDim2.new(0, vector.X - 100, 0, vector.Y - 10)
                label.Visible = true
            else
                label.Visible = false
            end
        elseif espLabels[plr] then
            -- Se o jogador morreu ou o personagem desapareceu, oculta o texto
            espLabels[plr].Visible = false
        end
    end
end)

-- Limpeza: Remove as labels se o jogador sair do servidor
Players.PlayerRemoving:Connect(function(plr)
    if espLabels[plr] then
        espLabels[plr]:Destroy()
        espLabels[plr] = nil
    end
end)

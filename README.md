-- Giant Hub v5.0

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GiantHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Name = "MainFrame"
Frame.Size = UDim2.new(0, 250, 0, 550)
Frame.Position = UDim2.new(0.2, 0, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local hubSizeState = "pequeno"

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "Giant Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextScaled = true
Title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Title.BorderSizePixel = 0
Title.Parent = Frame

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -35, 0, 5)
MinimizeBtn.Text = "-"
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextScaled = true
MinimizeBtn.Parent = Frame

local RestoreBtn = Instance.new("TextButton")
RestoreBtn.Size = UDim2.new(0, 120, 0, 40)
RestoreBtn.Position = UDim2.new(0.01, 0, 0.85, 0)
RestoreBtn.Text = "Abrir Giant Hub"
RestoreBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
RestoreBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
RestoreBtn.TextScaled = true
RestoreBtn.Visible = false
RestoreBtn.Parent = ScreenGui

local ResizeBtn = Instance.new("TextButton")
ResizeBtn.Size = UDim2.new(0, 80, 0, 30)
ResizeBtn.Position = UDim2.new(0, 5, 0, 5)
ResizeBtn.Text = "Grande"
ResizeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
ResizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ResizeBtn.TextScaled = true
ResizeBtn.Parent = Frame

-- Botões dinâmicos
local botaoY = 50
local function criarBotao(nome, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.Position = UDim2.new(0.05, 0, 0, botaoY)
    btn.Text = nome
    btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextScaled = true
    btn.Parent = Frame
    btn.MouseButton1Click:Connect(callback)
    botaoY += 45
end

-- Botões principais
criarBotao("Ativar Velocidade", function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildWhichIsA("Humanoid") then
        char:FindFirstChildWhichIsA("Humanoid").WalkSpeed = 100
    end
end)

criarBotao("Resetar Velocidade", function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildWhichIsA("Humanoid") then
        char:FindFirstChildWhichIsA("Humanoid").WalkSpeed = 16
    end
end)

criarBotao("Ativar Pulo Alto", function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildWhichIsA("Humanoid") then
        char:FindFirstChildWhichIsA("Humanoid").JumpPower = 120
    end
end)

criarBotao("Resetar Pulo", function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildWhichIsA("Humanoid") then
        char:FindFirstChildWhichIsA("Humanoid").JumpPower = 50
    end
end)

-- Caixa de texto para ações (Kill, View rápido)
local PlayerBox = Instance.new("TextBox")
PlayerBox.Size = UDim2.new(0.9, 0, 0, 35)
PlayerBox.Position = UDim2.new(0.05, 0, 0, botaoY)
PlayerBox.PlaceholderText = "Nome do Jogador"
PlayerBox.Text = ""
PlayerBox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
PlayerBox.TextColor3 = Color3.fromRGB(255, 255, 255)
PlayerBox.TextScaled = true
PlayerBox.Parent = Frame
botaoY += 45

-- Botão Kill Player
criarBotao("Kill (Jogador)", function()
    local targetName = PlayerBox.Text
    local target = game.Players:FindFirstChild(targetName)
    if target and target.Character and target.Character:FindFirstChildWhichIsA("Humanoid") then
        target.Character:FindFirstChildWhichIsA("Humanoid").Health = 0
    else
        warn("Jogador não encontrado ou não pode ser morto.")
    end
end)

-- Botão View Player (usando PlayerBox)
criarBotao("View (Jogador)", function()
    local targetName = PlayerBox.Text
    local target = game.Players:FindFirstChild(targetName)
    if target and target.Character and target.Character:FindFirstChild("Humanoid") then
        workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChild("Humanoid")
    else
        warn("Jogador não encontrado para visualizar.")
    end
end)

-- Botão Voltar para si mesmo
criarBotao("Voltar para mim", function()
    local player = game.Players.LocalPlayer
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        workspace.CurrentCamera.CameraSubject = player.Character:FindFirstChild("Humanoid")
    end
end)

-- NOVA Caixa de texto para View personalizado
local ViewBox = Instance.new("TextBox")
ViewBox.Size = UDim2.new(0.9, 0, 0, 35)
ViewBox.Position = UDim2.new(0.05, 0, 0, botaoY)
ViewBox.PlaceholderText = "Nome para View"
ViewBox.Text = ""
ViewBox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
ViewBox.TextColor3 = Color3.fromRGB(255, 255, 255)
ViewBox.TextScaled = true
ViewBox.Parent = Frame
botaoY += 45

-- Botão View separado
criarBotao("View (Nome do Player)", function()
    local viewName = ViewBox.Text
    local target = game.Players:FindFirstChild(viewName)
    if target and target.Character and target.Character:FindFirstChild("Humanoid") then
        workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChild("Humanoid")
    else
        warn("Jogador não encontrado para visualização.")
    end
end)

-- Minimizar
MinimizeBtn.MouseButton1Click:Connect(function()
    Frame.Visible = false
    RestoreBtn.Visible = true
end)

RestoreBtn.MouseButton1Click:Connect(function()
    Frame.Visible = true
    RestoreBtn.Visible = false
end)

-- Redimensionar
ResizeBtn.MouseButton1Click:Connect(function()
    if hubSizeState == "pequeno" then
        Frame.Size = UDim2.new(0, 400, 0, 600)
        ResizeBtn.Text = "Pequeno"
        hubSizeState = "grande"
    else
        Frame.Size = UDim2.new(0, 250, 0, 550)
        ResizeBtn.Text = "Grande"
        hubSizeState = "pequeno"
    end
end)

-- Interface: Mensagem "FEITO POR ISAAC" com estilo Minecraft

-- Proteção contra múltiplas execuções
if game.CoreGui:FindFirstChild("MensagemIsaac") then
    game.CoreGui:FindFirstChild("MensagemIsaac"):Destroy()
end

local TweenService = game:GetService("TweenService")

-- Cria a ScreenGui
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.Name = "MensagemIsaac"
screenGui.ResetOnSpawn = false

-- Cria o Frame de fundo (estilo fade)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 60)
frame.Position = UDim2.new(0.5, -150, 0.05, -30)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BackgroundTransparency = 0.3
frame.BorderSizePixel = 0
frame.Parent = screenGui
frame.AnchorPoint = Vector2.new(0.5, 0.5)

-- Cria o texto
local texto = Instance.new("TextLabel")
texto.Size = UDim2.new(1, 0, 1, 0)
texto.Position = UDim2.new(0, 0, 0, 0)
texto.BackgroundTransparency = 1
texto.Text = "FEITO POR ISAAC"
texto.TextColor3 = Color3.fromRGB(0, 255, 0)
texto.TextStrokeTransparency = 0.7
texto.Font = Enum.Font.Arcade -- Fonte estilo "Minecraft"
texto.TextScaled = true
texto.Parent = frame

-- Animação de entrada
frame.BackgroundTransparency = 1
texto.TextTransparency = 1
texto.TextStrokeTransparency = 1

TweenService:Create(frame, TweenInfo.new(0.5), {BackgroundTransparency = 0.3}):Play()
TweenService:Create(texto, TweenInfo.new(0.5), {TextTransparency = 0, TextStrokeTransparency = 0.7}):Play()

-- Espera 3 segundos
task.wait(3)

-- Animação de saída
TweenService:Create(frame, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
TweenService:Create(texto, TweenInfo.new(0.5), {TextTransparency = 1, TextStrokeTransparency = 1}):Play()

task.wait(0.6)
screenGui:Destroy()

-- UI BONITA E FUNCIONAL - BY CHATGPT

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")
local itemsFolder = workspace:WaitForChild("Items")

-- Criar a UI principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "BringItemsUI"

local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 240, 0, 350)
mainFrame.Position = UDim2.new(0.02, 0, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- Sombra (área para mover o painel)
local sombra = Instance.new("Frame", mainFrame)
sombra.Size = UDim2.new(1, 0, 0, 30)
sombra.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
sombra.BorderSizePixel = 0

-- Título
local titulo = Instance.new("TextLabel", sombra)
titulo.Size = UDim2.new(1, 0, 1, 0)
titulo.Text = "Painel de Puxar Itens"
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.BackgroundTransparency = 1
titulo.Font = Enum.Font.GothamBold
titulo.TextSize = 14

-- ScrollingFrame dos botões de tipos
local listaTipos = Instance.new("ScrollingFrame", mainFrame)
listaTipos.Size = UDim2.new(1, -10, 1, -70)
listaTipos.Position = UDim2.new(0, 5, 0, 40)
listaTipos.CanvasSize = UDim2.new(0, 0, 0, 0)
listaTipos.ScrollBarThickness = 4
listaTipos.BackgroundTransparency = 1
listaTipos.BorderSizePixel = 0

-- Botão "PUXAR TUDO"
local puxarTudo = Instance.new("TextButton", mainFrame)
puxarTudo.Size = UDim2.new(1, -10, 0, 30)
puxarTudo.Position = UDim2.new(0, 5, 1, -35)
puxarTudo.BackgroundColor3 = Color3.fromRGB(60, 120, 60)
puxarTudo.TextColor3 = Color3.new(1, 1, 1)
puxarTudo.Font = Enum.Font.GothamBold
puxarTudo.TextSize = 14
puxarTudo.Text = "PUXAR TUDO (?)"
puxarTudo.BorderSizePixel = 0

-- Tabela para rastrear botões
local botoes = {}

-- Função para puxar todos os itens de um tipo
local function puxarTipo(tipo)
    for _, item in pairs(itemsFolder:GetChildren()) do
        if item.Name == tipo then
            for _, obj in pairs(item:GetChildren()) do
                if obj:IsA("BasePart") then
                    obj.CFrame = root.CFrame + Vector3.new(math.random(-5,5), 2, math.random(-5,5))
                end
            end
        end
    end
end

-- Função para puxar todos os itens do mapa
local function puxarTudoFunc()
    local count = 0
    for _, item in pairs(itemsFolder:GetChildren()) do
        for _, obj in pairs(item:GetChildren()) do
            if obj:IsA("BasePart") then
                obj.CFrame = root.CFrame + Vector3.new(math.random(-5,5), 2, math.random(-5,5))
                count += 1
            end
        end
    end
    puxarTudo.Text = "PUXAR TUDO ("..count..")"
end

puxarTudo.MouseButton1Click:Connect(puxarTudoFunc)

-- Atualizador de UI
local function atualizarUI()
    local tiposAtuais = {}

    for _, item in pairs(itemsFolder:GetChildren()) do
        if not tiposAtuais[item.Name] and #item:GetChildren() > 0 then
            tiposAtuais[item.Name] = true
        end
    end

    -- Remover botões que não existem mais
    for nome, btn in pairs(botoes) do
        if not tiposAtuais[nome] then
            btn:Destroy()
            botoes[nome] = nil
        end
    end

    -- Adicionar novos botões
    local y = 0
    for nome in pairs(tiposAtuais) do
        if not botoes[nome] then
            local btn = Instance.new("TextButton", listaTipos)
            btn.Size = UDim2.new(1, -10, 0, 30)
            btn.Position = UDim2.new(0, 5, 0, y)
            btn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
            btn.TextColor3 = Color3.new(1, 1, 1)
            btn.Font = Enum.Font.Gotham
            btn.TextSize = 13
            btn.Text = nome
            btn.BorderSizePixel = 0

            btn.MouseButton1Click:Connect(function()
                puxarTipo(nome)
            end)

            botoes[nome] = btn
        end
        y = y + 35
    end
    listaTipos.CanvasSize = UDim2.new(0, 0, 0, y)
end

-- Atualizar automaticamente a cada segundo
task.spawn(function()
    while task.wait(1) do
        atualizarUI()
    end
end)

-- Atualizar tudo no início
task.wait(0.5)
atualizarUI()

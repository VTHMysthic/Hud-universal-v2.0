-- Serviços
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "HUDUniversal"
screenGui.Parent = game:GetService("CoreGui")

-- Função para criar frame com borda arredondada
local function createFrame(size, position)
    local frame = Instance.new("Frame")
    frame.Size = size
    frame.Position = position
    frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    frame.BorderSizePixel = 0
    frame.ClipsDescendants = true
    frame.Visible = true
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 15)
    corner.Parent = frame
    frame.Parent = screenGui
    return frame
end

-- Interfaces
local mainFrame = createFrame(UDim2.new(0, 400, 0, 250), UDim2.new(-1, 0, 0.5, -125))
local moveFrame = createFrame(UDim2.new(0, 400, 0, 250), UDim2.new(-1, 0, 0.5, -125))
moveFrame.Visible = false

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "HUD Universal v2.0"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.Cartoon
title.TextSize = 22
title.Parent = mainFrame

-- Função para criar botões
local function createButton(parent, text, position, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 250, 0, 50)
    btn.Position = position
    btn.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
    btn.BorderSizePixel = 0
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Cartoon
    btn.TextSize = 20
    btn.Parent = parent

    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 12)
    btnCorner.Parent = btn

    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(255, 200, 0)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(255, 170, 0)}):Play()
    end)

    btn.MouseButton1Click:Connect(callback)
end

-- Centralizar botões na interface normal
local buttonYStart = 60
createButton(mainFrame, "Velocidade", UDim2.new(0.5, -125, 0, buttonYStart), function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/VTHMysthic/Sander-zenitsu/refs/heads/main/README.md"))()
end)
createButton(mainFrame, "Super Pulo", UDim2.new(0.5, -125, 0, buttonYStart + 70), function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/VTHMysthic/Super-pulo-v1.0/refs/heads/main/README.md"))()
end)

-- Bola flutuante
local ball = Instance.new("TextButton")
ball.Size = UDim2.new(0, 50, 0, 50)
ball.Position = UDim2.new(0, 100, 0, 100)
ball.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
ball.BorderSizePixel = 2
ball.BorderColor3 = Color3.fromRGB(255, 255, 255)
ball.Text = "⚡️"
ball.TextSize = 25
ball.TextColor3 = Color3.fromRGB(255, 255, 255)
ball.Font = Enum.Font.Cartoon
ball.Parent = screenGui

local ballCorner = Instance.new("UICorner")
ballCorner.CornerRadius = UDim.new(0, 25)
ballCorner.Parent = ball

-- Abrir/fechar HUD
local hudOpen = false
local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
ball.MouseButton1Click:Connect(function()
    hudOpen = not hudOpen
    local goal = {}
    if hudOpen then
        goal.Position = UDim2.new(0.5, -200, 0.5, -125)
    else
        goal.Position = UDim2.new(-1, 0, 0.5, -125)
    end
    TweenService:Create(mainFrame, tweenInfo, goal):Play()
    TweenService:Create(moveFrame, tweenInfo, goal):Play()
end)

-- Emoji ⚙️ para alternar interfaces
local gearMain = Instance.new("TextButton")
gearMain.Size = UDim2.new(0, 40, 0, 40)
gearMain.Position = UDim2.new(1, -45, 0, 5)
gearMain.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
gearMain.BorderSizePixel = 1
gearMain.BorderColor3 = Color3.fromRGB(255, 255, 255)
gearMain.Text = "⚙️"
gearMain.TextSize = 25
gearMain.TextColor3 = Color3.fromRGB(255, 255, 255)
gearMain.Font = Enum.Font.Cartoon
gearMain.Parent = mainFrame

local gearCorner = Instance.new("UICorner")
gearCorner.CornerRadius = UDim.new(0, 8)
gearCorner.Parent = gearMain

-- Emoji ⚙️ na interface de movimentação
local gearMove = gearMain:Clone()
gearMove.Parent = moveFrame
gearMove.Position = UDim2.new(1, -45, 0, 5)

-- Interface de movimentação
local arrowButtons = {}
local arrowSize = 40
local spacing = 10 -- distância entre setas

local centerX = 0.5
local centerY = 0.5

local function createArrow(parent, direction)
    local arrow = Instance.new("TextButton")
    arrow.Size = UDim2.new(0, arrowSize, 0, arrowSize)
    arrow.AnchorPoint = Vector2.new(0.5, 0.5)

    local offsetX, offsetY = 0, 0
    if direction == "up" then
        offsetY = -(arrowSize + spacing)
        arrow.Text = "▲"
    elseif direction == "down" then
        offsetY = arrowSize + spacing
        arrow.Text = "▼"
    elseif direction == "left" then
        offsetX = -(arrowSize + spacing)
        arrow.Text = "◀"
    elseif direction == "right" then
        offsetX = arrowSize + spacing
        arrow.Text = "▶"
    end

    arrow.Position = UDim2.new(centerX, offsetX, centerY, offsetY)
    arrow.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    arrow.BorderSizePixel = 0
    arrow.TextColor3 = Color3.fromRGB(255, 255, 255)
    arrow.Font = Enum.Font.Cartoon
    arrow.TextSize = 25

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = arrow

    arrow.Parent = parent
    table.insert(arrowButtons, arrow)

    local moving = false
    local speed = 4
    arrow.MouseButton1Down:Connect(function() moving = true end)
    arrow.MouseButton1Up:Connect(function() moving = false end)
    arrow.MouseLeave:Connect(function() moving = false end)

    RunService.RenderStepped:Connect(function()
        if moving then
            local pos = ball.Position
            if direction == "up" then
                ball.Position = UDim2.new(pos.X.Scale, pos.X.Offset, pos.Y.Scale, pos.Y.Offset - speed)
            elseif direction == "down" then
                ball.Position = UDim2.new(pos.X.Scale, pos.X.Offset, pos.Y.Scale, pos.Y.Offset + speed)
            elseif direction == "left" then
                ball.Position = UDim2.new(pos.X.Scale, pos.X.Offset - speed, pos.Y.Scale, pos.Y.Offset)
            elseif direction == "right" then
                ball.Position = UDim2.new(pos.X.Scale, pos.X.Offset + speed, pos.Y.Scale, pos.Y.Offset)
            end
        end
    end)
end

-- Criar setas centralizadas e próximas no HUD
createArrow(moveFrame, "up")
createArrow(moveFrame, "down")
createArrow(moveFrame, "left")
createArrow(moveFrame, "right")

-- Alternar interfaces com emoji
local showingMove = false
local function toggleInterface()
    showingMove = not showingMove
    mainFrame.Visible = not showingMove
    moveFrame.Visible = showingMove
end

gearMain.MouseButton1Click:Connect(toggleInterface)
gearMove.MouseButton1Click:Connect(toggleInterface)

--// Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local Player = Players.LocalPlayer

--// Criando a GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ZenitsuInterface"
ScreenGui.Parent = game:GetService("CoreGui")

-- Frame principal
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 450, 0, 300)
Frame.Position = UDim2.new(0.5, -225, 0.5, -150)
Frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

-- Bordas e arredondamento
local UICornerFrame = Instance.new("UICorner")
UICornerFrame.CornerRadius = UDim.new(0, 15)
UICornerFrame.Parent = Frame

-- Fundo rainbow animado
local function RainbowColor()
    local hue = tick() % 5 / 5
    return Color3.fromHSV(hue, 1, 1)
end

RunService.RenderStepped:Connect(function()
    Frame.BackgroundColor3 = RainbowColor()
end)

-- Função para criar botões com degradê animado
local function CreateButton(Name, Size, Position, Colors, ScriptURL)
    local Button = Instance.new("TextButton")
    Button.Size = Size
    Button.Position = Position
    Button.BackgroundColor3 = Colors[1]
    Button.BorderColor3 = Color3.fromRGB(0,0,0)
    Button.BorderSizePixel = 3
    Button.Text = Name
    Button.TextColor3 = Color3.fromRGB(255,255,255)
    Button.Font = Enum.Font.Cartoon
    Button.TextScaled = true
    Button.Parent = Frame

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 10)
    UICorner.Parent = Button

    -- Degradê animado
    RunService.RenderStepped:Connect(function()
        local time = tick()
        local ratio = (math.sin(time*2)+1)/2
        local newColor = Colors[1]:Lerp(Colors[2], ratio)
        Button.BackgroundColor3 = newColor
    end)

    -- Executar script ao clicar
    Button.MouseButton1Click:Connect(function()
        if ScriptURL then
            loadstring(game:HttpGet(ScriptURL))()
        end
    end)
end

-- Botões
CreateButton(
    "sander o zenitsu veloz",
    UDim2.new(0, 200, 0, 60),
    UDim2.new(0, 20, 0, 40),
    {Color3.fromRGB(0,0,0), Color3.fromRGB(255,255,0)},
    "https://raw.githubusercontent.com/VTHMysthic/Sander-zenitsu/refs/heads/main/README.md"
)

CreateButton(
    "super pulo v1.0",
    UDim2.new(0, 200, 0, 60),
    UDim2.new(0, 230, 0, 40),
    {Color3.fromRGB(0,255,255), Color3.fromRGB(0,0,255)},
    "https://raw.githubusercontent.com/VTHMysthic/Super-pulo-v1.0/refs/heads/main/README.md"
)

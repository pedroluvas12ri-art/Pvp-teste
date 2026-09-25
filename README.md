local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

--==================================================
-- CONFIGURAÇÕES
--==================================================

local Whitelist = {}

local HighlightEnabled = false
local ArmEnabled = false
local ArmSize = 1

local OpenButtonVisible = true
local OpenButtonDraggable = true

--==================================================
-- GUI
--==================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PATO"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

--==================================================
-- BOLINHA
--==================================================

local OpenButton = Instance.new("TextButton")
OpenButton.Name = "OpenButton"
OpenButton.Size = UDim2.fromOffset(45, 45)
OpenButton.Position = UDim2.new(0, 10, 0.5, -22)
OpenButton.Text = "☰"
OpenButton.TextSize = 22
OpenButton.Font = Enum.Font.GothamBold
OpenButton.TextColor3 = Color3.new(1, 1, 1)
OpenButton.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
OpenButton.BorderSizePixel = 0
OpenButton.Parent = ScreenGui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(1, 0)
OpenCorner.Parent = OpenButton

--==================================================
-- PAINEL PRINCIPAL
--==================================================

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.fromOffset(360, 280)
MainFrame.Position = UDim2.new(0.5, -180, 0.5, -140)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

--==================================================
-- TÍTULO
--==================================================

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -55, 0, 45)
Title.Position = UDim2.fromOffset(10, 0)
Title.Text = "PATO"
Title.TextSize = 21
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

--==================================================
-- FECHAR
--==================================================

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.fromOffset(35, 35)
CloseButton.Position = UDim2.new(1, -42, 0, 5)
CloseButton.Text = "X"
CloseButton.TextSize = 17
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.BackgroundColor3 = Color3.fromRGB(170, 50, 50)
CloseButton.BorderSizePixel = 0
CloseButton.Parent = MainFrame

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseButton

--==================================================
-- SIDEBAR
--==================================================

local SideBar = Instance.new("Frame")
SideBar.Size = UDim2.new(0, 105, 1, -55)
SideBar.Position = UDim2.fromOffset(8, 48)
SideBar.BackgroundColor3 = Color3.fromRGB(27, 27, 33)
SideBar.BorderSizePixel = 0
SideBar.Parent = MainFrame

local SideCorner = Instance.new("UICorner")
SideCorner.CornerRadius = UDim.new(0, 8)
SideCorner.Parent = SideBar

--==================================================
-- ÁREA DE CONTEÚDO
--==================================================

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -125, 1, -55)
Content.Position = UDim2.fromOffset(118, 48)
Content.BackgroundColor3 = Color3.fromRGB(27, 27, 33)
Content.BorderSizePixel = 0
Content.ClipsDescendants = true
Content.Parent = MainFrame

local ContentCorner = Instance.new("UICorner")
ContentCorner.CornerRadius = UDim.new(0, 8)
ContentCorner.Parent = Content

--==================================================
-- CRIAR ABA
--==================================================

local function CreateTab(Name, Y)

    local Button = Instance.new("TextButton")

    Button.Size = UDim2.new(1, -10, 0, 38)
    Button.Position = UDim2.fromOffset(5, Y)
    Button.Text = Name
    Button.TextSize = 13
    Button.Font = Enum.Font.GothamBold
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
    Button.BorderSizePixel = 0
    Button.Parent = SideBar

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 7)
    Corner.Parent = Button

    return Button
end

local ESPTab = CreateTab("ESP", 8)
local ArmTab = CreateTab("BRAÇO", 52)
local WhiteTab = CreateTab("WHITELIST", 96)
local MiscTab = CreateTab("MISC", 140)

--==================================================
-- FUNÇÕES DE UI
--==================================================

local function ClearContent()

    for _, Object in ipairs(Content:GetChildren()) do
        Object:Destroy()
    end
end

local function CreateLabel(Text, Y, Height)

    local Label = Instance.new("TextLabel")

    Label.Size = UDim2.new(1, -20, 0, Height or 35)
    Label.Position = UDim2.fromOffset(10, Y)
    Label.Text = Text
    Label.TextSize = 14
    Label.Font = Enum.Font.GothamBold
    Label.TextColor3 = Color3.new(1, 1, 1)
    Label.BackgroundTransparency = 1
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.TextYAlignment = Enum.TextYAlignment.Center
    Label.TextWrapped = true
    Label.Parent = Content

    return Label
end

local function CreateButton(Text, Y, Height)

    local Button = Instance.new("TextButton")

    Button.Size = UDim2.new(1, -20, 0, Height or 38)
    Button.Position = UDim2.fromOffset(10, Y)
    Button.Text = Text
    Button.TextSize = 14
    Button.Font = Enum.Font.GothamBold
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
    Button.BorderSizePixel = 0
    Button.Parent = Content

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 7)
    Corner.Parent = Button

    return Button
end

--==================================================
-- ABA ESP
--==================================================

local function ShowESP()

    ClearContent()

    CreateLabel("ESP", 8, 35)

    local HighlightButton

    if HighlightEnabled then
        HighlightButton = CreateButton(
            "Highlight: LIGADO",
            48
        )
        HighlightButton.BackgroundColor3 =
            Color3.fromRGB(50, 120, 70)
    else
        HighlightButton = CreateButton(
            "Highlight: DESLIGADO",
            48
        )
    end

    HighlightButton.MouseButton1Click:Connect(function()

        HighlightEnabled = not HighlightEnabled

        ShowESP()
    end)

    CreateLabel(
        "Controle do Highlight da interface.",
        95,
        45
    )
end

--==================================================
-- ABA BRAÇO
--==================================================

local function ShowArm()

    ClearContent()

    CreateLabel("BRAÇO", 8, 35)

    local ToggleButton

    if ArmEnabled then

        ToggleButton = CreateButton(
            "Braço: LIGADO",
            48
        )

        ToggleButton.BackgroundColor3 =
            Color3.fromRGB(50, 120, 70)

    else

        ToggleButton = CreateButton(
            "Braço: DESLIGADO",
            48
        )
    end

    ToggleButton.MouseButton1Click:Connect(function()

        ArmEnabled = not ArmEnabled

        ShowArm()
    end)

    CreateLabel(
        "Valor do tamanho: " .. string.format("%.1f", ArmSize),
        95,
        35
    )

    local MinusButton = CreateButton("-", 135)

    MinusButton.MouseButton1Click:Connect(function()

        ArmSize = math.max(1, ArmSize - 0.1)

        ShowArm()
    end)

    local PlusButton = CreateButton("+", 180)

    PlusButton.MouseButton1Click:Connect(function()

        ArmSize = math.min(5, ArmSize + 0.1)

        ShowArm()
    end)

    CreateLabel(
        "Valor mínimo: 1.0 | máximo: 5.0",
        225,
        35
    )
end

--==================================================
-- ABA WHITELIST
--==================================================

local function ShowWhitelist()

    ClearContent()

    CreateLabel("WHITELIST", 8, 35)

    local Y = 48

    for _, Player in ipairs(Players:GetPlayers()) do

        if Player ~= LocalPlayer then

            local IsWhitelisted =
                Whitelist[Player.UserId] == true

            local Text

            if IsWhitelisted then
                Text = "✓ " .. Player.Name
            else
                Text = Player.Name
            end

            local Button = CreateButton(
                Text,
                Y
            )

            if IsWhitelisted then
                Button.BackgroundColor3 =
                    Color3.fromRGB(50, 120, 70)
            end

            Button.MouseButton1Click:Connect(function()

                if Whitelist[Player.UserId] then
                    Whitelist[Player.UserId] = nil
                else
                    Whitelist[Player.UserId] = true
                end

                ShowWhitelist()
            end)

            Y += 42
        end
    end

    if Y == 48 then

        CreateLabel(
            "Nenhum outro jogador encontrado.",
            55,
            45
        )
    end
end

--==================================================
-- ABA MISC
--==================================================

local function ShowMisc()

    ClearContent()

    CreateLabel("MISC", 8, 35)

    local ButtonText

    if OpenButtonVisible then
        ButtonText = "Bolinha: VISÍVEL"
    else
        ButtonText = "Bolinha: INVISÍVEL"
    end

    local ToggleButton =
        CreateButton(ButtonText, 48)

    if OpenButtonVisible then
        ToggleButton.BackgroundColor3 =
            Color3.fromRGB(50, 120, 70)
    end

    ToggleButton.MouseButton1Click:Connect(function()

        OpenButtonVisible = not OpenButtonVisible

        OpenButtonDraggable = OpenButtonVisible

        OpenButton.Visible = OpenButtonVisible

        ShowMisc()
    end)

    CreateLabel(
        "Visível: a bolinha pode ser movida.\nInvisível: a bolinha fica imóvel.",
        95,
        60
    )
end

--==================================================
-- EVENTOS DAS ABAS
--==================================================

ESPTab.MouseButton1Click:Connect(ShowESP)
ArmTab.MouseButton1Click:Connect(ShowArm)
WhiteTab.MouseButton1Click:Connect(ShowWhitelist)
MiscTab.MouseButton1Click:Connect(ShowMisc)

--==================================================
-- ABRIR / FECHAR
--==================================================

CloseButton.MouseButton1Click:Connect(function()

    MainFrame.Visible = false
end)

OpenButton.MouseButton1Click:Connect(function()

    MainFrame.Visible = not MainFrame.Visible
end)

--==================================================
-- ARRASTAR BOLINHA
--==================================================

local ButtonDragging = false
local ButtonDragStart
local ButtonStartPosition

OpenButton.InputBegan:Connect(function(Input)

    if not OpenButtonDraggable then
        return
    end

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        ButtonDragging = true
        ButtonDragStart = Input.Position
        ButtonStartPosition = OpenButton.Position
    end
end)

OpenButton.InputEnded:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        ButtonDragging = false
    end
end)

--==================================================
-- ARRASTAR PAINEL E BOLINHA
--==================================================

local PanelDragging = false
local PanelDragStart
local PanelStartPosition

Title.InputBegan:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        PanelDragging = true
        PanelDragStart = Input.Position
        PanelStartPosition = MainFrame.Position
    end
end)

Title.InputEnded:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        PanelDragging = false
    end
end)

UserInputService.InputChanged:Connect(function(Input)

    if Input.UserInputType ~= Enum.UserInputType.MouseMovement
        and Input.UserInputType ~= Enum.UserInputType.Touch then
        return
    end

    -- Mover bolinha
    if ButtonDragging and OpenButtonDraggable then

        local Delta =
            Input.Position - ButtonDragStart

        OpenButton.Position = UDim2.new(
            ButtonStartPosition.X.Scale,
            ButtonStartPosition.X.Offset + Delta.X,
            ButtonStartPosition.Y.Scale,
            ButtonStartPosition.Y.Offset + Delta.Y
        )
    end

    -- Mover painel
    if PanelDragging then

        local Delta =
            Input.Position - PanelDragStart

        MainFrame.Position = UDim2.new(
            PanelStartPosition.X.Scale,
            PanelStartPosition.X.Offset + Delta.X,
            PanelStartPosition.Y.Scale,
            PanelStartPosition.Y.Offset + Delta.Y
        )
    end
end)

--==================================================
-- ATUALIZAÇÃO DA LISTA DE JOGADORES
--==================================================

Players.PlayerAdded:Connect(function()

    if MainFrame.Visible then
        ShowWhitelist()
    end
end)

Players.PlayerRemoving:Connect(function(Player)

    Whitelist[Player.UserId] = nil

    if MainFrame.Visible then
        ShowWhitelist()
    end
end)

--==================================================
-- INICIAR
--==================================================

ShowESP()

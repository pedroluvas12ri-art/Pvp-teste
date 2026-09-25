local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

--==================================================
-- CONFIGURAÇÕES
--==================================================

local Whitelist = {}

local OpenButtonVisible = true
local OpenButtonDraggable = true

local HighlightEnabled = false
local ArmEnabled = false
local ArmSize = 1

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
OpenButton.Size = UDim2.fromOffset(45, 45)
OpenButton.Position = UDim2.new(0, 10, 0.5, -22)
OpenButton.Text = "☰"
OpenButton.TextSize = 22
OpenButton.Font = Enum.Font.GothamBold
OpenButton.TextColor3 = Color3.new(1, 1, 1)
OpenButton.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
OpenButton.Parent = ScreenGui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(1, 0)
OpenCorner.Parent = OpenButton

--==================================================
-- PAINEL
--==================================================

local MainFrame = Instance.new("Frame")
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
Title.Size = UDim2.new(1, -50, 0, 45)
Title.Position = UDim2.fromOffset(10, 0)
Title.Text = "PATO"
Title.TextSize = 21
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.fromOffset(35, 35)
CloseButton.Position = UDim2.new(1, -42, 0, 5)
CloseButton.Text = "X"
CloseButton.TextSize = 17
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.BackgroundColor3 = Color3.fromRGB(170, 50, 50)
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
-- CONTEÚDO
--==================================================

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -125, 1, -55)
Content.Position = UDim2.fromOffset(118, 48)
Content.BackgroundColor3 = Color3.fromRGB(27, 27, 33)
Content.BorderSizePixel = 0
Content.Parent = MainFrame

local ContentCorner = Instance.new("UICorner")
ContentCorner.CornerRadius = UDim.new(0, 8)
ContentCorner.Parent = Content

--==================================================
-- ABAS
--==================================================

local Tabs = {}

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

    Tabs[Name] = Button

    return Button
end

local ESPTab = CreateTab("ESP", 8)
local ArmTab = CreateTab("BRAÇO", 52)
local WhiteTab = CreateTab("WHITELIST", 96)
local MiscTab = CreateTab("MISC", 140)

--==================================================
-- FUNÇÕES
--==================================================

local function ClearContent()

    for _, Object in ipairs(Content:GetChildren()) do
        Object:Destroy()
    end
end

local function CreateLabel(Text, Y)

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -20, 0, 35)
    Label.Position = UDim2.fromOffset(10, Y)
    Label.Text = Text
    Label.TextSize = 15
    Label.Font = Enum.Font.GothamBold
    Label.TextColor3 = Color3.new(1, 1, 1)
    Label.BackgroundTransparency = 1
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Content

    return Label
end

local function CreateButton(Text, Y)

    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, -20, 0, 38)
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
-- ESP
--==================================================

local function ShowESP()

    ClearContent()

    CreateLabel("ESP", 10)

    local HighlightButton

    if HighlightEnabled then
        HighlightButton = CreateButton(
            "Highlight: LIGADO",
            55
        )
    else
        HighlightButton = CreateButton(
            "Highlight: DESLIGADO",
            55
        )
    end

    HighlightButton.MouseButton1Click:Connect(function()

        HighlightEnabled = not HighlightEnabled

        ShowESP()
    end)

    local Info = CreateLabel(
        "Controle visual do Highlight.",
        105
    )

    Info.TextWrapped = true
    Info.Size = UDim2.new(1, -20, 0, 45)
end

--==================================================
-- BRAÇO + HITBOX NA MESMA ESCALA
--==================================================

local function ResizeArm(Player)

	if Player == LocalPlayer then
		return
	end

	local Character = Player.Character

	if not Character then
		return
	end

	local Arm =
		Character:FindFirstChild("RightUpperArm")
		or Character:FindFirstChild("Right Arm")

	if not Arm or not Arm:IsA("BasePart") then
		return
	end

	-- Guarda o tamanho original
	if not OriginalArmSizes[Player] then
		OriginalArmSizes[Player] = Arm.Size
	end

	-- O tamanho visual e a área do braço
	-- acompanham a mesma escala
	Arm.Size = OriginalArmSizes[Player] * ArmScale
end

local function RestoreArm(Player)

	local Character = Player.Character

	if not Character then
		return
	end

	local Arm =
		Character:FindFirstChild("RightUpperArm")
		or Character:FindFirstChild("Right Arm")

	if Arm and OriginalArmSizes[Player] then
		Arm.Size = OriginalArmSizes[Player]
	end
end

local function UpdateArms()

	for _,Player in ipairs(Players:GetPlayers()) do

		if Player ~= LocalPlayer then

			if ArmEnabled then
				ResizeArm(Player)
			else
				RestoreArm(Player)
			end
		end
	end
end


--==================================================
-- TROCA DE ABAS
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

local DraggingButton = false
local ButtonDragStart
local ButtonStartPosition

OpenButton.InputBegan:Connect(function(Input)

    if not OpenButtonDraggable then
        return
    end

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        DraggingButton = true
        ButtonDragStart = Input.Position
        ButtonStartPosition = OpenButton.Position
    end
end)

OpenButton.InputEnded:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        DraggingButton = false
    end
end)

UserInputService.InputChanged:Connect(function(Input)

    if not DraggingButton then
        return
    end

    if not OpenButtonDraggable then
        return
    end

    if Input.UserInputType == Enum.UserInputType.MouseMovement
        or Input.UserInputType == Enum.UserInputType.Touch then

        local Delta =
            Input.Position - ButtonDragStart

        OpenButton.Position = UDim2.new(
            ButtonStartPosition.X.Scale,
            ButtonStartPosition.X.Offset + Delta.X,
            ButtonStartPosition.Y.Scale,
            ButtonStartPosition.Y.Offset + Delta.Y
        )
    end
end)

--==================================================
-- ARRASTAR PAINEL
--==================================================

local Dragging = false
local DragStart
local StartPosition

Title.InputBegan:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        Dragging = true
        DragStart = Input.Position
        StartPosition = MainFrame.Position
    end
end)

Title.InputEnded:Connect(function(Input)

    if Input.UserInputType == Enum.UserInputType.MouseButton1
        or Input.UserInputType == Enum.UserInputType.Touch then

        Dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(Input)

    if not Dragging then
        return
    end

    if Input.UserInputType == Enum.UserInputType.MouseMovement
        or Input.UserInputType == Enum.UserInputType.Touch then

        local Delta =
            Input.Position - DragStart

        MainFrame.Position = UDim2.new(
            StartPosition.X.Scale,
            StartPosition.X.Offset + Delta.X,
            StartPosition.Y.Scale,
            StartPosition.Y.Offset + Delta.Y
        )
    end
end)

--==================================================
-- ABA INICIAL
--==================================================

ShowESP()

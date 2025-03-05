-- Icon personalizado
local ScreenGui = Instance.new("ScreenGui")
local ImageButton = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

-- Propriedades da GUI
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Enabled = true -- Garante que a GUI está ativada

ImageButton.Parent = ScreenGui
ImageButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ImageButton.BorderSizePixel = 0
ImageButton.Position = UDim2.new(0.005, 0, 0.010, 0)
ImageButton.Size = UDim2.new(0, 50, 0, 50) -- Tamanho aumentado para facilitar a visualização
ImageButton.Image = "rbxassetid://139166819013498" -- ID do ícone

UICorner.Parent = ImageButton

-- Função para tornar o botão arrastável
local UIS = game:GetService("UserInputService")
local dragging, dragInput, startPos, startMousePos
local hasMoved = false

ImageButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        hasMoved = false
        startPos = ImageButton.Position
        startMousePos = UIS:GetMouseLocation()

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

ImageButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = UIS:GetMouseLocation() - startMousePos
        if math.abs(delta.X) > 5 or math.abs(delta.Y) > 5 then 
            hasMoved = true
        end
        ImageButton.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + delta.X,
            startPos.Y.Scale, startPos.Y.Offset + delta.Y
        )
    end
end)

ImageButton.MouseButton1Down:Connect(function()
    task.wait(0.1)
    if not hasMoved then
        game:GetService("VirtualInputManager"):SendKeyEvent(true, Enum.KeyCode.LeftControl, false, game)
    end
end)

-- Carregar Fluent Library
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

if not Fluent then
    warn("Erro ao carregar Fluent Library")
    return
end

local MarketplaceService = game:GetService("MarketplaceService")
local PlaceId = game.PlaceId
local ProductInfo = MarketplaceService:GetProductInfo(PlaceId)
local GameName = ProductInfo.Name

Fluent:Notify({ 
    Title = "Script executado com sucesso", 
    Content = "Você está usando Mender_hub",
    Duration = 4 
})

local Window = Fluent:CreateWindow({
    Title = "Mender_hub",
    SubTitle = "-- " .. GameName,
    TabWidth = 102,
    Size = UDim2.fromOffset(450, 320),
    Acrylic = false,
    Theme = "Amethyst",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Info = Window:AddTab({ Title = "Info", Icon = "scroll" }),
    Farm = Window:AddTab({ Title = "Farm", Icon = "rbxassetid://18831424669" }),
    Teleport = Window:AddTab({ Title = "Teleports", Icon = "rbxassetid://18831424669" }),
    Eggs = Window:AddTab({ Title = "Hatch", Icon = "egg" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" }),
}

Tabs.Info:AddParagraph({
    Title = "Esse script está em atualização",
    Content = "Correções de bugs\nAdicionado Magnet Drops\nAdicionado Teleporte\nAdicionado Rank Up"
})

-- Auto Click (Farm)
local AutoClickFarm = Tabs.Farm:AddToggle("AutoClickFarm", {Title = "Auto Click", Default = false})
AutoClickFarm:OnChanged(function()
    while AutoClickFarm.Value do
        local args = {
            [1] = "Enemies",
            [2] = "World",
            [3] = "Click"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
        task.wait(0.1)
    end
end)

-- Teleporte
Tabs.Teleport:AddButton({
    Title = "Lobby",
    Callback = function()
        local args = {
            [1] = "General",
            [2] = "Maps",
            [3] = "Teleport",
            [4] = "Lobby"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
        print("Teleportado para Lobby")
    end
})

Tabs.Teleport:AddButton({
    Title = "Piece Village",
    Callback = function()
        local args = {
            [1] = "General",
            [2] = "Maps",
            [3] = "Teleport",
            [4] = "Piece Village"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
        print("Teleportado para Piece Village")
    end
})

-- Auto Hatch (Eggs)
local AutoHatch = Tabs.Eggs:AddToggle("AutoHatch", {Title = "Piece village", Default = false})
AutoHatch:OnChanged(function()
    while AutoHatch.Value do
        local args = {
            [1] = "General",
            [2] = "Eggs",
            [3] = "Multi",
            [4] = "Piece Village",
            [5] = "Yen"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
        task.wait(0.1)
    end
end)

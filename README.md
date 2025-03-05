-- Icon personalizado
local ScreenGui = Instance.new("ScreenGui")
local ImageButton = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

-- Propriedades
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton.Parent = ScreenGui
ImageButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ImageButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
ImageButton.BorderSizePixel = 0
ImageButton.Position = UDim2.new(0.005, 0, 0.010, 0)
ImageButton.Size = UDim2.new(0, 45, 0, 45)
ImageButton.Image = "rbxassetid://139166819013498" -- Id da icon aqui

UICorner.Parent = ImageButton

-- FunÃ§Ã£o para tornar o botÃ£o arrastÃ¡vel
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
    task.wait(0.1) -- Isso serve pro icone nao abrir quando a pessoa for arrastar ele, mexe aqui nÃ£o
    if not hasMoved then
        game:GetService("VirtualInputManager"):SendKeyEvent(true, Enum.KeyCode.LeftControl, false, game)
    end
end)

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()
local MarketplaceService = game:GetService("MarketplaceService")
local PlaceId = game.PlaceId
local ProductInfo = MarketplaceService:GetProductInfo(PlaceId)
local GameName = ProductInfo.Name

Fluent:Notify({ Title = "Script executado com sucesso", Content = "Você esta usando Mender_hub",
Duration= 4 
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
    Info= Window:AddTab({ Title = "Info", Icon = "scroll" }),
    Farm= Window:AddTab({ Title = " Farm", Icon = "sword" }),
    Teleport= Window:AddTab({ Title = "Teleports", Icon = "sword" }),
    Eggs= Window:AddTab({ Title = "Hath", Icon = "egg" }),
    Main= Window:AddTab({ Title = "Main", Icon = "rbxassetid://18831424669" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" }),
}

Tabs.Info:AddParagraph({
        Title = "Esse script esta em update",
        Content = "Bug Fixeds \nAdd Magnect Drops \nAdd Teleport \nAdd rank Up"
    })


local AutoClick= Tabs.Farm:AddToggle("Auto Click", {Title = "Auto click", Default = false})
AutoClick:OnChanged(function()
    while AutoClick.Value do
    --remote
    
local args = {
    [1] = "Enemies",
    [2] = "World",
    [3] = "Click"
}
game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
wait(0)
           end
end)
Teleport:AddButton({
    Title = "Lobby",
    Callback = function()
--Clover
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
Teleport:AddButton({
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
local AutoClick= Tabs.Eggs:AddToggle("Village", {Title = "Piece Village|Mult", Default = false})
AutoClick:OnChanged(function()
    while AutoClick.Value do
--remote
local args = {
    [1] = "General",
    [2] = "Eggs",
    [3] = "Multi",
    [4] = "Piece Village",
    [5] = "Yen"
}

game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Bridge"):FireServer(unpack(args))
        wait(0)
    end
end)

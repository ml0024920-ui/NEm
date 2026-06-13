-- Serviços Nativos do Roblox
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer

-- Prevenção de duplicação de UI
if CoreGui:FindFirstChild("OpenSourceHub") then
    CoreGui.OpenSourceHub:Destroy()
end

-- Estrutura da Biblioteca
local Library = {}
Library.Theme = {
    Background = Color3.fromRGB(25, 25, 25),
    TopBar = Color3.fromRGB(35, 35, 35),
    Accent = Color3.fromRGB(85, 170, 255),
    Text = Color3.fromRGB(255, 255, 255),
    TextDark = Color3.fromRGB(150, 150, 150)
}

function Library:CreateWindow(WindowName)
    local Window = {}
    
    -- UI Base
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "OpenSourceHub"
    ScreenGui.Parent = CoreGui
    ScreenGui.ResetOnSpawn = false

    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 500, 0, 350)
    MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
    MainFrame.BackgroundColor3 = Library.Theme.Background
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = ScreenGui
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 8)
    UICorner.Parent = MainFrame

    -- Barra Superior
    local TopBar = Instance.new("Frame")
    TopBar.Size = UDim2.new(1, 0, 0, 35)
    TopBar.BackgroundColor3 = Library.Theme.TopBar
    TopBar.BorderSizePixel = 0
    TopBar.Parent = MainFrame
    
    local TopCorner = Instance.new("UICorner")
    TopCorner.CornerRadius = UDim.new(0, 8)
    TopCorner.Parent = TopBar

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, -20, 1, 0)
    Title.Position = UDim2.new(0, 15, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = WindowName
    Title.TextColor3 = Library.Theme.Text
    Title.TextSize = 16
    Title.Font = Enum.Font.GothamBold
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = TopBar

    -- Área de Conteúdo e Abas
    local TabContainer = Instance.new("Frame")
    TabContainer.Size = UDim2.new(0, 130, 1, -35)
    TabContainer.Position = UDim2.new(0, 0, 0, 35)
    TabContainer.BackgroundTransparency = 1
    TabContainer.Parent = MainFrame

    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Padding = UDim.new(0, 5)
    UIListLayout.Parent = TabContainer

    local ContentContainer = Instance.new("Frame")
    ContentContainer.Size = UDim2.new(1, -140, 1, -45)
    ContentContainer.Position = UDim2.new(0, 135, 0, 40)
    ContentContainer.BackgroundTransparency = 1
    ContentContainer.Parent = MainFrame

    function Window:CreateTab(TabName)
        local Tab = {}
        
        -- Botão da Aba
        local TabButton = Instance.new("TextButton")
        TabButton.Size = UDim2.new(1, -10, 0, 30)
        TabButton.Position = UDim2.new(0, 5, 0, 0)
        TabButton.BackgroundColor3 = Library.Theme.TopBar
        TabButton.Text = TabName
        TabButton.TextColor3 = Library.Theme.TextDark
        TabButton.Font = Enum.Font.Gotham
        TabButton.TextSize = 14
        TabButton.Parent = TabContainer
        
        local TabCorner = Instance.new("UICorner")
        TabCorner.CornerRadius = UDim.new(0, 4)
        TabCorner.Parent = TabButton

        -- Página de Conteúdo
        local Page = Instance.new("ScrollingFrame")
        Page.Size = UDim2.new(1, 0, 1, 0)
        Page.BackgroundTransparency = 1
        Page.ScrollBarThickness = 2
        Page.Visible = false
        Page.Parent = ContentContainer

        local PageLayout = Instance.new("UIListLayout")
        PageLayout.SortOrder = Enum.SortOrder.LayoutOrder
        PageLayout.Padding = UDim.new(0, 8)
        PageLayout.Parent = Page

        TabButton.MouseButton1Click:Connect(function()
            for _, child in pairs(ContentContainer:GetChildren()) do
                if child:IsA("ScrollingFrame") then
                    child.Visible = false
                end
            end
            for _, btn in pairs(TabContainer:GetChildren()) do
                if btn:IsA("TextButton") then
                    TweenService:Create(btn, TweenInfo.new(0.2), {TextColor3 = Library.Theme.TextDark}):Play()
                end
            end
            Page.Visible = true
            TweenService:Create(TabButton, TweenInfo.new(0.2), {TextColor3 = Library.Theme.Accent}):Play()
        end)

        -- Elementos da Página (Toggle)
        function Tab:CreateToggle(ToggleName, Callback)
            local toggled = false
            local ToggleFrame = Instance.new("Frame")
            ToggleFrame.Size = UDim2.new(1, -10, 0, 35)
            ToggleFrame.BackgroundColor3 = Library.Theme.TopBar
            ToggleFrame.Parent = Page
            
            local TCorner = Instance.new("UICorner")
            TCorner.CornerRadius = UDim.new(0, 4)
            TCorner.Parent = ToggleFrame

            local Label = Instance.new("TextLabel")
            Label.Size = UDim2.new(1, -50, 1, 0)
            Label.Position = UDim2.new(0, 10, 0, 0)
            Label.BackgroundTransparency = 1
            Label.Text = ToggleName
            Label.TextColor3 = Library.Theme.Text
            Label.Font = Enum.Font.Gotham
            Label.TextSize = 14
            Label.TextXAlignment = Enum.TextXAlignment.Left
            Label.Parent = ToggleFrame

            local ToggleBtn = Instance.new("TextButton")
            ToggleBtn.Size = UDim2.new(0, 40, 0, 20)
            ToggleBtn.Position = UDim2.new(1, -50, 0.5, -10)
            ToggleBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            ToggleBtn.Text = ""
            ToggleBtn.Parent = ToggleFrame
            
            local BtnCorner = Instance.new("UICorner")
            BtnCorner.CornerRadius = UDim.new(1, 0)
            BtnCorner.Parent = ToggleBtn

            local Circle = Instance.new("Frame")
            Circle.Size = UDim2.new(0, 16, 0, 16)
            Circle.Position = UDim2.new(0, 2, 0.5, -8)
            Circle.BackgroundColor3 = Library.Theme.Text
            Circle.Parent = ToggleBtn
            
            local CircleCorner = Instance.new("UICorner")
            CircleCorner.CornerRadius = UDim.new(1, 0)
            CircleCorner.Parent = Circle

            ToggleBtn.MouseButton1Click:Connect(function()
                toggled = not toggled
                if toggled then
                    TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {BackgroundColor3 = Library.Theme.Accent}):Play()
                    TweenService:Create(Circle, TweenInfo.new(0.2), {Position = UDim2.new(1, -18, 0.5, -8)}):Play()
                else
                    TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 50)}):Play()
                    TweenService:Create(Circle, TweenInfo.new(0.2), {Position = UDim2.new(0, 2, 0.5, -8)}):Play()
                end
                pcall(Callback, toggled)
            end)
        end

        return Tab
    end

    return Window
end

-- Inicialização do Framework e Criação das Categorias
local MyWindow = Library:CreateWindow("Project: Horizon | Open-Source")

-- Aba 1: Progresso
local FarmTab = MyWindow:CreateTab("Auto-Farm")
FarmTab:CreateToggle("Habilitar Auto-Farm Level", function(state)
    -- Lógica modular de automação (Loop)
    getgenv().AutoFarm = state
    while getgenv().AutoFarm do
        task.wait(0.5)
        print("Buscando NPC mais próximo...")
    end
end)

FarmTab:CreateToggle("Auto-Maestria", function(state)
    getgenv().AutoMastery = state
end)

-- Aba 2: Utilitários
local UtilsTab = MyWindow:CreateTab("Utilitários")
UtilsTab:CreateToggle("Auto-Raça V4 (Mirage)", function(state)
    getgenv().AutoRace = state
end)

-- Aba 3: Combate
local CombatTab = MyWindow:CreateTab("Combate (PvP)")
CombatTab:CreateToggle("Aimbot Habilidades", function(state)
    getgenv().Aimbot = state
end)

-- Aba 4: Localizadores
local SniperTab = MyWindow:CreateTab("Fruit Sniper")
SniperTab:CreateToggle("Notificar Frutas no Chão", function(state)
    getgenv().FruitSniper = state
end)

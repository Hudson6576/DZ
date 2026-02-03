-- LocalScript para a GUI de estilo Redz Hub (colocar em StarterGui)

-- Obtém o PlayerGui para inserir a GUI
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Cria a ScreenGui que conterá todos os elementos visuais
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DZGui"
screenGui.ResetOnSpawn = false    -- Persiste após respawn
screenGui.IgnoreGuiInset = true   -- Ignora barras de topo em dispositivos
screenGui.Parent = playerGui

-- Adiciona UIScale para responsividade (pode ajustar conforme a resolução)
local uiScale = Instance.new("UIScale")
uiScale.Parent = screenGui
uiScale.Scale = 0.9  -- Escala inicial (pode ser alterada no código)

-- === Botão de ativação (ícone "DZ") ===
local openButton = Instance.new("ImageButton")
openButton.Name = "OpenButton"
openButton.Parent = screenGui
openButton.Image = "rbxassetid://SEU_ID_DE_ICONE_AQUI"  -- Defina o ID do ícone 'DZ'
openButton.Size = UDim2.new(0, 50, 0, 50)
openButton.Position = UDim2.new(0, 10, 0, 10)           -- Canto superior esquerdo
openButton.AnchorPoint = Vector2.new(0, 0)
openButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
openButton.BackgroundTransparency = 0.5
openButton.BorderSizePixel = 0
-- Borda arredondada no botão de ativação
local openCorner = Instance.new("UICorner")
openCorner.Parent = openButton
openCorner.CornerRadius = UDim.new(0, 8)

-- === Painel Principal (inicialmente oculto) ===
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainPanel"
mainFrame.Parent = screenGui
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)   -- Centralizado na tela
mainFrame.Size = UDim2.new(0.7, 0, 0.7, 0)       -- 70% da largura e altura da tela
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false    -- Começa invisível

-- Borda arredondada no painel principal
local panelCorner = Instance.new("UICorner")
panelCorner.Parent = mainFrame
panelCorner.CornerRadius = UDim.new(0, 12)

-- Título no topo do painel (por exemplo, "DZ")
local titleLabel = Instance.new("TextLabel")
titleLabel.Parent = mainFrame
titleLabel.Text = "DZ"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 24
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.BackgroundTransparency = 1
titleLabel.Size = UDim2.new(1, -20, 0, 30)
titleLabel.Position = UDim2.new(0, 10, 0, 10)
titleLabel.TextStrokeTransparency = 0.7  -- Ligeira sombra no texto

-- Botão de fechamento ("X") no canto superior direito do painel
local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Parent = mainFrame
closeButton.Text = "X"
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 20
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
closeButton.Size = UDim2.new(0, 25, 0, 25)
closeButton.AnchorPoint = Vector2.new(1, 0)
closeButton.Position = UDim2.new(1, -10, 0, 10)  -- canto superior direito
closeButton.BorderSizePixel = 0
local closeCorner = Instance.new("UICorner")
closeCorner.Parent = closeButton
closeCorner.CornerRadius = UDim.new(0, 6)

-- === Menu Lateral de Abas ===
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Parent = mainFrame
sideMenu.Size = UDim2.new(0.22, 0, 1, -40)   -- 22% da largura, altura restante abaixo do título
sideMenu.Position = UDim2.new(0, 0, 0, 40)   -- começa abaixo do título
sideMenu.BackgroundTransparency = 1

-- Layout vertical para organizar os botões das abas
local sideLayout = Instance.new("UIListLayout")
sideLayout.Parent = sideMenu
sideLayout.SortOrder = Enum.SortOrder.LayoutOrder
sideLayout.Padding = UDim.new(0, 4)

-- Lista de abas (nome interno e texto exibido)
local tabs = {
    {Name = "Home",    Text = "Home"},
    {Name = "Main",    Text = "Main"},
    {Name = "ItemQuests", Text = "Item / Quests"},
    {Name = "SeaEvent",    Text = "Sea Event"},
    {Name = "Other",   Text = "Other"},
    {Name = "Misc",    Text = "Misc"},
    {Name = "Settings",Text = "Settings"}
}

-- Tabelas para armazenar referências dos botões e frames de conteúdo
local tabButtons = {}
local tabContents = {}

for i, tabInfo in ipairs(tabs) do
    -- Botão da aba lateral
    local btn = Instance.new("TextButton")
    btn.Name = tabInfo.Name.."Button"
    btn.Parent = sideMenu
    btn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 18
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = tabInfo.Text

    -- Indicador de aba ativa (barra vermelha lateral)
    local indicator = Instance.new("Frame")
    indicator.Name = "Indicator"
    indicator.Parent = btn
    indicator.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    indicator.Size = UDim2.new(0, 4, 1, 0)
    indicator.Position = UDim2.new(0, 0, 0, 0)
    indicator.Visible = false  -- somente visível quando a aba está selecionada

    -- Frame de conteúdo para esta aba (invisível inicialmente)
    local contentFrame = Instance.new("Frame")
    contentFrame.Name = tabInfo.Name.."Content"
    contentFrame.Parent = mainFrame
    contentFrame.Size = UDim2.new(0.78, -10, 1, -40)  -- ocupa o espaço à direita do menu
    contentFrame.Position = UDim2.new(0.22, 0, 0, 40)
    contentFrame.BackgroundTransparency = 1
    contentFrame.Visible = false

    -- Exemplo de conteúdo para a aba "ItemQuests"
    if tabInfo.Name == "ItemQuests" then
        local worlds = {"First World", "Second World", "Third World"}
        for j, wName in ipairs(worlds) do
            local wBtn = Instance.new("TextButton")
            wBtn.Name = "World"..j.."Btn"
            wBtn.Parent = contentFrame
            wBtn.Text = wName
            wBtn.Size = UDim2.new(0, 200, 0, 30)
            wBtn.Position = UDim2.new(0, 10, 0, 10 + (j-1) * 35)
            wBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
            wBtn.BorderSizePixel = 0
            wBtn.Font = Enum.Font.Gotham
            wBtn.TextSize = 16
            wBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            local btnCorner = Instance.new("UICorner")
            btnCorner.Parent = wBtn
            btnCorner.CornerRadius = UDim.new(0, 6)
        end
    else
        -- Texto de exemplo para demais abas
        local sampleLabel = Instance.new("TextLabel")
        sampleLabel.Parent = contentFrame
        sampleLabel.Text = tabInfo.Name.." em construção..."
        sampleLabel.Font = Enum.Font.Gotham
        sampleLabel.TextSize = 20
        sampleLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
        sampleLabel.BackgroundTransparency = 1
        sampleLabel.Size = UDim2.new(1, -10, 0, 30)
        sampleLabel.Position = UDim2.new(0, 10, 0, 10)
    end

    -- Armazena referências para controlar abas
    tabButtons[tabInfo.Name] = btn
    tabContents[tabInfo.Name] = contentFrame
end

-- Função para trocar de aba: esconde todas e mostra a selecionada
local function selectTab(tabName)
    for name, btn in pairs(tabButtons) do
        btn.Indicator.Visible = (name == tabName)
        tabContents[name].Visible = (name == tabName)
    end
end

-- Conecta eventos de clique nos botões das abas laterais
for name, btn in pairs(tabButtons) do
    btn.MouseButton1Click:Connect(function()
        selectTab(name)
    end)
end

-- === Abertura e fechamento do painel ===
openButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    selectTab("Home")  -- Seleciona aba inicial (pode ser alterada para outra)
end)
closeButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
end)

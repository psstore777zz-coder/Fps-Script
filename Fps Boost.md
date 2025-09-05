--[[  
FPS Boost+  
ttk: breaksstore
]]--

local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FPSBoostPlus"
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- BOTÃO ABRIR GUI MAIS BONITO
local openButton = Instance.new("TextButton")
openButton.Size = UDim2.new(0,60,0,60)
openButton.Position = UDim2.new(0.5, -30, 0, -100) -- inicia fora da tela, no topo
openButton.BackgroundColor3 = Color3.fromRGB(40,40,40)
openButton.BackgroundTransparency = 0
openButton.TextColor3 = Color3.fromRGB(255,255,255)
openButton.Font = Enum.Font.GothamBold
openButton.TextSize = 26
openButton.Text = "⚡"
openButton.Parent = screenGui
openButton.Draggable = true
openButton.ClipsDescendants = true

local circleCorner = Instance.new("UICorner")
circleCorner.CornerRadius = UDim.new(1,0)
circleCorner.Parent = openButton

local shadow = Instance.new("UIStroke")
shadow.Color = Color3.fromRGB(255,255,255)
shadow.Thickness = 1
shadow.Transparency = 0.6
shadow.Parent = openButton

openButton.MouseEnter:Connect(function()
    openButton.BackgroundColor3 = Color3.fromRGB(70,70,70)
end)
openButton.MouseLeave:Connect(function()
    openButton.BackgroundColor3 = Color3.fromRGB(40,40,40)
end)

-- Animação de entrada do botão (desliza do topo até a posição inicial)
TweenService:Create(openButton, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
    Position = UDim2.new(0.5, -30, 0, 20)
}):Play()

-- Janela principal decorada
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0,280,0,360) -- altura ajustada
mainFrame.Position = UDim2.new(0.5,-140,0.5,-180)
mainFrame.BackgroundColor3 = Color3.fromRGB(25,25,25)
mainFrame.BackgroundTransparency = 0.3
mainFrame.Visible = false
mainFrame.Parent = screenGui
mainFrame.Draggable = true
mainFrame.Active = true

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0,20)
frameCorner.Parent = mainFrame

-- Borda preta
local border = Instance.new("Frame")
border.Size = UDim2.new(1, -4, 1, -4)
border.Position = UDim2.new(0,2,0,2)
border.BackgroundColor3 = Color3.fromRGB(0,0,0)
border.BackgroundTransparency = 0.3
border.BorderSizePixel = 0
border.Parent = mainFrame
local borderCorner = Instance.new("UICorner")
borderCorner.CornerRadius = UDim.new(0,18)
borderCorner.Parent = border

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,0,0,30)
title.Position = UDim2.new(0,0,0,0)
title.BackgroundTransparency = 1
title.Text = "✨ FPS Boost+"
title.TextColor3 = Color3.fromRGB(255,255,255)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextStrokeTransparency = 0
title.Parent = mainFrame

-- Subtítulo
local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1,0,0,20)
subtitle.Position = UDim2.new(0,0,0,30)
subtitle.BackgroundTransparency = 1
subtitle.Text = "ttk: breaksstore"
subtitle.TextColor3 = Color3.fromRGB(200,200,200)
subtitle.Font = Enum.Font.GothamBold
subtitle.TextSize = 14
subtitle.TextStrokeTransparency = 0
subtitle.Parent = mainFrame

-- Botões com hover efeito
local function createButton(text, yPos)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.8,0,0,40)
    btn.Position = UDim2.new(0.1,0,0,yPos)
    btn.BackgroundColor3 = Color3.fromRGB(0,0,0)
    btn.BackgroundTransparency = 0.1
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.Parent = mainFrame
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0,12)
    corner.Parent = btn
    local shadow = Instance.new("UIStroke")
    shadow.Color = Color3.fromRGB(255,255,255)
    shadow.Thickness = 1
    shadow.Transparency = 0.6
    shadow.Parent = btn
    btn.MouseEnter:Connect(function()
        btn.BackgroundTransparency = 0
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundTransparency = 0.1
    end)
    return btn
end

local boostButton = createButton("Ativar FPS Boost",60)
local disableButton = createButton("Desativar Boost",110)
local stretchButton = createButton("Esticar Tela",160)
local invisButton = createButton("Ficar Invisível",210)
-- Novos botões de teleporte
local teleportButton = createButton("Salvar Local",260)
local goButton = createButton("Teleportar",310)

---------------------------------------------------
-- FPS BOOST SUAVE
---------------------------------------------------
local originalMaterials = {}
for _, obj in ipairs(workspace:GetDescendants()) do
    if obj:IsA("BasePart") and not obj:IsDescendantOf(Players) then
        originalMaterials[obj] = {Material=obj.Material, Color=obj.Color, Reflectance=obj.Reflectance}
    end
end

local function fpsBoost()
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(Players) then
            obj.Material = Enum.Material.SmoothPlastic
            obj.Reflectance = 0
        elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") 
            or obj:IsA("Fire") or obj:IsA("Sparkles") then
            obj.Enabled = false
        end
    end
    local lighting = game:GetService("Lighting")
    lighting.GlobalShadows = false
    lighting.FogEnd = 9e9
    lighting.Brightness = 1.5
    lighting.OutdoorAmbient = Color3.fromRGB(140,140,140)
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
end

local function disableFpsBoost()
    for obj,data in pairs(originalMaterials) do
        if obj and obj:IsA("BasePart") then
            obj.Material = data.Material
            obj.Color = data.Color
            obj.Reflectance = data.Reflectance
        end
    end
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") 
            or obj:IsA("Fire") or obj:IsA("Sparkles") then
            obj.Enabled = true
        end
    end
    local lighting = game:GetService("Lighting")
    lighting.GlobalShadows = true
    lighting.FogEnd = 1000
    lighting.Brightness = 2
    lighting.OutdoorAmbient = Color3.fromRGB(128,128,128)
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level21
    boostButton.Text = "Ativar FPS Boost"
end

---------------------------------------------------
-- ESTICAR TELA
---------------------------------------------------
local stretched = false
local stretchConnection

local function toggleStretch()
    local Camera = workspace.CurrentCamera
    if not stretched then
        getgenv().Resolution = {[".gg/scripters"]=0.65}
        if getgenv().gg_scripters == nil then
            stretchConnection = RunService.RenderStepped:Connect(function()
                Camera.CFrame = Camera.CFrame * CFrame.new(
                    0,0,0,
                    1,0,0,
                    0,getgenv().Resolution[".gg/scripters"],0,
                    0,0,1
                )
            end)
        end
        getgenv().gg_scripters = "Aori0001"
        stretched = true
        stretchButton.Text = "✅ Tela Esticada"
        stretchButton.BackgroundColor3 = Color3.fromRGB(40,120,180)
    else
        if stretchConnection then
            stretchConnection:Disconnect()
            stretchConnection = nil
        end
        stretched = false
        stretchButton.Text = "Esticar Tela"
        stretchButton.BackgroundColor3 = Color3.fromRGB(0,0,0)
    end
end

---------------------------------------------------
-- INVISIBILIDADE COM BRAINROT MELHORADA
---------------------------------------------------
local invisible = false
local function toggleInvisibility()
    local character = player.Character
    if not character then return end

    local tool = character:FindFirstChild("Brainrot") or player.Backpack:FindFirstChild("Brainrot")

    invisible = not invisible -- alterna estado

    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Transparency = invisible and 1 or 0
            part.CanCollide = not invisible
        elseif part:IsA("Accessory") and part:FindFirstChild("Handle") then
            part.Handle.Transparency = invisible and 1 or 0
            part.Handle.CanCollide = not invisible
        end
    end

    -- Brainrot invisível também
    if tool and tool:IsA("Tool") then
        for _, tpart in pairs(tool:GetChildren()) do
            if tpart:IsA("BasePart") then
                tpart.Transparency = invisible and 1 or 0
                tpart.CanCollide = not invisible
            end
        end
    end
end

---------------------------------------------------
-- TELEPORTADOR TELEGUIDADO
---------------------------------------------------
local teleportSavePosition = nil

teleportButton.MouseButton1Click:Connect(function()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        teleportSavePosition = character.HumanoidRootPart.Position
        teleportButton.Text = "Local Salvo!"
        task.delay(1.5, function()
            teleportButton.Text = "Salvar Local"
        end)
    end
end)

goButton.MouseButton1Click:Connect(function()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") and teleportSavePosition then
        character.HumanoidRootPart.CFrame = CFrame.new(teleportSavePosition)
    end
end)

---------------------------------------------------
-- FPS COUNTER NO TOPO
---------------------------------------------------
local fpsLabel = Instance.new("TextLabel")
fpsLabel.Position = UDim2.new(0.5,-60,0,5)
fpsLabel.Size = UDim2.new(0,120,0,30)
fpsLabel.BackgroundTransparency = 1
fpsLabel.TextColor3 = Color3.fromRGB(255,255,255)
fpsLabel.Font = Enum.Font.GothamBold
fpsLabel.TextSize = 18
fpsLabel.TextStrokeTransparency = 0
fpsLabel.Parent = screenGui

local lastTime = tick()
local frameCount = 0
RunService.RenderStepped:Connect(function()
    frameCount += 1
    if tick() - lastTime >= 1 then
        fpsLabel.Text = "FPS: "..frameCount
        frameCount = 0
        lastTime = tick()
    end
end)

---------------------------------------------------
-- ABRIR/FECHAR GUI
---------------------------------------------------
local open = false
openButton.MouseButton1Click:Connect(function()
    open = not open
    if open then
        mainFrame.Visible = true
        mainFrame.BackgroundTransparency = 1
        TweenService:Create(mainFrame,TweenInfo.new(0.4,Enum.EasingStyle.Quint,Enum.EasingDirection.Out),
            {BackgroundTransparency=0.3}):Play()
    else
        TweenService:Create(mainFrame,TweenInfo.new(0.4,Enum.EasingStyle.Quint,Enum.EasingDirection.In),
            {BackgroundTransparency=1}):Play()
        task.wait(0.4)
        mainFrame.Visible = false
    end
end)

---------------------------------------------------
-- BOTÕES
---------------------------------------------------
boostButton.MouseButton1Click:Connect(fpsBoost)
disableButton.MouseButton1Click:Connect(disableFpsBoost)
stretchButton.MouseButton1Click:Connect(toggleStretch)
invisButton.MouseButton1Click:Connect(toggleInvisibility)

---------------------------------------------------
-- ADICIONAR GLOW NEON NOS BOTÕES
---------------------------------------------------
local function addGlow(button, color)
    local glow = Instance.new("UIStroke")
    glow.Color = color
    glow.Thickness = 2
    glow.Transparency = 0.7
    glow.Parent = button
    spawn(function()
        while button.Parent do
            for i = 0.7, 0.4, -0.03 do
                glow.Transparency = i
                task.wait(0.05)
            end
            for i = 0.4, 0.7, 0.03 do
                glow.Transparency = i
                task.wait(0.05)
            end
        end
    end)
end

addGlow(boostButton, Color3.fromRGB(80,255,80))
addGlow(disableButton, Color3.fromRGB(255,80,80))
addGlow(stretchButton, Color3.fromRGB(80,150,255))
addGlow(invisButton, Color3.fromRGB(200,200,255))
addGlow(openButton, Color3.fromRGB(255,255,100))
addGlow(teleportButton, Color3.fromRGB(255,200,80))
addGlow(goButton, Color3.fromRGB(255,255,255))

# Rad-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Variáveis
local headshotEnabled = true
local walkspeedEnabled = false
local walkSpeedValue = 16
local FOV_RADIUS = 30
local headshotTarget = "Head"
local ESP_Enabled = true
local noRecoilEnabled = true

-- --- GUI ---
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TheKingModV1"
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- FOV Frame
local fovFrame = Instance.new("Frame")
fovFrame.Size = UDim2.new(0,FOV_RADIUS*2,0,FOV_RADIUS*2)
fovFrame.Position = UDim2.new(0.5,-FOV_RADIUS,0.5,-FOV_RADIUS)
fovFrame.BorderSizePixel = 2
fovFrame.BorderColor3 = Color3.fromRGB(0,255,0)
fovFrame.BackgroundTransparency = 1
fovFrame.Parent = screenGui
local fovCorner = Instance.new("UICorner", fovFrame)
fovCorner.CornerRadius = UDim.new(1,0)

-- --- HUB ---
local hubFrame = Instance.new("Frame")
hubFrame.Size = UDim2.new(0,400,0,300)
hubFrame.Position = UDim2.new(0.5,-200,0.5,-150)
hubFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
hubFrame.Visible = true
hubFrame.Parent = screenGui

local hubTitle = Instance.new("TextLabel")
hubTitle.Size = UDim2.new(1,0,0,40)
hubTitle.Text = "The King Mod V1"
hubTitle.TextColor3 = Color3.fromRGB(255,255,255)
hubTitle.BackgroundTransparency = 1
hubTitle.Font = Enum.Font.SourceSansBold
hubTitle.TextScaled = true
hubTitle.Parent = hubFrame

-- --- Botão redondo MOD ---
local function criarBotaoRedondo(parent, posX, posY, texto)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,50,0,50)
    btn.Position = UDim2.new(0,posX,0,posY)
    btn.Text = texto
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextScaled = true
    btn.BorderSizePixel = 0
    btn.Parent = parent
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1,0)
    corner.Parent = btn
    return btn
end

local minimizeBtn = criarBotaoRedondo(hubFrame, 350, 10, "MOD")
local reopenBtn = criarBotaoRedondo(screenGui, 10, 10, "MOD")
reopenBtn.Visible = false

minimizeBtn.MouseButton1Click:Connect(function()
    hubFrame.Visible = false
    reopenBtn.Visible = true
end)
reopenBtn.MouseButton1Click:Connect(function()
    hubFrame.Visible = true
    reopenBtn.Visible = false
end)

-- --- Função criar Botões com Emoji ---
local function criarBotaoEmoji(texto, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,150,0,40)
    btn.Position = UDim2.new(0,20,0,posY)
    btn.Text = texto
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextScaled = true
    btn.BorderSizePixel = 0
    btn.Parent = hubFrame
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0.3,0)
    corner.Parent = btn
    return btn
end

local headshotBtn = criarBotaoEmoji("🎯 HEADSHOT: ON", 60)
local recoilBtn = criarBotaoEmoji("🔫 NO RECOIL: ON", 140)
local wsBtn = criarBotaoEmoji("🏃 WALKSPEED: OFF", 180)

-- --- LEDs ---
local function criarLED(parent, posX, posY)
    local led = Instance.new("Frame")
    led.Size = UDim2.new(0,15,0,15)
    led.Position = UDim2.new(0,posX,0,posY)
    led.BackgroundColor3 = Color3.fromRGB(255,0,0)
    led.BorderSizePixel = 0
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1,0)
    corner.Parent = led
    led.Parent = parent
    return led
end

local headshotLED = criarLED(hubFrame, 180, 65)
local recoilLED = criarLED(hubFrame, 180, 145)
local wsLED = criarLED(hubFrame, 180, 185)

-- --- Toggle HS Cabeça / Peito ---
local hsToggleText = Instance.new("TextButton")
hsToggleText.Size = UDim2.new(0,150,0,30)
hsToggleText.Position = UDim2.new(0,20,0,105)
hsToggleText.Text = "HS Cabeça ✅"
hsToggleText.TextColor3 = Color3.fromRGB(255,255,255)
hsToggleText.BackgroundColor3 = Color3.fromRGB(30,30,30)
hsToggleText.Font = Enum.Font.SourceSansBold
hsToggleText.TextScaled = true
hsToggleText.BorderSizePixel = 0
hsToggleText.Parent = hubFrame
local hsCorner = Instance.new("UICorner", hsToggleText)
hsCorner.CornerRadius = UDim.new(0.3,0)

hsToggleText.MouseButton1Click:Connect(function()
    if headshotTarget == "Head" then
        headshotTarget = "Torso"
        hsToggleText.Text = "HS Peito ✅"
    else
        headshotTarget = "Head"
        hsToggleText.Text = "HS Cabeça ✅"
    end
end)

-- --- WalkSpeed ---
local wsBox = Instance.new("TextBox")
wsBox.Size = UDim2.new(0,100,0,30)
wsBox.Position = UDim2.new(0,180,0,180)
wsBox.PlaceholderText = tostring(walkSpeedValue)
wsBox.TextColor3 = Color3.fromRGB(255,255,255)
wsBox.BackgroundColor3 = Color3.fromRGB(35,35,35)
wsBox.Font = Enum.Font.SourceSansBold
wsBox.TextScaled = true
wsBox.ClearTextOnFocus = true
wsBox.Parent = hubFrame

local wsLabel = Instance.new("TextLabel")
wsLabel.Size = UDim2.new(0,150,0,30)
wsLabel.Position = UDim2.new(0,20,0,220)
wsLabel.Text = "Velocidade atual: "..walkSpeedValue
wsLabel.TextColor3 = Color3.fromRGB(255,255,255)
wsLabel.BackgroundTransparency = 1
wsLabel.Font = Enum.Font.SourceSansBold
wsLabel.TextScaled = true
wsLabel.Parent = hubFrame

wsBox.FocusLost:Connect(function()
    local value = tonumber(wsBox.Text)
    if value then
        walkSpeedValue = value
        wsLabel.Text = "Velocidade atual: "..walkSpeedValue
        wsBox.Text = ""
    end
end)

-- --- Botões Toggle ---
wsBtn.MouseButton1Click:Connect(function()
    walkspeedEnabled = not walkspeedEnabled
    wsBtn.Text = "🏃 WALKSPEED: "..(walkspeedEnabled and "ON" or "OFF")
end)

headshotBtn.MouseButton1Click:Connect(function()
    headshotEnabled = not headshotEnabled
    headshotBtn.Text = "🎯 HEADSHOT: "..(headshotEnabled and "ON" or "OFF")
end)

recoilBtn.MouseButton1Click:Connect(function()
    noRecoilEnabled = not noRecoilEnabled
    recoilBtn.Text = "🔫 NO RECOIL: "..(noRecoilEnabled and "ON" or "OFF")
end)

-- --- ESP ---
local function criarESP(player)
    if not player.Character then return end
    local root = player.Character:FindFirstChild("HumanoidRootPart")
    if not root then return end
    if root:FindFirstChild("ESPBox") then return end
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Name = "ESPBox"
    espBox.Adornee = root
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 2
    espBox.Size = Vector3.new(4,6,1)
    espBox.Color = Color3.fromRGB(255,255,255)
    espBox.Transparency = 0.7
    espBox.Parent = root
end

local function atualizarESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            criarESP(player)
        end
    end
end

-- --- Loop principal ---
RunService.RenderStepped:Connect(function()
    -- Centraliza FOV
    fovFrame.Position = UDim2.new(0.5,-fovFrame.Size.X.Offset/2,0.5,-fovFrame.Size.Y.Offset/2)

    -- LEDs
    headshotLED.BackgroundColor3 = headshotEnabled and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)
    wsLED.BackgroundColor3 = walkspeedEnabled and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)
    recoilLED.BackgroundColor3 = noRecoilEnabled and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)

    -- WalkSpeed
    if walkspeedEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = walkSpeedValue
    end

    -- Headshot
    if headshotEnabled then
        local closest
        local minDist = FOV_RADIUS
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local part = player.Character:FindFirstChild(headshotTarget) or player.Character:FindFirstChild("Head")
                if part then
                    local screenPos, onScreen = Camera:WorldToViewportPoint(part.Position)
                    if onScreen then
                        local dist = (Vector2.new(screenPos.X,screenPos.Y) - Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)).Magnitude
                        if dist <= minDist then
                            minDist = dist
                            closest = part
                        end
                    end
                end
            end
        end
        if closest then
            pcall(function()
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, closest.Position)
            end)
        end
    end

    -- ESP
    if ESP_Enabled then
        atualizarESP()
    end

    -- No Recoil
    if noRecoilEnabled and LocalPlayer.Character then
        pcall(function()
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, Camera.CFrame.Position + Camera.CFrame.LookVector)
        end)
    end
end)

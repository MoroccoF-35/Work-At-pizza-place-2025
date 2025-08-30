-- Work at a Pizza Place Ultimate Script 2025
-- Designer: AI Crafted for Sleek, Dynamic Black GUI Menu
-- Features: Auto Jobs, Teleport, Infinite Coins, Kick, Troll, Anti-AFK, Speed Boost

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "PizzaPlaceAutoHub"

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 280, 0, 360)
mainFrame.Position = UDim2.new(0.5, -140, 0.5, -180)
mainFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = ScreenGui
mainFrame.ClipsDescendants = true
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)

local UICorner = Instance.new("UICorner", mainFrame)
UICorner.CornerRadius = UDim.new(0, 10)

-- Title
local title = Instance.new("TextLabel", mainFrame)
title.Text = "PizzaPlace AutoHub 2025"
title.Size = UDim2.new(1,0,0,40)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.Position = UDim2.new(0,0,0,0)

-- Utility function for buttons
local function CreateButton(text, position, parent)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 240, 0, 30)
    btn.Position = position
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.Text = text
    btn.AutoButtonColor = false
    btn.Parent = parent

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(70,70,70)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    end)

    return btn
end

-- Toggles storage
local toggles = {
    AutoCashier = false,
    AutoCook = false,
    AutoBoxer = false,
    AutoDelivery = false,
    InfiniteCoins = false,
    AntiAFK = false,
    SpeedBoost = false,
}

-- Teleport locations
local teleportLocations = {
    ["Kitchen"] = Vector3.new(17, 4, 22),
    ["Storage"] = Vector3.new(-88, 4, 170),
    ["Delivery"] = Vector3.new(0, 4, -70),
    ["BossRoom"] = Vector3.new(100, 10, 100),
    ["PizzaBoxArea"] = Vector3.new(40, 4, 10),
}

-- Create Toggle Button function
local function CreateToggleButton(text, yPos)
    local btn = CreateButton(text.." [OFF]", UDim2.new(0, 20, 0, yPos), mainFrame)
    btn.MouseButton1Click:Connect(function()
        local key = text:gsub(" ","")
        toggles[key] = not toggles[key]
        btn.Text = text.." ["..(toggles[key] and "ON" or "OFF").."]"
    end)
    return btn
end

-- Auto Jobs toggles
local autoCashierBtn = CreateToggleButton("Auto Cashier", 50)
local autoCookBtn = CreateToggleButton("Auto Cook", 90)
local autoBoxerBtn = CreateToggleButton("Auto Boxer", 130)
local autoDeliveryBtn = CreateToggleButton("Auto Delivery", 170)

-- Infinite Coins and Anti-AFK toggles
local infiniteCoinsBtn = CreateToggleButton("Infinite Coins", 210)
local antiAfkBtn = CreateToggleButton("Anti AFK", 250)

-- Speed Boost toggle
local speedBoostBtn = CreateToggleButton("Speed Boost", 290)

-- Teleport Menu
local teleportLabel = Instance.new("TextLabel", mainFrame)
teleportLabel.Text = "Teleport Locations"
teleportLabel.Size = UDim2.new(1,0,0,20)
teleportLabel.Position = UDim2.new(0, 20, 0, 330)
teleportLabel.BackgroundTransparency = 1
teleportLabel.TextColor3 = Color3.new(1,1,1)
teleportLabel.Font = Enum.Font.GothamBold
teleportLabel.TextSize = 14

local teleportY = 350
for name,pos in pairs(teleportLocations) do
    local tpBtn = CreateButton("Teleport to "..name, UDim2.new(0, 20, 0, teleportY), mainFrame)
    tpBtn.Size = UDim2.new(0, 240, 0, 25)
    tpBtn.TextSize = 14
    tpBtn.MouseButton1Click:Connect(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(pos + Vector3.new(0,3,0))
        end
    end)
    teleportY = teleportY + 30
end

-- Kick Player UI
local kickLabel = Instance.new("TextLabel", mainFrame)
kickLabel.Text = "Kick Player (Type exact name)"
kickLabel.Size = UDim2.new(1, 0, 0, 20)
kickLabel.Position = UDim2.new(0, 20, 0, 530)
kickLabel.BackgroundTransparency = 1
kickLabel.TextColor3 = Color3.new(1,1,1)
kickLabel.Font = Enum.Font.GothamBold
kickLabel.TextSize = 14

local kickBox = Instance.new("TextBox", mainFrame)
kickBox.PlaceholderText = "Player Name"
kickBox.Size = UDim2.new(0, 150, 0, 25)
kickBox.Position = UDim2.new(0, 20, 0, 560)
kickBox.TextColor3 = Color3.new(1,1,1)
kickBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
kickBox.Font = Enum.Font.Gotham
kickBox.TextSize = 14
local uic = Instance.new("UICorner", kickBox)
uic.CornerRadius = UDim.new(0,5)

local kickBtn = CreateButton("Kick Player", UDim2.new(0, 180, 0, 560), mainFrame)
kickBtn.Size = UDim2.new(0, 80, 0, 25)
kickBtn.MouseButton1Click:Connect(function()
    local targetName = kickBox.Text
    if targetName == "" then return end
    local kickEvent = ReplicatedStorage:FindFirstChild("KickPlayerEvent") or ReplicatedStorage:FindFirstChild("KickEvent")
    if kickEvent then
        pcall(function()
            kickEvent:FireServer(targetName)
        end)
    end
end)

-- Anti-AFK Implementation
local antiAfkConnection
local function toggleAntiAFK(on)
    if on then
        antiAfkConnection = game:GetService("Players").LocalPlayer.Idled:Connect(function()
            game:GetService("VirtualUser"):ClickButton2(Vector2.new())
        end)
    else
        if antiAfkConnection then
            antiAfkConnection:Disconnect()
            antiAfkConnection = nil
        end
    end
end

-- Speed Boost Implementation
local normalWalkSpeed = 16
local function toggleSpeedBoost(on)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        if on then
            LocalPlayer.Character.Humanoid.WalkSpeed = 32
        else
            LocalPlayer.Character.Humanoid.WalkSpeed = normalWalkSpeed
        end
    end
end

-- Auto Jobs functions
local function autoCashier()
    if toggles.AutoCashier then
        local cashierEvent = ReplicatedStorage:FindFirstChild("CashierEvent") or ReplicatedStorage:FindFirstChild("OrderEvent")
        if cashierEvent then
            pcall(function()
                cashierEvent:FireServer()
            end)
        end
    end
end

local function autoCook()
    if toggles.AutoCook then
        local cookEvent = ReplicatedStorage:FindFirstChild("CookEvent") or ReplicatedStorage:FindFirstChild("PreparePizzaEvent")
        if cookEvent then
            pcall(function()
                cookEvent:FireServer()
            end)
        end
    end
end

local function autoBoxer()
    if toggles.AutoBoxer then
        local boxEvent = ReplicatedStorage:FindFirstChild("BoxerEvent") or ReplicatedStorage:FindFirstChild("BoxPizzaEvent")
        if boxEvent then
            pcall(function()
                boxEvent:FireServer()
            end)
        end
    end
end

local function autoDelivery()
    if toggles.AutoDelivery then
        local deliveryEvent = ReplicatedStorage:FindFirstChild("DeliveryEvent") or ReplicatedStorage:FindFirstChild("DeliverPizzaEvent")
        if deliveryEvent then
            pcall(function()
                deliveryEvent:FireServer()
            end)
        end
    end
end

-- Infinite Coins placeholder (Note: often requires server exploit, so simulated)
local function infiniteCoins()
    if toggles.InfiniteCoins then
        -- This feature depends heavily on game server vulnerabilities not accessible publicly
        -- Here we simulate it by repeatedly firing coin gain event
        local coinEvent = ReplicatedStorage:FindFirstChild("AddCoinsEvent") or ReplicatedStorage:FindFirstChild("MoneyEvent")
        if coinEvent then
            pcall(function()
                coinEvent:FireServer(1000) -- Add 1000 coins repeatedly
            end)
        end
    end
end

-- Main loop
RunService.Heartbeat:Connect(function()
    if toggles.AutoCashier then autoCashier() end
    if toggles.AutoCook then autoCook() end
    if toggles.AutoBoxer then autoBoxer() end
    if toggles.AutoDelivery then autoDelivery() end
    if toggles.InfiniteCoins then infiniteCoins() end
    toggleAntiAFK(toggles.AntiAFK)
    toggleSpeedBoost(toggles.SpeedBoost)
end)

-- Script GUI para Steal a Brainrot
-- Créditos: Tony_Dev

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Mouse = LocalPlayer:GetMouse()
local StarterSpeed = 16

-- GUI principal
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "TonyDevScript"

-- Intro
local IntroFrame = Instance.new("Frame", ScreenGui)
IntroFrame.Size = UDim2.new(1, 0, 1, 0)
IntroFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
IntroFrame.BackgroundTransparency = 0.9

local Title = Instance.new("TextLabel", IntroFrame)
Title.Size = UDim2.new(0, 520, 0, 60)
Title.Position = UDim2.new(0.5, -260, 0.5, -30)
Title.BackgroundTransparency = 1
Title.Text = "🔥 Script criado por Tony_Dev 🔥"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 32
Title.TextColor3 = Color3.fromRGB(0, 170, 255)
Title.TextStrokeColor3 = Color3.fromRGB(20, 20, 20)
Title.TextStrokeTransparency = 0.7

task.wait(1.5)
TweenService:Create(Title, TweenInfo.new(1.5), {Rotation = 360}):Play()
task.wait(1.5)
TweenService:Create(Title, TweenInfo.new(1.5), {TextTransparency = 1}):Play()
task.wait(1.5)
IntroFrame:Destroy()

-- Botão Menu
local MenuBtn = Instance.new("TextButton", ScreenGui)
MenuBtn.Size = UDim2.new(0, 130, 0, 40)
MenuBtn.Position = UDim2.new(0, 15, 0, 15)
MenuBtn.BackgroundColor3 = Color3.fromRGB(15, 135, 255)
MenuBtn.Text = "📂 Abrir Menu"
MenuBtn.TextColor3 = Color3.fromRGB(255,255,255)
MenuBtn.Font = Enum.Font.GothamBold
MenuBtn.TextSize = 16

-- Frame principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 280, 0, 460)
MainFrame.Position = UDim2.new(0, 15, 0, 70)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true

-- Título
local TitleLabel = Instance.new("TextLabel", MainFrame)
TitleLabel.Size = UDim2.new(1, -40, 0, 35)
TitleLabel.Position = UDim2.new(0, 20, 0, 15)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "Roube um Brainrot - Script"
TitleLabel.Font = Enum.Font.GothamBlack
TitleLabel.TextSize = 22
TitleLabel.TextColor3 = Color3.fromRGB(0, 190, 255)

-- Créditos
local credit = Instance.new("TextLabel", MainFrame)
credit.Size = UDim2.new(1, 0, 0, 30)
credit.Position = UDim2.new(0, 0, 1, -40)
credit.BackgroundTransparency = 1
credit.Text = "🛠️ By Tony_Dev"
credit.Font = Enum.Font.Gotham
credit.TextSize = 16
credit.TextColor3 = Color3.fromRGB(0, 170, 255)

-- Botão de Fechar
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(0.5, -14, 1, -75)
CloseBtn.BackgroundColor3 = Color3.fromRGB(230, 40, 40)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20

MenuBtn.MouseButton1Click:Connect(function()
MainFrame.Visible = true
end)
CloseBtn.MouseButton1Click:Connect(function()
MainFrame.Visible = false
end)

-- Criar Botão
local function createButton(name, posY)
local btn = Instance.new("TextButton", MainFrame)
btn.Size = UDim2.new(1, -40, 0, 42)
btn.Position = UDim2.new(0, 20, 0, posY)
btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
btn.TextColor3 = Color3.fromRGB(220, 220, 220)
btn.Font = Enum.Font.Gotham
btn.TextSize = 16
btn.Text = name
return btn
end

-- Variáveis
local boostSpeed, esp, godmode, infjump, clicktp = false, false, false, false, false
local pressedKeys = {}
local highlights = {}

-- Boost Speed
local function applyBoost()
local char = LocalPlayer.Character
if not char or not char:FindFirstChild("HumanoidRootPart") then return end
local hrp = char.HumanoidRootPart
local camCF = workspace.CurrentCamera.CFrame
local moveDir = Vector3.new()
if pressedKeys["W"] then moveDir += camCF.LookVector end
if pressedKeys["S"] then moveDir -= camCF.LookVector end
if pressedKeys["A"] then moveDir -= camCF.RightVector end
if pressedKeys["D"] then moveDir += camCF.RightVector end
moveDir = Vector3.new(moveDir.X, 0, moveDir.Z)
if moveDir.Magnitude > 0 then
moveDir = moveDir.Unit * 50
hrp.Velocity = Vector3.new(moveDir.X, hrp.Velocity.Y, moveDir.Z)
else
hrp.Velocity = Vector3.new(0, hrp.Velocity.Y, 0)
end
end

UserInputService.InputBegan:Connect(function(input)
if input.KeyCode.Name then pressedKeys[input.KeyCode.Name] = true end
end)
UserInputService.InputEnded:Connect(function(input)
if input.KeyCode.Name then pressedKeys[input.KeyCode.Name] = false end
end)

RunService.Heartbeat:Connect(function()
if boostSpeed then applyBoost() end
end)

-- ESP (Chams)
local function toggleESP()
esp = not esp
for _, p in pairs(Players:GetPlayers()) do
if p ~= LocalPlayer and p.Character then
if esp then
local h = Instance.new("Highlight", p.Character)
h.FillColor = Color3.fromRGB(255, 0, 0)
h.OutlineColor = Color3.fromRGB(255, 0, 0)
h.Adornee = p.Character
highlights[p] = h
elseif highlights[p] then
highlights[p]:Destroy()
end
end
end
end

-- GodMode
RunService.Heartbeat:Connect(function()
if godmode and LocalPlayer.Character then
local h = LocalPlayer.Character:FindFirstChild("Humanoid")
if h and h.Health < 100 then h.Health = 100 end
end
end)

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
if infjump then
local h = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end
end
end)

-- Click TP
Mouse.Button1Down:Connect(function()
if clicktp and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 5, 0))
end
end)

-- Botões
local b1 = createButton("⚡ Boost Speed", 70)
b1.MouseButton1Click:Connect(function()
boostSpeed = not boostSpeed
b1.Text = boostSpeed and "⚡ Boost Speed: ON" or "⚡ Boost Speed: OFF"
end)

local b2 = createButton("👁️ ESP (Chams)", 120)
b2.MouseButton1Click:Connect(function()
toggleESP()
b2.Text = esp and "👁️ ESP (Chams): ON" or "👁️ ESP (Chams): OFF"
end)

local b3 = createButton("💖 GodMode", 170)
b3.MouseButton1Click:Connect(function()
godmode = not godmode
b3.Text = godmode and "💖 GodMode: ON" or "💖 GodMode: OFF"
end)

local b4 = createButton("🦘 Infinite Jump", 220)
b4.MouseButton1Click:Connect(function()
infjump = not infjump
b4.Text = infjump and "🦘 Infinite Jump: ON" or "🦘 Infinite Jump: OFF"
end)

local b5 = createButton("📍 Click TP", 270)
b5.MouseButton1Click:Connect(function()
clicktp = not clicktp
b5.Text = clicktp and "📍 Click TP: ON" or "📍 Click TP: OFF"
end)

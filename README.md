-- Blox Fruits GUI Funcional Delta Executor
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Criar ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BloxFruitsGUI"
ScreenGui.Parent = PlayerGui

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 350, 0, 400)
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1,0,0,50)
Title.Position = UDim2.new(0,0,0,0)
Title.BackgroundColor3 = Color3.fromRGB(50,50,50)
Title.Text = "Blox Fruits GUI"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.Parent = MainFrame

-- Status
local Status = Instance.new("TextLabel")
Status.Size = UDim2.new(1,0,0,30)
Status.Position = UDim2.new(0,0,0,370)
Status.BackgroundColor3 = Color3.fromRGB(40,40,40)
Status.TextColor3 = Color3.fromRGB(255,255,255)
Status.Text = "Status: Inativo"
Status.Font = Enum.Font.Gotham
Status.TextSize = 16
Status.Parent = MainFrame

-- Função para criar toggles
local function createToggle(text, position, callback)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0,200,0,40)
    label.Position = position
    label.BackgroundColor3 = Color3.fromRGB(60,60,60)
    label.Text = text
    label.TextColor3 = Color3.fromRGB(255,255,255)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 16
    label.Parent = MainFrame

    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.new(0,100,0,40)
    toggle.Position = UDim2.new(0,200,0,position.Y.Offset)
    toggle.BackgroundColor3 = Color3.fromRGB(150,0,0)
    toggle.Text = "OFF"
    toggle.TextColor3 = Color3.fromRGB(255,255,255)
    toggle.Font = Enum.Font.GothamBold
    toggle.TextSize = 16
    toggle.Parent = MainFrame

    local state = false
    toggle.MouseButton1Click:Connect(function()
        state = not state
        toggle.Text = state and "ON" or "OFF"
        toggle.BackgroundColor3 = state and Color3.fromRGB(0,150,0) or Color3.fromRGB(150,0,0)
        callback(state)
    end)
end

-- Variáveis globais
local AutoFarm = false

-- Função Auto Farm simples (exemplo genérico)
local function autoFarm(state)
    AutoFarm = state
    Status.Text = state and "Status: Auto Farm Ativo" or "Status: Inativo"
    if AutoFarm then
        spawn(function()
            while AutoFarm do
                wait(0.5)
                for _, npc in pairs(Workspace:GetChildren()) do
                    if npc:FindFirstChild("HumanoidRootPart") then
                        Player.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame + Vector3.new(0,5,0)
                        -- Exemplo de ataque simples (substitua pelo RemoteEvent correto)
                        if ReplicatedStorage:FindFirstChild("RemoteEvents") then
                            local remotes = ReplicatedStorage.RemoteEvents
                            if remotes:FindFirstChild("UseSkill") then
                                remotes.UseSkill:FireServer("SkillName")
                            end
                        end
                        wait(0.3)
                    end
                end
            end
        end)
    end
end

-- Criar Toggle Auto Farm
createToggle("Auto Farm NPC", UDim2.new(0,20,0,70), autoFarm)

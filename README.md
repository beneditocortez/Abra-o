local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local LP = Players.LocalPlayer
local Char = LP.Character or LP.CharacterAdded:Wait()
local Humanoid = Char:WaitForChild("Humanoid")
local hugging = false
local animTrack = nil

-- GUI do botão
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "HugUI"

local botao = Instance.new("TextButton", gui)
botao.Size = UDim2.new(0, 150, 0, 50)
botao.Position = UDim2.new(0.5, -75, 0.85, 0)
botao.BackgroundColor3 = Color3.fromRGB(255, 200, 200)
botao.Text = "🤗 Abraçar"
botao.TextScaled = true
botao.Font = Enum.Font.GothamBold
botao.TextColor3 = Color3.new(0, 0, 0)
botao.Draggable = true

-- Animação de abraço (R15)
local anim = Instance.new("Animation")
anim.AnimationId = "rbxassetid://507771019" -- Hug anim (funciona em R15)

-- Função: pegar player mais próximo
local function getClosestPlayer()
	local closestDist = math.huge
	local closestPlr = nil
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LP and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local dist = (Char.HumanoidRootPart.Position - plr.Character.HumanoidRootPart.Position).Magnitude
			if dist < closestDist and dist <= 10 then
				closestDist = dist
				closestPlr = plr
			end
		end
	end
	return closestPlr
end

-- Ação do botão
botao.MouseButton1Click:Connect(function()
	if not Char or not Humanoid then return end
	if hugging then
		hugging = false
		if animTrack then animTrack:Stop() end
		botao.Text = "🤗 Abraçar"
	else
		local target = getClosestPlayer()
		if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
			Char.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
			animTrack = Humanoid:LoadAnimation(anim)
			animTrack:Play()
			botao.Text = "💔 Desabraçar"
			hugging = true
		end
	end
end)

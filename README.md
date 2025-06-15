-- LocalScript (coloque em StarterPlayerScripts ou StarterGui)

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local player = Players.LocalPlayer
local hugging = false
local animTrack = nil
local nearestPlayer = nil

-- Cria botão na tela
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "HugUI"

local hugButton = Instance.new("TextButton", screenGui)
hugButton.Size = UDim2.new(0, 150, 0, 50)
hugButton.Position = UDim2.new(0.5, -75, 0.9, 0)
hugButton.Text = "🤗 Abraçar"
hugButton.BackgroundColor3 = Color3.fromRGB(255, 200, 200)
hugButton.TextScaled = true
hugButton.Font = Enum.Font.GothamBold
hugButton.TextColor3 = Color3.new(0, 0, 0)
hugButton.BorderSizePixel = 0
hugButton.Active = true
hugButton.Draggable = true

-- Animação de abraço
local hugAnim = Instance.new("Animation")
hugAnim.AnimationId = "rbxassetid://507771019" -- pode trocar por outra se quiser

-- Função para encontrar jogador mais próximo
local function getNearestPlayer()
	local closestDist = math.huge
	local closestPlayer = nil
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local dist = (plr.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
			if dist < closestDist and dist < 10 then -- até 10 studs de distância
				closestDist = dist
				closestPlayer = plr
			end
		end
	end
	return closestPlayer
end

-- Ação do botão
hugButton.MouseButton1Click:Connect(function()
	if not player.Character or not player.Character:FindFirstChild("Humanoid") then return end
	local humanoid = player.Character.Humanoid

	if hugging then
		-- Desabraçar
		if animTrack then animTrack:Stop() end
		hugButton.Text = "🤗 Abraçar"
		hugging = false
	else
		nearestPlayer = getNearestPlayer()
		if nearestPlayer and nearestPlayer.Character then
			local hrp = player.Character:WaitForChild("HumanoidRootPart")
			local targetHRP = nearestPlayer.Character:FindFirstChild("HumanoidRootPart")
			if targetHRP then
				-- Mover perto
				hrp.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 2)
				-- Tocar animação
				animTrack = humanoid:LoadAnimation(hugAnim)
				animTrack:Play()
				hugButton.Text = "💔 Desabraçar"
				hugging = true
			end
		end
	end
end)
# Abra-o

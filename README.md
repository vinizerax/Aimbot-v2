# Aimbot-v2

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local Camera = Workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- Config
local aimbotEnabled = true

-- Função para encontrar o inimigo mais próximo (mesmo atrás de você)
local function GetClosestEnemyHead()
	local closestDistance = math.huge
	local closestHead = nil

	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team then
			local character = player.Character
			if character and character:FindFirstChild("Head") and character:FindFirstChild("Humanoid") and character.Humanoid.Health > 0 then
				local head = character.Head
				local distance = (Camera.CFrame.Position - head.Position).Magnitude
				if distance < closestDistance then
					closestDistance = distance
					closestHead = head
				end
			end
		end
	end

	return closestHead
end

-- Aimbot loop
RunService.RenderStepped:Connect(function()
	if aimbotEnabled then
		local targetHead = GetClosestEnemyHead()
		if targetHead then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, targetHead.Position)
		end
	end
end)

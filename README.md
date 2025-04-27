-- LocalScript (StarterPlayerScripts, por exemplo)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local localPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera

local MAX_DISTANCE = 100

local function getClosestTarget()
	local closest = nil
	local shortestDistance = MAX_DISTANCE

	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= localPlayer and player.Character and player.Character:FindFirstChild("Head") then
			local head = player.Character.Head
			local distance = (head.Position - camera.CFrame.Position).Magnitude

			if distance < shortestDistance then
				shortestDistance = distance
				closest = head
			end
		end
	end

	return closest
end

local function aimAtTarget(target)
	if target then
		local direction = (target.Position - camera.CFrame.Position).Unit
		local newCFrame = CFrame.new(camera.CFrame.Position, camera.CFrame.Position + direction)
		camera.CFrame = newCFrame
	end
end

RunService.RenderStepped:Connect(function()
	local target = getClosestTarget()
	aimAtTarget(target)
end)

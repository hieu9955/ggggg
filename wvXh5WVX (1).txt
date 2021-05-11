--Define basics
local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()
local UIS = game:GetService("UserInputService")
local char = player.Character
local playerGui = player.PlayerGui
--Variables
local uiOpened = true
local walkSpeed = 16
local changingWalkSpeed = false
local jumpPower = 50
local changingJumpPower = false
local findPlayers = false
local findRokas = false
local findArrows = false
local findMasks = false
local noclip = false
--MakeUI functions
local g = Instance.new("ScreenGui")
g.ResetOnSpawn = false
g.Name = tostring(math.random(1.0005, 99999.9999))
g.Parent = playerGui
g.DisplayOrder = 99
makeBaseDragUI = function(size, pos, topName)
	local origSize = size
	local frame = Instance.new("Frame", g)
	frame.BorderSizePixel = 0
	frame.BackgroundTransparency = .3
	frame.BackgroundColor3 = Color3.new(0, 0, 0)
	frame.Size = size
	frame.Position = pos
	local tn = Instance.new("TextButton", frame)
	tn.Name = "tn"
	tn.Size = UDim2.new(.9, 0, .15, 0)
	tn.Text = topName
	tn.TextScaled = true
	tn.TextStrokeTransparency = 0
	tn.BackgroundTransparency = .9
	tn.TextColor3 = BrickColor.new("Royal purple").Color
	tn.Font = Enum.Font.Fantasy
	local ty = Instance.new("TextButton", frame)
	ty.Name = "tx"
	ty.Size = UDim2.new(.1, 0, .15, 0)
	ty.Position = UDim2.new(.9, 0, 0, 0)
	ty.Text = "+"
	ty.TextScaled = true
	ty.TextStrokeTransparency = 0
	ty.BackgroundTransparency = .9
	ty.TextColor3 = BrickColor.new("Forest green").Color
	ty.Font = Enum.Font.Fantasy
	local mainFrame = Instance.new("Frame", frame)
	mainFrame.BorderSizePixel = 0
	mainFrame.Visible = false
	mainFrame.BackgroundTransparency = .9
	mainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
	mainFrame.Size = UDim2.new(1, 0, .85, 0)
	mainFrame.Position = UDim2.new(0, 0, .15, 0)
	local minimise = false
	local dragging = false
	mouse.Move:connect(function()
		if dragging == true then
			frame.Position =  UDim2.new(-frame.Size.X.Scale/2, mouse.X, -frame.Size.X.Scale*.05, mouse.Y)
		end
	end)
	tn.MouseButton1Down:connect(function()
		dragging = true
	end)
	tn.MouseButton1Up:connect(function()
		dragging = false
	end)
	ty.MouseButton1Click:connect(function()
		if minimise == false then
			frame:TweenSize(UDim2.new(frame.Size.X.Scale, 0, .05, 0), "Out", "Linear", .4, true)
			tn:TweenSize(UDim2.new(.9, 0, 1, 0), "Out", "Linear", .4, true)
			ty:TweenSize(UDim2.new(.1, 0, 1, 0), "Out", "Linear", .4, true)
			mainFrame.Visible = false
			minimise = true
			ty.Text = "+"
			ty.TextColor3 = BrickColor.new("Forest green").Color
		else
			frame:TweenSize(origSize, "Out", "Linear", .4, true)
			tn:TweenSize(UDim2.new(.9, 0, .15, 0), "Out", "Linear", .4, true)
			ty:TweenSize(UDim2.new(.1, 0, .15, 0), "Out", "Linear", .4, true)
			mainFrame.Visible = true
			minimise = false
			ty.Text = "_"
			ty.TextColor3 = BrickColor.new("Brick yellow").Color
		end
	end)
	frame:TweenSize(UDim2.new(frame.Size.X.Scale, 0, .05, 0), "Out", "Linear", .4, true)
	tn:TweenSize(UDim2.new(.9, 0, 1, 0), "Out", "Linear", .4, true)
	ty:TweenSize(UDim2.new(.1, 0, 1, 0), "Out", "Linear", .4, true)
	minimise = true
	
	return mainFrame
end
makeSlider = function(parent, size, pos)
	local f = makeFrame(parent, size, pos, 8)
	f.Name = "outerFrame"
	local innerFrame = makeFrame(f, UDim2.new(1, 0, .1, 0), UDim2.new(0, 0, .5, 0), .4)
	innerFrame.Name = "innerFrame"
	local drag = makeButton(innerFrame, UDim2.new(.05, 0, 5, 0), UDim2.new(f.Position.X.Scale+ .1, 0, -2.5, 0), "", Color3.new(0, 0, 0), BrickColor.new("Royal purple").Color, 0)
	drag.Name = "dragFrame"
	return drag
end
makeButton = function(parent, size, pos, text, tcolor, bgColor, bgTransparency)
	local button = Instance.new("TextButton", parent)
	button.Size = size
	button.BackgroundTransparency = bgTransparency
	button.BorderSizePixel = 0
	button.Text = text
	button.TextStrokeTransparency = 0
	button.TextColor3 = tcolor
	button.Position = pos
	button.Font = Enum.Font.Fantasy
	button.BackgroundColor3 = bgColor
	button.TextScaled = true
	return button
end
makeLabel = function(parent, size, pos, text, tcolor, bgColor, bgTransparency)
	local label = Instance.new("TextLabel", parent)
	label.Size = size
	label.BackgroundTransparency = bgTransparency
	label.BorderSizePixel = 0
	label.Text = text
	label.TextStrokeTransparency = 0
	label.TextColor3 = tcolor
	label.Position = pos
	label.Font = Enum.Font.Fantasy
	label.BackgroundColor3 = bgColor
	label.TextScaled = true
	return label
end
makeFrame = function(parent, size, pos, bgTransparency)
	local frame = Instance.new("Frame", parent)
	frame.Size = size
	frame.BorderSizePixel = 0
	frame.BackgroundTransparency = bgTransparency
	frame.Position = pos
	frame.BackgroundColor3 = Color3.new(0, 0, 0)
	return frame
end
--Functions
--WALKSPEED FUNCTION
local walkSpeed = makeBaseDragUI(UDim2.new(.2, 0, .4, 0), UDim2.new(.65, 0, .25, 0), "Walkspeed")
walkSpeedSlider = makeSlider(walkSpeed, UDim2.new(1, 0, .2, 0), UDim2.new(0, 0, .4, 0))
local walkSpeedLabel = makeLabel(walkSpeed, UDim2.new(1, 0, .2, 0), UDim2.new(0, 0, .2, 0), "Walkspeed: 16", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .8)
local dragging = false
local pos = mouse.X
walkSpeedSlider.MouseButton1Down:connect(function()
	dragging = true
	pos = mouse.X
end)
walkSpeedSlider.MouseButton1Up:connect(function()
	dragging = false
end)
local walkspeed = 16
mouse.Move:connect(function()
	if dragging == true then
		local f = walkSpeedSlider.Parent.Parent
		local newPos = mouse.X
		local diff = newPos - pos
		pos = mouse.X
		local max = mouse.ViewSizeX
		local scaleDiff = (diff/max)
		local leftBarrier = f.Position.X.Scale--(f.Size.X.Scale/2)
		local rightBarrier = f.Position.X.Scale+(f.Size.X.Scale*.95)
		local dragPos = walkSpeedSlider.Position.X.Scale + (scaleDiff*5)
		if(dragPos > leftBarrier and dragPos < rightBarrier) then
			walkSpeedSlider.Position = UDim2.new(dragPos, 0, walkSpeedSlider.Position.Y.Scale, 0)
		else
			dragging = false
		end
		local finalPos = (-(f.Size.X.Scale/2)+walkSpeedSlider.Position.X.Scale)*2
		changingWalkSpeed = true
		walkSpeed = math.floor(116 + (finalPos*100))
		walkSpeedLabel.Text = "Walkspeed: "..tostring(walkSpeed)
	end
end)
--JUMPPOWER FUNCTION
local jumpPowerF = makeBaseDragUI(UDim2.new(.2, 0, .4, 0), UDim2.new(.65, 0, .3, 0), "Jump Power")
jumpPowerSlider = makeSlider(jumpPowerF, UDim2.new(1, 0, .2, 0), UDim2.new(0, 0, .4, 0))
local jumpPowerLabel = makeLabel(jumpPowerF, UDim2.new(1, 0, .2, 0), UDim2.new(0, 0, .2, 0), "Jump Power: 16", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .8)
local dragging = false
local pos = mouse.X
local jumpPower = 50
jumpPowerSlider.MouseButton1Down:connect(function()
	dragging = true
	pos = mouse.X
end)
jumpPowerSlider.MouseButton1Up:connect(function()
	dragging = false
end)
mouse.Move:connect(function()
	if dragging == true then
		local f = jumpPowerSlider.Parent.Parent
		local newPos = mouse.X
		local diff = newPos - pos
		pos = mouse.X
		local max = mouse.ViewSizeX
		local scaleDiff = (diff/max)
		local leftBarrier = f.Position.X.Scale--(f.Size.X.Scale/2)
		local rightBarrier = f.Position.X.Scale+(f.Size.X.Scale*.95)
		local dragPos = jumpPowerSlider.Position.X.Scale + (scaleDiff*5)
		if(dragPos > leftBarrier and dragPos < rightBarrier) then
			jumpPowerSlider.Position = UDim2.new(dragPos, 0, jumpPowerSlider.Position.Y.Scale, 0)
		else
			dragging = false
		end
		local finalPos = (-(f.Size.X.Scale/2)+jumpPowerSlider.Position.X.Scale)*2
		changingJumpPower = true
		jumpPower = math.floor(150 + (finalPos*100))
		jumpPowerLabel.Text = "Jump Power: "..tostring(jumpPower)
	end
end)
--ESP FUNCTION
local Esp = makeBaseDragUI(UDim2.new(.2, 0, .45, 0), UDim2.new(.65, 0, .35, 0), "Player&Item ESP")
local targetLabels = {}
local itemLabels = {}
detectRigType = function(target)
	if target == nil then
		return "Abort"
	elseif target:FindFirstChild("Torso") then 
		return "R6"
	elseif target:FindFirstChild("LowerTorso") then 
		return "R15"
	else
		return "Abort" -- Probably loading in or something
	end
end
detectHumanoid = function(target)
	for i,v in pairs(target:GetChildren()) do
		if v.Name == "Health" then
			return v -- Found the name of our humanoid
		end
	end
end
detectTeam = function(player)
	if player.Neutral == false then
		return player.TeamColor
	end
end
itemLabel = function(meshPart)
	coroutine.resume(coroutine.create(function()
		local bb = Instance.new("BillboardGui", meshPart)
		bb.AlwaysOnTop = true
		bb.Size = UDim2.new(5, 0, 5, 0)
		local frL = Instance.new("Frame", bb)
		frL.BorderSizePixel = 0
		frL.Size = UDim2.new(.05, 0, 1.05, 0)
		frL.BackgroundColor3 = BrickColor.new("Lime green").Color
		local frR = Instance.new("Frame", bb)
		frR.BorderSizePixel = 0
		frR.Size = UDim2.new(.05, 0, 1.05, 0)
		frR.Position = UDim2.new(.99, 0, 0, 0)
		frR.BackgroundColor3 = BrickColor.new("Lime green").Color
		local frT = Instance.new("Frame", bb)
		frT.BorderSizePixel = 0
		frT.Size = UDim2.new(1, 0, .05, 0)
		frT.Position = UDim2.new(0, 0, 0, 0)
		frT.BackgroundColor3 = BrickColor.new("Lime green").Color
		local frB = Instance.new("Frame", bb)
		frB.BorderSizePixel = 0
		frB.Size = UDim2.new(1, 0, .05, 0)
		frB.Position = UDim2.new(0, 0, 0.99, 0)
		frB.BackgroundColor3 = BrickColor.new("Lime green").Color
		table.insert(itemLabels, bb)
		if meshPart.MeshId == "rbxassetid://3497428510" then
			frL.BackgroundColor3 = BrickColor.new("Neon orange").Color
			frR.BackgroundColor3 = BrickColor.new("Neon orange").Color
			frT.BackgroundColor3 = BrickColor.new("Neon orange").Color
			frB.BackgroundColor3 = BrickColor.new("Neon orange").Color
		elseif meshPart.MeshId == "rbxassetid://4496695972" then
			frL.BackgroundColor3 = BrickColor.new("New Yeller").Color
			frR.BackgroundColor3 = BrickColor.new("New Yeller").Color
			frT.BackgroundColor3 = BrickColor.new("New Yeller").Color
			frB.BackgroundColor3 = BrickColor.new("New Yeller").Color
		end
		local mag = (char.HumanoidRootPart.CFrame.p - meshPart.CFrame.p).magnitude
			if mag > 100 then
				local extra = mag/100
				bb.Size = UDim2.new(5*extra, 0, 5*extra)
			end
		coroutine.yield()
	end))
end
makeTargetLabels = function(targ, player)
	if targ ~= nil and player ~= nil then
		coroutine.resume(coroutine.create(function()
			local rigType = detectRigType(targ)
			if rigType == "Abort" or targ:FindFirstChild("HumanoidRootPart") == nil then return end
			local bb = Instance.new("BillboardGui", targ.HumanoidRootPart)
			bb.AlwaysOnTop = true
			bb.Size = UDim2.new(5, 0, 5, 0)
			game.Debris:AddItem(bb, .5)
			local frL = Instance.new("Frame", bb)
			frL.BorderSizePixel = 0
			frL.Size = UDim2.new(.05, 0, 1.05, 0)
			frL.BackgroundColor3 = BrickColor.new("Really red").Color
			local frR = Instance.new("Frame", bb)
			frR.BorderSizePixel = 0
			frR.Size = UDim2.new(.05, 0, 1.05, 0)
			frR.Position = UDim2.new(.99, 0, 0, 0)
			frR.BackgroundColor3 = BrickColor.new("Really red").Color
			local frT = Instance.new("Frame", bb)
			frT.BorderSizePixel = 0
			frT.Size = UDim2.new(1, 0, .05, 0)
			frT.Position = UDim2.new(0, 0, 0, 0)
			frT.BackgroundColor3 = BrickColor.new("Really red").Color
			local frB = Instance.new("Frame", bb)
			frB.BorderSizePixel = 0
			frB.Size = UDim2.new(1, 0, .05, 0)
			frB.Position = UDim2.new(0, 0, 0.99, 0)
			frB.BackgroundColor3 = BrickColor.new("Really red").Color
			--Health bars
			local hum = detectHumanoid(targ)
			if hum ~= nil then
				local fr = Instance.new("Frame", bb)
				fr.BorderSizePixel = 0
				fr.Size = UDim2.new(1, 5, .1, 5)
				fr.Position = UDim2.new(.025, 0, .9, 0)
				fr.BackgroundColor3 = Color3.new(0, 0, 0)
				local hp = Instance.new("Frame", fr)
				hp.BackgroundColor3 = BrickColor.new("Forest green").Color
				hp.BorderSizePixel = 0
				local total
				if targ:FindFirstChild("Health") ~= nil then
					total = (hum.Value/hum.MaxValue)
				end
				hp.Size = UDim2.new(total, 0, 1, 0)
			end
			local txt = Instance.new("TextLabel", bb)
			txt.BackgroundTransparency = 1
			txt.TextScaled = true
			txt.TextStrokeTransparency = 0
			txt.TextColor3 = BrickColor.new("Really red").Color
			txt.Position = UDim2.new(.05, 0, .05, 0)
			txt.Size = UDim2.new(.95, 20, .1, 20)--uses scalar and native based sizing for distance
			txt.Text = targ.Name
			if hum ~= nil then
				if targ:FindFirstChild("Health") ~= nil then
					local estimated = math.floor((hum.Value/hum.MaxValue)*100)
					txt.Text = targ.Name.." "..tostring(estimated).."%"
				end
			end
			local team = detectTeam(player)
			if team ~= nil then
				txt.TextColor3 = team.Color
				frL.BackgroundColor3 = team.Color
				frR.BackgroundColor3 = team.Color
				frT.BackgroundColor3 = team.Color
				frB.BackgroundColor3 = team.Color
			end
			local mag = (char.HumanoidRootPart.CFrame.p - targ.HumanoidRootPart.CFrame.p).magnitude
			if mag > 100 then
				local extra = mag/100
				bb.Size = UDim2.new(5*extra, 0, 5*extra)
			end
			table.insert(targetLabels, bb)
			coroutine.yield()
		end))
	end
end
local playerESP = makeButton(Esp, UDim2.new(1, 0, .25, 0), UDim2.new(0, 0, 0, 0), "Player ESP OFF", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
local arrowESP = makeButton(Esp, UDim2.new(1, 0, .25, 0), UDim2.new(0, 0, .25, 0), "Arrow ESP OFF", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
local RokaESP = makeButton(Esp, UDim2.new(1, 0, .25, 0), UDim2.new(0, 0, .5, 0), "Roka ESP OFF", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
local vampESP = makeButton(Esp, UDim2.new(1, 0, .25, 0), UDim2.new(0, 0, .75, 0), "Vampire Mask ESP OFF", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
playerESP.MouseButton1Click:connect(function()
	if findPlayers == false then
		findPlayers = true
		playerESP.Text = "Player ESP ON"
	else
		findPlayers = false
		playerESP.Text = "Player ESP OFF"
	end
end)
arrowESP.MouseButton1Click:connect(function()
	if findArrows == false then
		findArrows = true
		arrowESP.Text = "Arrow ESP ON"
	else
		findArrows = false
		arrowESP.Text = "Arrow ESP OFF"
	end
end)
RokaESP.MouseButton1Click:connect(function()
	if findRokas == false then
		findRokas = true
		RokaESP.Text = "Roka ESP ON"
	else
		findRokas = false
		RokaESP.Text = "Roka ESP OFF"
	end
end)
vampESP.MouseButton1Click:connect(function()
	if findMasks == false then
		findMasks = true
		vampESP.Text = "Vampire Mask ESP ON"
	else
		findMasks = false
		vampESP.Text = "Vampire Mask ESP OFF"
	end
end)
--Credits
local credits = makeBaseDragUI(UDim2.new(.2, 0, .45, 0), UDim2.new(.65, 0, .4, 0), "Credits")
local maker = makeLabel(credits, UDim2.new(1, 0, 1, 0), UDim2.new(0, 0, 0, 0), "Made by deadline3652/Cameron", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
--Noclip
local noclipF = makeBaseDragUI(UDim2.new(.2, 0, .45, 0), UDim2.new(.65, 0, .45, 0), "Noclip(N=Down)(M=Up)")
local nobutton = makeButton(noclipF, UDim2.new(1, 0, 1, 0), UDim2.new(0, 0, 0, 0), "Noclip is OFF", BrickColor.new("Royal purple").Color, Color3.new(0, 0, 0), .4)
nobutton.MouseButton1Click:connect(function()
	if noclip == false then
		noclip = true
		nobutton.Text = "Noclip is ON"
	else
		noclip = false
		nobutton.Text = "Noclip is OFF"
	end
end)
--Looped Runservice
print("EXPLOIT RUNNING! Made by deadline3652/Cameron")
UIS.InputBegan:connect(function(input)
	local i = input.KeyCode
	if noclip == true then
		if i == Enum.KeyCode.N then
			char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame* CFrame.new(0, -5, 0)
		elseif i == Enum.KeyCode.M then
			char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame* CFrame.new(0, 5, 0)
		end
	end
end)
game:GetService("RunService").RenderStepped:connect(function()
	char = player.Character
	char.Humanoid.AutoRotate = true
	--WS
	if changingWalkSpeed == true then
		char.Humanoid.WalkSpeed = walkSpeed
	end
	--JP
	if changingJumpPower == true then
		char.Humanoid.JumpPower = jumpPower
	end
	--NOCLIP
	if noclip == true then
		game.Players.LocalPlayer.Character.Humanoid:ChangeState(11)
	end
	--ESP
	for i,v in pairs(targetLabels) do
		v:Destroy()
	end
	for i,v in pairs(itemLabels) do
		v:Destroy()
	end
	itemLabels = {}
	targetLabels = {}
	if findPlayers == true then
		for i,v in pairs(game:GetService("Players"):GetChildren()) do 
			if v.Name ~= player.Name then
				makeTargetLabels(v.Character, v)
			end
		end
	end
	if game.Workspace:FindFirstChild("Item_Spawns") then
		for i,v in pairs(game.Workspace:WaitForChild("Item_Spawns"):WaitForChild("Items"):GetChildren()) do
			if v:WaitForChild("Base"):FindFirstChild("ParticleEmitter") == nil then
				if v.Base.MeshId == "rbxassetid://3497428510" then
					if findRokas == true then
						itemLabel(v.Base)
					end
				elseif v.Base.MeshId == "rbxassetid://4496695972" then
					if findArrows == true then
						itemLabel(v.Base)
					end
				else
					if findMasks == true then
						itemLabel(v.Base)
					end
				end
			end
		end
	end
end)
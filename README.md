local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "cerolzera"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 310)
MainFrame.Position = UDim2.new(0.5, -110, 0.4, -155)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "CEROLZERA | BLOX FRUITS"
Title.TextColor3 = Color3.fromRGB(255, 60, 60)
Title.TextSize = 16
Title.Font = Enum.Font.SourceSansBold
Title.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = MainFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

Title.LayoutOrder = 0

local function CreateButton(name, order, isFunctional, callback)
	local Button = Instance.new("TextButton")
	Button.Name = name
	Button.Size = UDim2.new(0.85, 0, 0, 35)
	Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	Button.Text = name
	Button.TextColor3 = Color3.fromRGB(255, 255, 255)
	Button.TextSize = 14
	Button.Font = Enum.Font.SourceSansSemibold
	Button.LayoutOrder = order
	Button.Parent = MainFrame

	local BtnCorner = Instance.new("UICorner")
	BtnCorner.CornerRadius = UDim.new(0, 6)
	BtnCorner.Parent = Button

	local isActive = false

	Button.MouseButton1Click:Connect(function()
		isActive = not isActive
		
		if isActive then
			Button.BackgroundColor3 = Color3.fromRGB(60, 180, 75)
		else
			Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
		end

		if isFunctional and callback then
			callback(isActive)
		end
	end)

	return Button
end

CreateButton("Auto PVP Aura", 1, false)
CreateButton("Auto Cerol Aura", 2, false)
CreateButton("Max Aura", 3, false)
CreateButton("Max Cerol Aura", 4, false)

local function BloxFruitsFPSBoost(enable)
	if enable then
		Lighting.GlobalShadows = false
		Lighting.FogEnd = 9e9
		
		for _, child in pairs(Lighting:GetChildren()) do
			if child:IsA("PostEffect") then
				child.Enabled = false
			end
		end

		local function CleanObject(obj)
			if obj:IsA("BasePart") and not obj:IsA("MeshPart") then
				obj.Material = Enum.Material.SmoothPlastic
				obj.Reflectance = 0
			elseif obj:IsA("Decal") or obj:IsA("Texture") then
				obj.Transparency = 1
			elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") or obj:IsA("Fire") then
				obj.Enabled = false
			end
		end

		for _, obj in pairs(Workspace:GetDescendants()) do
			CleanObject(obj)
		end

		-- Optimizes new elements spawned in the map (skill effects and attacks)
		Workspace.DescendantAdded:Connect(function(obj)
			CleanObject(obj)
		end)
	end
end

CreateButton("Optimize FPS", 5, true, BloxFruitsFPSBoost)

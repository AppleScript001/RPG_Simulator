local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()
local Mobs_Table = {}
for _, v in ipairs(workspace.Mobs:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not table.find(Mobs_Table, v.Name) then
        table.insert(Mobs_Table, v.Name)
        table.sort(Mobs_Table)
    end
end
local PlayerTP1
local TweenService = game:GetService("TweenService")
local X = Material.Load({
	Title = "RPG Simulator",
	Style = 2,
	SizeX = 350,
	SizeY = 380,
	Theme = "Dark",
	ColorOverrides = {
		MainFrame = Color3.fromRGB(0,9,55)
	}
})
local Y = X.New({
	Title = "Main"
})
local ii = Y.Dropdown({
	Text = "Select Mobs!",
	Callback = function(t)
        PlayerTP1 = t
	end,
	Options = Mobs_Table
})
Y.Button(
    {
        Text = "Refresh/Mobs",
        Callback = function()
            table.clear(Mobs_Table)
            for i, v in pairs(workspace.Mobs:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(Mobs_Table, tostring(v)) then
                        table.insert(Mobs_Table, tostring(v))
                        ii:SetOptions(Mobs_Table)
                    end
                end
            end
        end
    }
)
Y.Toggle({
Text = "Auto Farm/Mobs",
Callback = function(Value)
_G.mob = Value
while _G.mob do
task.wait(0.1)
pcall(function()
for i,v in pairs(workspace.Mobs:GetDescendants()) do
if v.Name == PlayerTP1 then
if v.Humanoid.Health > 0 then
repeat
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,6)
wait()  -- Wait a short time before checking again
until not _G.mob or v.Humanoid.Health <= 0
end
end
end
end)
end
end,
Enabled = false
})
Y.Toggle({
Text = "Auto Slash",
Callback = function(Value)
_G.Slash = Value
while _G.Slash do
task.wait(0.1)
    local args = {
        [1] = "Slash"
    }
    
    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("attack"):FireServer(unpack(args))
end
end,
Enabled = false
})

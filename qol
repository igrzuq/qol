
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

OrionLib:MakeNotification({
	Name = "igrzuq's QOL GUI",
	Content = "QOL",
	Image = "rbxassetid://4483345998",
	Time = 5
})


local Window = OrionLib:MakeWindow({Name = "igrzuq's QOL GUI", HidePremium = false, SaveConfig = true, ConfigFolder = "QOL"})

--Player Tab--

local PlayerTab = Window:MakeTab({
	Name = "Player",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

local PlayerSection = PlayerTab:AddSection({
	Name = "Player"
})


local defaultWalkSpeed = nil -- Store the default walkspeed
local speedControlEnabled = true -- Flag for toggle state

PlayerSection:AddToggle({
    Name = "Speed Control",
    Default = true,
    Callback = function(Value)
        speedControlEnabled = Value
        local player = game.Players.LocalPlayer
        local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
        
        if humanoid then
            if speedControlEnabled then
                -- Save default speed when toggled on
                defaultWalkSpeed = humanoid.WalkSpeed
                print("Speed control enabled. Default speed saved:", defaultWalkSpeed)
            else
                -- Reset speed to default when toggled off
                if defaultWalkSpeed then
                    humanoid.WalkSpeed = defaultWalkSpeed
                    print("Speed control disabled. Speed reset to default:", defaultWalkSpeed)
                end
            end
        end
    end
})

PlayerSection:AddSlider({
    Name = "Walkspeed",
    Min = 0,
    Max = 100,
    Default = 12,
    Color = Color3.fromRGB(255,255,255),
    Increment = 1,
    ValueName = "Walkspeed",
    Callback = function(Value)
        if speedControlEnabled then
            local player = game.Players.LocalPlayer
            local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
            
            if humanoid then
                humanoid.WalkSpeed = Value
                print("Walkspeed set to:", Value)
            else
                print("Humanoid not found")
            end
        else
            print("Speed control is disabled. Walkspeed change ignored.")
        end
    end
})

local infiniteJumpEnabled = false -- Toggle state for infinite jump

-- Infinite Jump Functionality
local function ToggleInfiniteJump(Value)
    infiniteJumpEnabled = Value
    print("Infinite Jump:", infiniteJumpEnabled and "Enabled" or "Disabled")
end

-- Infinite Jump Logic
game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        local player = game.Players.LocalPlayer
        local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
        
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Add Infinite Jump Toggle to GUI
PlayerSection:AddToggle({
    Name = "Infinite Jump",
    Default = false,
    Callback = function(Value)
        ToggleInfiniteJump(Value)
    end
})



-- Anti-AFK variables
local antiAFKEnabled = false
local virtualUser = game:GetService("VirtualUser")
local connection

PlayerSection:AddToggle({
    Name = "ANTI AFK",
    Default = false,
    Callback = function(Value)
        antiAFKEnabled = Value
        if antiAFKEnabled then
            -- Enable anti-AFK
            connection = game:GetService("Players").LocalPlayer.Idled:Connect(function()
                virtualUser:CaptureController()
                virtualUser:ClickButton2(Vector2.new())
            end)
            print("Anti-AFK enabled")
        else
            -- Disable anti-AFK
            if connection then
                connection:Disconnect()
                connection = nil
            end
            print("Anti-AFK disabled")
        end
    end
})

local freecamEnabled = false -- Toggle state for freecam
local Speed = 50 -- Default camera movement speed
local RotationSpeed = 2 -- Default camera rotation speed
local Camera = workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

local Movement = {
    Forward = 0,
    Backward = 0,
    Left = 0,
    Right = 0,
    Up = 0,
    Down = 0
}

local Rotation = Vector2.new(0, 0) -- X = Pitch, Y = Yaw
local keyPressed = {} -- Table to track the state of arrow keys

-- Toggle Freecam Function
local function ToggleFreecam(Value)
    freecamEnabled = Value
    if freecamEnabled then
        Camera.CameraType = Enum.CameraType.Scriptable
        Humanoid.WalkSpeed = 0 -- Freeze player movement
        print("Freecam enabled")
    else
        Camera.CameraType = Enum.CameraType.Custom
        Humanoid.WalkSpeed = 16 -- Default WalkSpeed
        print("Freecam disabled")
    end
end

-- Handle Movement Inputs
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed or not freecamEnabled then return end

    if input.KeyCode == Enum.KeyCode.S then
        Movement.Forward = 1
    elseif input.KeyCode == Enum.KeyCode.W then
        Movement.Backward = 1
    elseif input.KeyCode == Enum.KeyCode.A then
        Movement.Left = 1
    elseif input.KeyCode == Enum.KeyCode.D then
        Movement.Right = 1
    elseif input.KeyCode == Enum.KeyCode.Space then
        Movement.Up = 1
    elseif input.KeyCode == Enum.KeyCode.LeftControl then
        Movement.Down = 1
    elseif input.KeyCode == Enum.KeyCode.Up then
        keyPressed.Up = true
    elseif input.KeyCode == Enum.KeyCode.Down then
        keyPressed.Down = true
    elseif input.KeyCode == Enum.KeyCode.Right then
        keyPressed.Left = true
    elseif input.KeyCode == Enum.KeyCode.Left then
        keyPressed.Right = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if not freecamEnabled then return end

    if input.KeyCode == Enum.KeyCode.S then
        Movement.Forward = 0
    elseif input.KeyCode == Enum.KeyCode.W then
        Movement.Backward = 0
    elseif input.KeyCode == Enum.KeyCode.A then
        Movement.Left = 0
    elseif input.KeyCode == Enum.KeyCode.D then
        Movement.Right = 0
    elseif input.KeyCode == Enum.KeyCode.Space then
        Movement.Up = 0
    elseif input.KeyCode == Enum.KeyCode.LeftControl then
        Movement.Down = 0
    elseif input.KeyCode == Enum.KeyCode.Up then
        keyPressed.Up = false
    elseif input.KeyCode == Enum.KeyCode.Down then
        keyPressed.Down = false
    elseif input.KeyCode == Enum.KeyCode.Right then
        keyPressed.Left = false
    elseif input.KeyCode == Enum.KeyCode.Left then
        keyPressed.Right = false
    end
end)

-- Camera Movement Logic
RunService.RenderStepped:Connect(function(deltaTime)
    if not freecamEnabled then return end

    -- Calculate movement direction relative to camera orientation
    local moveVector = Vector3.new(
        Movement.Right - Movement.Left,
        Movement.Up - Movement.Down,
        Movement.Forward - Movement.Backward
    )

    if moveVector.Magnitude > 0 then
        local moveCFrame = Camera.CFrame:VectorToWorldSpace(moveVector)
        Camera.CFrame = Camera.CFrame + (moveCFrame.Unit * Speed * deltaTime)
    end

    -- Apply continuous rotation if arrow keys are held down
    if keyPressed.Up then
        Rotation = Rotation + Vector2.new(-RotationSpeed, 0) -- Look up
    end
    if keyPressed.Down then
        Rotation = Rotation + Vector2.new(RotationSpeed, 0) -- Look down
    end
    if keyPressed.Left then
        Rotation = Rotation + Vector2.new(0, -RotationSpeed) -- Look left
    end
    if keyPressed.Right then
        Rotation = Rotation + Vector2.new(0, RotationSpeed) -- Look right
    end

    -- Apply rotation to the camera without clamping the pitch (vertical rotation)
    local rotationCFrame = CFrame.Angles(
        math.rad(Rotation.X), -- Pitch (look up/down)
        math.rad(Rotation.Y), -- Yaw (look left/right)
        0
    )

    Camera.CFrame = CFrame.new(Camera.CFrame.Position) * rotationCFrame
end)

-- Add Freecam Toggle and Speed Sliders to GUI
PlayerSection:AddToggle({
    Name = "Drone",
    Default = false,
    Callback = function(Value)
        ToggleFreecam(Value)
    end
})

PlayerSection:AddSlider({
    Name = "Drone Speed",
    Min = 1,
    Max = 1000,
    Default = 50,
    Color = Color3.fromRGB(255, 255, 255),
    Increment = 1,
    ValueName = "Speed",
    Callback = function(Value)
        Speed = Value -- Update the freecam speed
    end
})

PlayerSection:AddSlider({
    Name = "Drone Rotation Speed",
    Min = 1,
    Max = 50,
    Default = 2,
    Color = Color3.fromRGB(255, 255, 255),
    Increment = 0.5,
    ValueName = "Rotation Speed",
    Callback = function(Value)
        RotationSpeed = Value -- Update the rotation speed
    end
})


--Settings Tab--

local SettingsTab = Window:MakeTab({
	Name = "Settings",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

local SettingsSection = SettingsTab:AddSection({
	Name = "Settings"
})

SettingsSection:AddButton({
	Name = "Destroy UI",
	Callback = function()
        OrionLib:Destroy()
  	end    
})

--Settings End--

OrionLib:Init() --UI Lib End

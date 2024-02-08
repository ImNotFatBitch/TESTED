local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- /Variables/

local Player = game.Players.LocalPlayer
local workspace = game:GetService("Workspace")
local players = game:GetService("Players")
local runService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local localPlayer = game.Players.LocalPlayer
local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
local abilitiesFolder = character:WaitForChild("Abilities")
local upgrades = localPlayer.Upgrades
local player = game.Players.LocalPlayer
local heartbeatConnection
local character = player.Character or player.CharacterAdded:Wait()
local replicatedStorage = game:GetService("ReplicatedStorage")
local runService = game:GetService("RunService")
local parryButtonPress = replicatedStorage.Remotes.ParryButtonPress
local ballsFolder = workspace:WaitForChild("Balls")
local VirtualInput = game:GetService("VirtualInputManager")
local Camera = game:GetService("Workspace").CurrentCamera
local LocalPlayer = game.Players.LocalPlayer
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local VirtualInput = game:GetService("VirtualInputManager")
local UserInputService = game:GetService("UserInputService")
local BASE_THRESHOLD = 0.2
local focusedBall, displayBall = nil, nil
local VELOCITY_SCALING_FACTOR_FAST = 0.050
local VELOCITY_SCALING_FACTOR_VERYFAST = 0.000000005
local VELOCITY_SCALING_FACTOR_SLOW = 0.1
local PlayerGui = localPlayer:WaitForChild("PlayerGui")
local function onCharacterAdded(newCharacter)
    character = newCharacter
    abilitiesFolder = character:WaitForChild("Abilities")
end

localPlayer.CharacterAdded:Connect(onCharacterAdded)

local TruValue = Instance.new("StringValue")
if workspace:FindFirstChild("AbilityThingyk1212") then
    workspace:FindFirstChild("AbilityThingyk1212"):Remove()
    task.wait(0.1)
    TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Dash"
    else
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Dash"
end

-- /Variables/

-- Config
local ESPEnabled = false
local ESPNameColor = Color3.fromRGB(255,255,255)
local ESPBoxColor = Color3.fromRGB(255,255,255)
local PlayerLineEnabled = false
local PlayerDistanceEnabled = false
local ObjectDistanceEnabled = false
local sliderValue = 15
local ToggleBox = false
local ToggleHealthBar = false
local ToggleNames = false
-- Config

-- Source ESP
local function getCharacterSize(character)
    local head = character:FindFirstChild("Head")
    local human = character:FindFirstChild("Humanoid")
    if head and human then
        local headSize = head.Size
        local torsoSize = Vector3.new(2, 2, 1)
        return headSize + torsoSize
    end
    return Vector3.new(0, 0, 0)
end

local EspList = {}

local function createESP(Player)
    local Distance = Drawing.new("Text")
    Distance.Size = 16
    Distance.Outline = true
    Distance.Center = true

    local Line = Drawing.new("Line")
    Line.Thickness = 2
    Line.Visible = false

    local function update()
        local Character = Player.Character
        if Character then
            local Humanoid = Character:FindFirstChildOfClass("Humanoid")
            if Humanoid and Humanoid.Health > 0 then
                local Torso = Character:FindFirstChild("HumanoidRootPart")
                if Torso then
                    local TorsoPos, TorsoOnScreen = Camera:WorldToViewportPoint(Torso.Position)

                    if TorsoOnScreen then
                        local X, Y = TorsoPos.X, TorsoPos.Y

                        local playerPos = localPlayer.Character.HumanoidRootPart.Position
                        local objectPos = Torso.Position
                        local dist = (playerPos - objectPos).Magnitude
                        Distance.Position = Vector2.new(X, Y + 10)

                        Distance.Visible = ESPEnabled

                        if PlayerDistanceEnabled then
                            Distance.Visible = ESPEnabled
                        else
                            Distance.Visible = false
                        end

                        if PlayerLineEnabled then
                            Line.Color = ESPBoxColor
                            Line.From = Vector2.new(X, Y)
                            Line.To = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                            Line.Visible = ESPEnabled
                        else
                            Line.Visible = false
                        end
                    else
                        Distance.Visible = false
                        Line.Visible = false
                    end
                end
            end
        end
    end

    local Connection1 = Player.CharacterAdded:Connect(function()
        update()
    end)

    local Connection2 = Player.CharacterRemoving:Connect(function()
        Distance.Visible = false
        Line.Visible = false
    end)

    update()

    return {
        update = update,
        disconnect = function()
            Distance:Remove()
            Line:Remove()
            Connection1:Disconnect()
            Connection2:Disconnect()
        end,
        Player = Player
    }
end


local EspList = {}

local function updateRainbow()
    if RainbowEnabled then
        RainbowIndex = (RainbowIndex + 1/RainbowSpeed) % #RainbowColors
        local ColorIndex = math.floor(RainbowIndex) + 1
        ESPNameColor = RainbowColors[ColorIndex]
        ESPBoxColor = RainbowColors[ColorIndex]
        for _, Esp in pairs(EspList) do
            Esp.update()
        end
    end
end

local EspList = {}
for _, Player in pairs(game.Players:GetPlayers()) do
    if Player ~= game.Players.LocalPlayer then
        table.insert(EspList, createESP(Player))
    end
end

game:GetService("RunService").RenderStepped:Connect(function()
    for _, Esp in pairs(EspList) do
        Esp.update()
    end
    updateRainbow()
end)

game.Players.PlayerAdded:Connect(function(Player)
    if Player ~= game.Players.LocalPlayer then
        table.insert(EspList, createESP(Player))
    end
end)

local ObjectEspList = {}
local LineColor = Color3.new(255,255,255)
local DistanceColor = Color3.new(255,255,255)

local function createObjectESP(object)
    local Distance = Drawing.new("Text")
    Distance.Size = 16
    Distance.Outline = true
    Distance.Center = true

    local Line = Drawing.new("Line")
    Line.Thickness = 2
    Line.Visible = false

    local function update()
        if object and object.Parent == game.Workspace.Balls then
            local Pos, OnScreen = Camera:WorldToViewportPoint(object.Position)
            if OnScreen then
                local X = Pos.X
                local Y = Pos.Y
                Distance.Position = Vector2.new(X, Y - 33)
                Line.From = Vector2.new(X, Y)
                Line.To = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                Distance.Visible = ESPEnabled
                Line.Visible = ESPEnabled

                local playerPos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                local objectPos = object.Position
                local dist = (playerPos - objectPos).Magnitude
                Distance.Text = string.format("%.2f", dist)

                if character:FindFirstChild("Highlight") then
                    Line.Color = Color3.new(1, 0, 0)
                    Distance.Color = Color3.new(1, 0, 0)
                elseif RainbowMode then
                    local color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
                    Line.Color = color
                    Distance.Color = color
                else
                    Line.Color = LineColor
                    Distance.Color = DistanceColor
                end

            else
                Line.Visible = false
                Distance.Visible = false
            end
        else
            Line.Visible = false
            Distance.Visible = false
        end
    end

    update()

    local nameChangedConnection = object:GetPropertyChangedSignal("Name"):Connect(function()
        update()
    end)

    return {
        update = update,
        disconnect = function()
            Distance:Remove()
            Line:Remove()
            nameChangedConnection:Disconnect()
        end,
        Object = object
    }
end


local ObjectEspList = {}
for _, object in pairs(game.Workspace.Balls:GetChildren()) do
    table.insert(ObjectEspList, createObjectESP(object))
end

game:GetService("RunService").RenderStepped:Connect(function()
    for _, Esp in pairs(ObjectEspList) do
        Esp.update()
    end
end)

local function updateObjectEsp()
    for _, Esp in pairs(ObjectEspList) do
        Esp.update()
    end
end

-- Source ESP (Continued)


-- Auto Parry

local function startAutoParry()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local ballsFolder = workspace:WaitForChild("Balls")
    local parryButtonPress = replicatedStorage.Remotes.ParryButtonPress
    local abilityButtonPress = replicatedStorage.Remotes.AbilityButtonPress

    print("Script successfully ran.")

    local function onCharacterAdded(newCharacter)
        character = newCharacter
    end
    localPlayer.CharacterAdded:Connect(onCharacterAdded)

    if character then
        print("Character found.")
    else
        print("Character not found.")
        return
    end
    

local function chooseNewFocusedBall()
    local balls = ballsFolder:GetChildren()
    for _, ball in ipairs(balls) do
        if ball:GetAttribute("realBall") ~= nil and ball:GetAttribute("realBall") == true then
            focusedBall = ball
            print(focusedBall.Name)
            break
        elseif ball:GetAttribute("target") ~= nil then
            focusedBall = ball
            print(focusedBall.Name)
            break
        end
    end
    
    if focusedBall == nil then
        print("Could not find a ball.")
    end
    return focusedBall
end


    chooseNewFocusedBall()

    local BASE_THRESHOLD = 0.15
    local VELOCITY_SCALING_FACTOR_FAST = 0.050
    local VELOCITY_SCALING_FACTOR_SLOW = 0.1

    local function getDynamicThreshold(ballVelocityMagnitude)
        if ballVelocityMagnitude > 60 then
            print("Speed calculated")
            return math.max(0.20, BASE_THRESHOLD - (ballVelocityMagnitude * VELOCITY_SCALING_FACTOR_FAST))
        elseif isAutoSpamParryEnabled and ballVelocityMagnitude > 120 then
            print("Spam Parry Enabled")
            game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
        else
            return math.min(0.01, BASE_THRESHOLD + (ballVelocityMagnitude * VELOCITY_SCALING_FACTOR_SLOW))
        end
    end

    local function timeUntilImpact(ballVelocity, distanceToPlayer, playerVelocity)
        local directionToPlayer = (character.HumanoidRootPart.Position - focusedBall.Position).Unit
        local velocityTowardsPlayer = ballVelocity:Dot(directionToPlayer) - playerVelocity:Dot(directionToPlayer)
        
        if velocityTowardsPlayer <= 0 then
            return math.huge
        end
        
        return (distanceToPlayer - sliderValue) / velocityTowardsPlayer
    end

    local function isWalkSpeedZero()
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            return humanoid.WalkSpeed == 0
        end
        return false
    end


    local function checkBallDistance()
        if not character or not character:FindFirstChild("Highlight") then return end

        local charPos = character.PrimaryPart.Position
        local charVel = character.PrimaryPart.Velocity

        if focusedBall and not focusedBall.Parent then
            print("Focused ball.")
            chooseNewFocusedBall()
        end
        if not focusedBall then 
            print("No focused ball.")
            chooseNewFocusedBall()
        end

        local ball = focusedBall
        local distanceToPlayer = (ball.Position - charPos).Magnitude
        local ballVelocityTowardsPlayer = ball.Velocity:Dot((charPos - ball.Position).Unit)
        
        if distanceToPlayer < 10 then
            parryButtonPress:Fire()
        end
        local isCheckingRage = false

        if timeUntilImpact(ball.Velocity, distanceToPlayer, charVel) < getDynamicThreshold(ballVelocityTowardsPlayer) then
            if (character.Abilities["Raging Deflection"].Enabled or character.Abilities["Rapture"].Enabled) and UseRage == true then
                if not isCheckingRage then
                    isCheckingRage = true
                    abilityButtonPress:Fire()
                    if not isWalkSpeedZero() then
                        parryButtonPress:Fire()
                    end
                    isCheckingRage = false
                end
            else
                parryButtonPress:Fire()
            end
        end
    end


    heartbeatConnection = game:GetService("RunService").Heartbeat:Connect(function()
        checkBallDistance()
    end)
end

local function stopAutoParry()
    if heartbeatConnection then
        heartbeatConnection:Disconnect()
        heartbeatConnection = nil
    end
end

-- Auto Parry

local Window = Fluent:CreateWindow({
    Title = "Blade Ball " .. Fluent.Version,
    SubTitle = "by VeryFat",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = false,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Esp = Window:AddTab({ Title = "Esp", Icon = "" }),
    Abilities = Window:AddTab({ Title = "abilities", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do

local Esp = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/x114/RobloxScripts/main/OpenSourceEsp"))()

local AutoParrys = Tabs.Main:AddToggle("AutoParrys", { Title = "Auto Parry Legit", Default = false })

AutoParrys:OnChanged(function(Value)
    if Value then
       startAutoParry()
    else
       stopAutoParry()
    end
end)

local isAutoSpamParryEnabled = false

local SpamParry = Tabs.Main:AddToggle("SpamParryS", { Title = "Auto Spam Parry", Default = false })

SpamParry:OnChanged(function(value)
    isAutoSpamParryEnabled = value
    if value then
        print("Spam Parry Enable")
    else
        print("Spam parry Disable !")
    end
end)

local SpamParry1 = Tabs.Main:AddToggle("SpamParryS1", { Title = "Manual Spam Parry", Default = false })

SpamParry1:OnChanged(function(value)
    if value then
        print("Manual Spam Parry Enable !")
        spamming = true

        while spamming do
            game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
            wait()
        end
    else
        print("Manual Spam Parry Disable !")
        spamming = false
    end
end)

local isKeybindEnabled = false

local Keybind = Tabs.Main:AddKeybind("Keybind", {
    Title = "Manual Spam Parry KeyBind",
    Mode = "Toggle",
    Default = "E",
    Callback = function(Value)
        print("KeyBind SpamParry Statue :", Value)
        isKeybindEnabled = Value
    end,
})

task.spawn(function()
    while true do
        wait()
        if isKeybindEnabled then
            game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
        end

        if Fluent.Unloaded then
            break
        end
    end
end)

local Toggle = Tabs.Main:AddToggle("TpBehind", {Title = "Tp Behind", Default = false })

Toggle:OnChanged(function()
    if Toggle.Value then
        print("DEBUG ON")
    else
        print("DEBUG OFF")
    end
end)

game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
    if Toggle.Value then
        TeleportBehindClosestPlayer()
    end
end)

function TeleportBehindClosestPlayer()
    local players = game:GetService("Players"):GetPlayers()
    local myCharacter = game.Players.LocalPlayer.Character

    local closestPlayer = nil
    local closestDistance = math.huge

    for _, player in ipairs(players) do
        if player ~= game.Players.LocalPlayer then
            local character = player.Character
            if character then
                local distance = (character.HumanoidRootPart.Position - myCharacter.HumanoidRootPart.Position).magnitude
                if distance < closestDistance then
                    closestPlayer = player
                    closestDistance = distance
                end
            end
        end
    end

    if closestPlayer then
        local targetPosition = closestPlayer.Character.HumanoidRootPart.Position
        local direction = (targetPosition - myCharacter.HumanoidRootPart.Position).unit
        local teleportDistance = 5
        local newPosition = targetPosition - direction * teleportDistance

        myCharacter:SetPrimaryPartCFrame(CFrame.new(newPosition))
    else
        print("Nobody found")
    end
end



local Slider = Tabs.Main:AddSlider("Slider", {
    Title = "Ajust Distance",
    Description = "Ajust Distance Auto Parry",
    Default = 20,
    Min = 15,
    Max = 40,
    Rounding = 0,
    Callback = function(Value)
      sliderValue = Value
    end
})

Tabs.Main:AddParagraph({
    Title = "Blatant",
    Content = ""
})

local Config = {
    Box               = false,
    BoxOutline        = false,
    BoxColor          = ESPBoxColor,
    BoxOutlineColor   = Color3.fromRGB(0,0,0),
    HealthBar         = false,
    HealthBarSide     = "Left",
    Names             = false,
    NamesOutline      = false,
    NamesColor        = ESPNameColor,
    NamesOutlineColor = Color3.fromRGB(0,0,0),
    NamesFont         = 2,
    NamesSize         = 13
}

local EnableEsp = Tabs.Esp:AddToggle("EnableEsp", { Title = "Enable Esp", Default = false })
local ESPBoxs = Tabs.Esp:AddToggle("ESP Box", { Title = "ESP Box", Default = false })
local ESPNames = Tabs.Esp:AddToggle("ESP Names", { Title = "ESP Names", Default = false })
local ESPTracers = Tabs.Esp:AddToggle("ESPTracer", { Title = "ESP Tracers", Default = false })
local ToggleBalls = Tabs.Esp:AddToggle("EspBalls", { Title = "ESP Balls", Default = false })

-- All Button Abilities

Tabs.Abilities:AddButton({
    Title = "Dash",
    Description = "Take Dash",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Dash"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Super Jump",
    Description = "Take Super Jump",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Super Jump"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Platform",
    Description = "Take Platform",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Platform"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Quad Jump",
    Description = "Take Quad Jump",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Quad Jump"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Invisibility",
    Description = "Take Invisibility",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Invisibility"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Thunder Dash",
    Description = "Take Thunder Dash",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Thunder Dash"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Blink",
    Description = "Take Blink",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Blink"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Wind Cloak",
    Description = "Take Wind Cloak",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Wind Cloak"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Shadow Step",
    Description = "Take Shadow Step",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Shadow Step"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Freeze",
    Description = "Take Freeze",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Freeze"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Swap",
    Description = "Take Swap",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Swap"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Forcefield",
    Description = "Take Forcefield",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Forcefield"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Reaper",
    Description = "Take Reaper",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Reaper"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Raging Deflect",
    Description = "Take Raging Deflect",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Raging Deflection"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Telekinesis",
    Description = "Take Telekinesis",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Telekinesis"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Pull",
    Description = "Take Pull",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Pull"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})


Tabs.Abilities:AddButton({
    Title = "Phantom",
    Description = "Take Phantom",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Phantom"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Phase Bypass",
    Description = "Take Phase Bypass",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Phase Bypass"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Waypoint",
    Description = "Take Waypoint",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Waypoint"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Rapture",
    Description = "Take Rapture",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Rapture"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

Tabs.Abilities:AddButton({
    Title = "Infinity",
    Description = "Take Infinity",
    Callback = function()
      local function AbilityValue2()
      local TruValue = Instance.new("StringValue")
      workspace:FindFirstChild("AbilityThingyk1212"):Remove()
        TruValue.Parent = game:GetService("Workspace")
        TruValue.Name = "AbilityThingyk1212"
        TruValue.Value = "Infinity"
      end

      for i,v in pairs(abilitiesFolder:GetChildren()) do


      for i,b in pairs(abilitiesFolder:GetChildren()) do
       local Ability = b
      
       if v.Enabled == true then
        local EquippedAbility = v
        local ChosenAbility = {}
        spawn(function()
        ChosenAbility = AbilityValue2()
      end)

    task.wait(0.05)
        local AbilityValue = workspace.AbilityThingyk1212
        if b.Name == AbilityValue.Value then

            v.Enabled = false
            b.Enabled = true
    end
  end
 end
 end
end,
})

-- All Button Abilities

EnableEsp:OnChanged(function(bool)
    ESPEnabled = bool
end)

ESPBoxs:OnChanged(function(bool)
    ToggleBox = bool
    Esp.Box = bool
end)

ESPNames:OnChanged(function(bool)
    ToggleNames = bool
    Esp.Names = bool
end)

Esp.Enabled = true

local function UpdateEsp()
    Esp.Box = ToggleBox
    Esp.HealthBar = ToggleHealthBar
    Esp.Names = ToggleNames
end

UpdateEsp()

ESPTracers:OnChanged(function(bool)
    PlayerLineEnabled = bool
end)

ToggleBalls:OnChanged(function(Value)
            ObjectESPEnabled = Value
            if Value then
            for _, Esp in pairs(ObjectEspList) do
                Esp.disconnect()
            end
            ObjectEspList = {}
            local object = game.Workspace.Balls:GetChildren()[1]
            if object then
                local Esp = createObjectESP(object)
                table.insert(ObjectEspList, Esp)
                object:GetPropertyChangedSignal("Name"):Connect(function()
                    Esp.update()
                end)
            end
            game:GetService("RunService").RenderStepped:Connect(updateObjectEsp)
            game.Workspace.Balls.ChildAdded:Connect(function(child)
                for _, Esp in pairs(ObjectEspList) do
                    Esp.disconnect()
                end
                ObjectEspList = {}
                local newEsp = createObjectESP(child)
                table.insert(ObjectEspList, newEsp)
            end)
        else
            for _, Esp in pairs(ObjectEspList) do
                Esp.disconnect()
            end
            ObjectEspList = {}
        end
end)

local AutoParryB = Tabs.Main:AddToggle("AutoParryB", { Title = "Auto Parry Blatant", Default = false })

AutoParryB:OnChanged(function(state)
    Toggle = state
    if state then
        while Toggle do
            task.wait()
            local ballsFolder = workspace:FindFirstChild("Balls")
            
            if ballsFolder then
                local balls = ballsFolder:GetChildren()
                
                for i = 1, #balls do
                    local ball = balls[i]
                    if ball:IsA("BasePart") then
                        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local character = game.Players.LocalPlayer.Character
                            local ballPosition = ball.Position
                            local newPosition
                            
                            if TeleportDirection == "Up" then
                                newPosition = Vector3.new(ballPosition.X, ballPosition.Y + sliderValue1, ballPosition.Z)
                            elseif TeleportDirection == "Down" then
                                newPosition = Vector3.new(ballPosition.X, ballPosition.Y - sliderValue1, ballPosition.Z)
                            else
                                newPosition = Vector3.new(
                                    ballPosition.X + math.random(-15, 15),
                                    ballPosition.Y + math.random(-15, 15),
                                    ballPosition.Z + math.random(-15, 15)
                                )
                            end
                            
                            character.HumanoidRootPart.CFrame = CFrame.new(newPosition)
                            
                            if game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then
                                game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
                                task.wait()
                            end
                        end
                    end
                end
            end
        end
    end
end)

local Dropdown = Tabs.Main:AddDropdown("Dropdown", {
    Title = "Teleport Blatant Direction",
    Values = {"Up", "Down", "Random"},
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(Value)
    if Value == "Up" then
        TeleportDirection = "Up"
    elseif Value == "Down" then
        TeleportDirection = "Down"
    else
        TeleportDirection = "Random"
    end
end)

local Slider = Tabs.Main:AddSlider("DistanceSlider", {
    Title = "Adjust teleport distance",
    Description = "Adjust teleport distance for Up/Down mode",
    Default = 10,
    Min = 5,
    Max = 15,
    Rounding = 0,
    Callback = function(Value)
        sliderValue1 = Value
    end
})

Dropdown:SetValue("Up")

Dropdown:OnChanged(function(Value)
    TeleportDirection = Value
end)
end

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
SaveManager:IgnoreThemeSettings()

SaveManager:SetIgnoreIndexes({})

InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "Fluent",
    Content = "The script has been loaded.",
    Duration = 8
})

SaveManager:LoadAutoloadConfig()

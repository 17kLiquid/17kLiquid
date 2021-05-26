game:GetService("CoreGui")
-- init
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/GreenDeno/Venyx-UI-Library/main/source.lua"))()
local venyx = library.new("FE Abdi", 5013109572)

-- themes
local themes = {
Background = Color3.fromRGB(24, 24, 24),
Glow = Color3.fromRGB(0, 0, 0),
Accent = Color3.fromRGB(10, 10, 10),
LightContrast = Color3.fromRGB(20, 20, 20),
DarkContrast = Color3.fromRGB(14, 14, 14),  
TextColor = Color3.fromRGB(255, 255, 255)
}

-- first page
local page = venyx:addPage("FE Script", 5012544693)
local section1 = page:addSection("FE scripts 1")




section1:addButton("FE fake vr", function()
    -- CLOVR - FE FULL-BODY VR SCRIPT, UPDATED ON MARCH 17, 2020, TO SUPPORT ACTUAL CHARACTER USAGE
 
-- IT DOESN'T WORK ON RAGDOLL GAMES, PLEASE PLAY THIS SCRIPT AT R6 GAMES LIKE PRISION LIFE, CAFE GAMES, ETC...
-- Also, please use a strong executor like Synapse X, Sentinel, (jk sentinel is garbage) or KRNL. Other executors will NOT work. Feel free to make this a require, I honestly don't give a shit.
 
--  | made by 0866!!!!!!! and abacaxl!!!!!!!!
--  | tysm unverified
 
-- This is an older version of CLOVR, I found that the legs would 'walk' better in this version than the one we released.
 
--Controls are a bit different from last release because this is older. Still works good!
 
--RagDollEnabled is set to true, DON'T set it to false or CLOVR won't work. Feel free to change the other settings though. -Abacaxl
 
 --get network ownership through sethiddenproperty
 spawn(function()while wait()do
    sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.pow(math.huge, math.huge))
    sethiddenproperty(game.Players.LocalPlayer,"MaximumSimulationRadius",math.pow(math.huge, math.huge))
    end end)
     
     
    --|| Settings:
     
    local StudsOffset = 0 -- Character height (negative if you're too high)
    local Smoothness = .5 -- Character interpolation (0.1 - 1 = smooth - rigid)
     
    local AnchorCharacter = true -- Prevent physics from causing inconsistencies
    local HideCharacter = false -- Hide character on a platform
    local NoCollision = true -- Disable player collision
     
    local ChatEnabled = false -- See chat on your left hand in-game
     local ChatLocalRange = 75 -- Local chat range
     
    local ViewportEnabled = true -- View nearby players in a frame
     local ViewportRange = 30 -- Maximum distance players are updated
     
    local RagdollEnabled = true -- Use your character instead of hats (NetworkOwner vulnerability)
     local RagdollHeadMovement = true -- Move your head separately from your body (+9 second wait)
     
    local AutoRun = false -- Run script on respawn
    local AutoRespawn = true -- Kill your real body when your virtual body dies
     
    local WearAllAccessories = true -- Use all leftover hats for the head
    local AccurateHandPosition = true -- Move your Roblox hands according to your real hands
     
    local AccessorySettings = {
        LeftArm     = "";
        RightArm    = "";
        LeftLeg     = "";
        RightLeg    = "";
        Torso       = "";
        Head        = true;
       
        BlockArms   = true;
        BlockLegs   = true;
        BlockTorso  = true;
       
        LimbOffset  = CFrame.Angles(math.rad(90), 0, 0);
    }
     
    local FootPlacementSettings = {
        RightOffset = Vector3.new(.5, 0, 0),
        LeftOffset = Vector3.new(-.5, 0, 0),
    }
     
    --|| Script:
     
    local Script = nil;
     
    Script = function()
     
    --[[
        Variables
    --]]
     
    local Players = game:GetService("Players")
     local Client = Players.LocalPlayer
      local Character = Client.Character or Client.CharacterAdded:Wait()
       local WeldBase = Character:WaitForChild("HumanoidRootPart")
       local ArmBase = Character:FindFirstChild("RightHand") or Character:FindFirstChild("Right Arm") or WeldBase
      local Backpack = Client:WaitForChild("Backpack")
      local Mouse = Client:GetMouse()
     
    local Camera = workspace.CurrentCamera
     
    local VRService = game:GetService("VRService")
     local VRReady = VRService.VREnabled
     
    local UserInputService = game:GetService("UserInputService")
    local RunService = game:GetService("RunService")
    local HttpService = game:GetService("HttpService")
    local StarterGui = game:GetService("StarterGui")   
     
    local HeadAccessories = {};
    local UsedAccessories = {};
     
    local Pointer = false;
    local Point1 = false;
    local Point2 = false;
     
    local VirtualRig = game:GetObjects("rbxassetid://4468539481")[1]
    local VirtualBody = game:GetObjects("rbxassetid://4464983829")[1]
     
    local Anchor = Instance.new("Part")
     
    Anchor.Anchored = true
    Anchor.Transparency = 1
    Anchor.CanCollide = false
    Anchor.Parent = workspace
     
    if RagdollEnabled then
        print("RagdollEnabled, thank you for using CLOVR!")
        local NetworkAccess = coroutine.create(function()
    settings().Physics.AllowSleep = false
    while true do game:GetService("RunService").RenderStepped:Wait()
    for _,Players in next, game:GetService("Players"):GetChildren() do
    if Players ~= game:GetService("Players").LocalPlayer then
    Players.MaximumSimulationRadius = 0.1 Players.SimulationRadius = 0 end end
    game:GetService("Players").LocalPlayer.MaximumSimulationRadius = math.pow(math.huge,math.huge)
    game:GetService("Players").LocalPlayer.SimulationRadius = math.huge*math.huge end end)
    coroutine.resume(NetworkAccess)
    end
     
     
     
    --[[
        Character Protection
    --]]
     
    local CharacterCFrame = WeldBase.CFrame
     
    if not RagdollEnabled then
        Character.Humanoid.AnimationPlayed:Connect(function(Animation)
            Animation:Stop()
        end)
       
        for _, Track in next, Character.Humanoid:GetPlayingAnimationTracks() do
            Track:Stop()
        end
       
        if HideCharacter then
            local Platform = Instance.new("Part")
           
            Platform.Anchored = true
            Platform.Size = Vector3.new(100, 5, 100)
            Platform.CFrame = CFrame.new(0, 10000, 0)
            Platform.Transparency = 1
            Platform.Parent = workspace
           
            Character:MoveTo(Platform.Position + Vector3.new(0, 5, 0))
           
            wait(.5)
        end
       
        if AnchorCharacter then
            for _, Part in pairs(Character:GetChildren()) do
                if Part:IsA("BasePart") then
                    Part.Anchored = true
                end
            end
        end
    end
     
    --[[
        Functions
    --]]
     
    function Tween(Object, Style, Direction, Time, Goal)
        local tweenInfo = TweenInfo.new(Time, Enum.EasingStyle[Style], Enum.EasingDirection[Direction])
        local tween = game:GetService("TweenService"):Create(Object, tweenInfo, Goal)
     
        tween.Completed:Connect(function()
            tween:Destroy()
        end)
       
        tween:Play()
     
        return tween
    end
     
    local function GetMotorForLimb(Limb)
        for _, Motor in next, Character:GetDescendants() do
            if Motor:IsA("Motor6D") and Motor.Part1 == Limb then
                return Motor
            end
        end
    end
     
    local function CreateAlignment(Limb, Part0)
        local Attachment0 = Instance.new("Attachment", Part0 or Anchor)
        local Attachment1 = Instance.new("Attachment", Limb)
       
        local Orientation = Instance.new("AlignOrientation")
        local Position = Instance.new("AlignPosition")
       
        Orientation.Attachment0 = Attachment1
        Orientation.Attachment1 = Attachment0
        Orientation.RigidityEnabled = false
        Orientation.MaxTorque = 20000
        Orientation.Responsiveness = 40
        Orientation.Parent = Character.HumanoidRootPart
       
        Position.Attachment0 = Attachment1
        Position.Attachment1 = Attachment0
        Position.RigidityEnabled = false
        Position.MaxForce = 40000
        Position.Responsiveness = 40
        Position.Parent = Character.HumanoidRootPart
       
        Limb.Massless = false
       
        local Motor = GetMotorForLimb(Limb)
        if Motor then
            Motor:Destroy()
        end
       
        return function(CF, Local)
            if Local then
                Attachment0.CFrame = CF
            else
                Attachment0.WorldCFrame = CF
            end
        end;
    end
     
    local function GetExtraTool()
        for _, Tool in next, Character:GetChildren() do
            if Tool:IsA("Tool") and not Tool.Name:match("LIMB_TOOL") then
                return Tool
            end
        end
    end
     
    local function GetGripForHandle(Handle)
        for _, Weld in next, Character:GetDescendants() do
            if Weld:IsA("Weld") and (Weld.Part0 == Handle or Weld.Part1 == Handle) then
                return Weld
            end
        end
       
        wait(.2)
       
        for _, Weld in next, Character:GetDescendants() do
            if Weld:IsA("Weld") and (Weld.Part0 == Handle or Weld.Part1 == Handle) then
                return Weld
            end
        end
    end
     
    local function CreateRightGrip(Handle)
        local RightGrip = Instance.new("Weld")
       
        RightGrip.Name = "RightGrip"
        RightGrip.Part1 = Handle
        RightGrip.Part0 = WeldBase
        RightGrip.Parent = WeldBase
       
        return RightGrip
    end
     
    local function CreateAccessory(Accessory, DeleteMeshes)
        if not Accessory then
            return
        end
       
        local HatAttachment = Accessory.Handle:FindFirstChildWhichIsA("Attachment")
        local HeadAttachment = VirtualRig:FindFirstChild(HatAttachment.Name, true)
        local BasePart = HeadAttachment.Parent
       
        local HatAtt = HatAttachment.CFrame
        local HeadAtt = HeadAttachment.CFrame
       
        if DeleteMeshes then
            if Accessory.Handle:FindFirstChild("Mesh") then
                Accessory.Handle.Mesh:Destroy()
            end
        end
       
        wait()
       
        local Handle = Accessory:WaitForChild("Handle")
       
        if Handle:FindFirstChildWhichIsA("Weld", true) then
            Handle:FindFirstChildWhichIsA("Weld", true):Destroy()
            Handle:BreakJoints()
        else
            Handle:BreakJoints()
        end
       
        Handle.Massless = true
        Handle.Transparency = 0.5
       
        UsedAccessories[Accessory] = true
       
        local RightGrip = CreateRightGrip(Handle)
       
        wait()
       
        for _, Object in pairs(Handle:GetDescendants()) do
            if not Object:IsA("BasePart") then
                pcall(function()
                    Object.Transparency = 1
                end)
               
                pcall(function()
                    Object.Enabled = false
                end)
            end
        end
       
        return Handle, RightGrip, HatAtt, HeadAtt, BasePart;
    end
     
    local function GetHeadAccessories()
        for _, Accessory in next, Character:GetChildren() do
            if Accessory:IsA("Accessory") and not UsedAccessories[Accessory] then
                local Handle, RightGrip, HatAtt, HeadAtt, BasePart = CreateAccessory(Accessory)
               
                table.insert(HeadAccessories, {Handle, RightGrip, HatAtt, HeadAtt, BasePart})
               
                do
                    Handle.Transparency = 1
                end
               
                if not WearAllAccessories then
                    break
                end
            end
        end
    end
     
    --[[
        VR Replication Setup
    --]]
     
    if not RagdollEnabled then
        LeftHandle, LeftHandGrip = CreateAccessory(Character:FindFirstChild(AccessorySettings.LeftArm), AccessorySettings.BlockArms)
        RightHandle, RightHandGrip = CreateAccessory(Character:FindFirstChild(AccessorySettings.RightArm), AccessorySettings.BlockArms)
        LeftHipHandle, LeftLegGrip = CreateAccessory(Character:FindFirstChild(AccessorySettings.LeftLeg), AccessorySettings.BlockLegs)
        RightHipHandle, RightLegGrip = CreateAccessory(Character:FindFirstChild(AccessorySettings.RightLeg), AccessorySettings.BlockLegs)
        TorsoHandle, TorsoGrip = CreateAccessory(Character:FindFirstChild(AccessorySettings.Torso), AccessorySettings.BlockTorso)
        GetHeadAccessories()
       
    elseif RagdollEnabled then
        if RagdollHeadMovement then
            Permadeath()
            MoveHead = CreateAlignment(Character["Head"])
        end
       
        MoveRightArm = CreateAlignment(Character["Right Arm"])
        MoveLeftArm = CreateAlignment(Character["Left Arm"])
        MoveRightLeg = CreateAlignment(Character["Right Leg"])
        MoveLeftLeg = CreateAlignment(Character["Left Leg"])
        MoveTorso = CreateAlignment(Character["Torso"])
        MoveRoot = CreateAlignment(Character.HumanoidRootPart)
       
        if RagdollHeadMovement then
            for _, Accessory in next, Character:GetChildren() do
                if Accessory:IsA("Accessory") and Accessory:FindFirstChild("Handle") then
                    local Attachment1 = Accessory.Handle:FindFirstChildWhichIsA("Attachment")
                    local Attachment0 = Character:FindFirstChild(tostring(Attachment1), true)
                   
                    local Orientation = Instance.new("AlignOrientation")
                    local Position = Instance.new("AlignPosition")
                   
                    print(Attachment1, Attachment0, Accessory)
                   
                    Orientation.Attachment0 = Attachment1
                    Orientation.Attachment1 = Attachment0
                    Orientation.RigidityEnabled = false
                    Orientation.ReactionTorqueEnabled = true
                    Orientation.MaxTorque = 20000
                    Orientation.Responsiveness = 40
                    Orientation.Parent = Character.Head
                   
                    Position.Attachment0 = Attachment1
                    Position.Attachment1 = Attachment0
                    Position.RigidityEnabled = false
                    Position.ReactionForceEnabled = true
                    Position.MaxForce = 40000
                    Position.Responsiveness = 40
                    Position.Parent = Character.Head
                end
            end
        end
    end
     
    --[[
        Movement
    --]]
     
    VirtualRig.Name = "VirtualRig"
    VirtualRig.RightFoot.BodyPosition.Position = CharacterCFrame.p
    VirtualRig.LeftFoot.BodyPosition.Position = CharacterCFrame.p
    VirtualRig.Parent = workspace
    VirtualRig:SetPrimaryPartCFrame(CharacterCFrame)
     
    VirtualRig.Humanoid.Health = 50
    VirtualRig:BreakJoints()
    --
     
    VirtualBody.Parent = workspace
    VirtualBody.Name = "VirtualBody"
    VirtualBody.Humanoid.WalkSpeed = 8
    VirtualBody.Humanoid.CameraOffset = Vector3.new(0, StudsOffset, 0)
    VirtualBody:SetPrimaryPartCFrame(CharacterCFrame)
     
    VirtualBody.Humanoid.Died:Connect(function()
        print("Virtual death")
        if AutoRespawn then
            Character:BreakJoints()
           
            if RagdollHeadMovement and RagdollEnabled then
                Network:Unclaim()
                Respawn()
            end
        end
    end)
    --
     
    Camera.CameraSubject = VirtualBody.Humanoid
     
    Character.Humanoid.WalkSpeed = 0
    Character.Humanoid.JumpPower = 1
     
    for _, Part in next, VirtualBody:GetChildren() do
        if Part:IsA("BasePart") then
            Part.Transparency = 1
        end
    end
     
    for _, Part in next, VirtualRig:GetChildren() do
        if Part:IsA("BasePart") then
            Part.Transparency = 1
        end
    end
     
    if not VRReady then
        VirtualRig.RightUpperArm.ShoulderConstraint.RigidityEnabled = true
        VirtualRig.LeftUpperArm.ShoulderConstraint.RigidityEnabled = true
    end
     
     
    local OnMoving = RunService.Stepped:Connect(function()
        local Direction = Character.Humanoid.MoveDirection
        local Start = VirtualBody.HumanoidRootPart.Position
        local Point = Start + Direction * 6
       
        VirtualBody.Humanoid:MoveTo(Point)
    end)
     
    Character.Humanoid.Jumping:Connect(function()
        VirtualBody.Humanoid.Jump = true
    end)
     
    UserInputService.JumpRequest:Connect(function()
        VirtualBody.Humanoid.Jump = true
    end)
     
    --[[
        VR Replication
    --]]
     
    if RagdollEnabled then
        for _, Part in pairs(Character:GetDescendants()) do
            if Part:IsA("BasePart") and Part.Name == "Handle" and Part.Parent:IsA("Accessory") then
                Part.LocalTransparencyModifier = 1
            elseif Part:IsA("BasePart") and Part.Transparency < 0.5 and Part.Name ~= "Head" then
                Part.LocalTransparencyModifier = 0.5
            elseif Part:IsA("BasePart") and Part.Name == "Head" then
                Part.LocalTransparencyModifier = 1
            end
           
            if not Part:IsA("BasePart") and not Part:IsA("AlignPosition") and not Part:IsA("AlignOrientation") then
                pcall(function()
                    Part.Transparency = 1
                end)
               
                pcall(function()
                    Part.Enabled = false
                end)
            end
        end
    end
     
    local FootUpdateDebounce = tick()
     
    local function FloorRay(Part, Distance)
        local Position = Part.CFrame.p
        local Target = Position - Vector3.new(0, Distance, 0)
        local Line = Ray.new(Position, (Target - Position).Unit * Distance)
       
        local FloorPart, FloorPosition, FloorNormal = workspace:FindPartOnRayWithIgnoreList(Line, {VirtualRig, VirtualBody, Character})
       
        if FloorPart then
            return FloorPart, FloorPosition, FloorNormal, (FloorPosition - Position).Magnitude
        else
            return nil, Target, Vector3.new(), Distance
        end
    end
     
    local function Flatten(CF)
        local X,Y,Z = CF.X,CF.Y,CF.Z
        local LX,LZ = CF.lookVector.X,CF.lookVector.Z
       
        return CFrame.new(X,Y,Z) * CFrame.Angles(0,math.atan2(LX,LZ),0)
    end
     
    local FootTurn = 1
     
    local function FootReady(Foot, Target)
        local MaxDist
       
        if Character.Humanoid.MoveDirection.Magnitude > 0 then
            MaxDist = .5
        else
            MaxDist = 1
        end
       
        local PastThreshold = (Foot.Position - Target.Position).Magnitude > MaxDist
        local PastTick = tick() - FootUpdateDebounce >= 2
       
        if PastThreshold or PastTick then
            FootUpdateDebounce = tick()
        end
       
        return
            PastThreshold
        or
            PastTick
    end
     
    local function FootYield()
        local RightFooting = VirtualRig.RightFoot.BodyPosition
        local LeftFooting = VirtualRig.LeftFoot.BodyPosition
        local LowerTorso = VirtualRig.LowerTorso
       
        local Yield = tick()
       
        repeat
            RunService.Stepped:Wait()
            if
                (LowerTorso.Position - RightFooting.Position).Y > 4
            or
                (LowerTorso.Position - LeftFooting.Position).Y > 4
            or
                ((LowerTorso.Position - RightFooting.Position) * Vector3.new(1, 0, 1)).Magnitude > 4
            or
                ((LowerTorso.Position - LeftFooting.Position) * Vector3.new(1, 0, 1)).Magnitude > 4
            then
                break
            end
        until tick() - Yield >= .17
    end
     
    local function UpdateFooting()
        if not VirtualRig:FindFirstChild("LowerTorso") then
            wait()
            return
        end
       
        local Floor, FloorPosition, FloorNormal, Dist = FloorRay(VirtualRig.LowerTorso, 3)
       
        Dist = math.clamp(Dist, 0, 5)
       
        local FootTarget =
            VirtualRig.LowerTorso.CFrame *
            CFrame.new(FootPlacementSettings.RightOffset) -
            Vector3.new(0, Dist, 0) +
            Character.Humanoid.MoveDirection * (VirtualBody.Humanoid.WalkSpeed / 8) * 2
       
        if FootReady(VirtualRig.RightFoot, FootTarget) then
            VirtualRig.RightFoot.BodyPosition.Position = FootTarget.p
            VirtualRig.RightFoot.BodyGyro.CFrame = Flatten(VirtualRig.LowerTorso.CFrame)
        end
       
        FootYield()
       
        local FootTarget =
            VirtualRig.LowerTorso.CFrame *
            CFrame.new(FootPlacementSettings.LeftOffset) -
            Vector3.new(0, Dist, 0) +
            Character.Humanoid.MoveDirection * (VirtualBody.Humanoid.WalkSpeed / 8) * 2
       
        if FootReady(VirtualRig.LeftFoot, FootTarget) then
            VirtualRig.LeftFoot.BodyPosition.Position = FootTarget.p
            VirtualRig.LeftFoot.BodyGyro.CFrame = Flatten(VirtualRig.LowerTorso.CFrame)
        end
    end
     
    local function UpdateTorsoPosition()
        if not RagdollEnabled then
            if TorsoHandle then
                local Positioning = VirtualRig.UpperTorso.CFrame
               
                if not TorsoGrip or not TorsoGrip.Parent then
                    TorsoGrip = CreateRightGrip(TorsoHandle)
                end
               
                local Parent = TorsoGrip.Parent
               
                TorsoGrip.C1 = CFrame.new()
                TorsoGrip.C0 = TorsoGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(Positioning * CFrame.new(0, -0.25, 0) * AccessorySettings.LimbOffset), Smoothness)
                TorsoGrip.Parent = nil
                TorsoGrip.Parent = Parent
            end
        else
            local Positioning = VirtualRig.UpperTorso.CFrame
           
            MoveTorso(Positioning * CFrame.new(0, -0.25, 0))
            MoveRoot(Positioning * CFrame.new(0, -0.25, 0))
        end
    end
     
    local function UpdateLegPosition()
        if not RagdollEnabled then
            if RightHipHandle then
                local Positioning =
                    VirtualRig.RightLowerLeg.CFrame
                    : Lerp(VirtualRig.RightFoot.CFrame, 0.5)
                    + Vector3.new(0, 0.5, 0)
               
                if not RightHipHandle or not RightHipHandle.Parent then
                    RightLegGrip = CreateRightGrip(RightHipHandle)
                end
               
                local Parent = RightLegGrip.Parent
               
                RightLegGrip.C1 = CFrame.new()
                RightLegGrip.C0 = RightLegGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(Positioning * AccessorySettings.LimbOffset), Smoothness)
                RightLegGrip.Parent = nil
                RightLegGrip.Parent = Parent
            end
           
            if LeftHipHandle then
                local Positioning =
                    VirtualRig.LeftLowerLeg.CFrame
                    : Lerp(VirtualRig.LeftFoot.CFrame, 0.5)
                    + Vector3.new(0, 0.5, 0)
               
                if not LeftLegGrip or not LeftLegGrip.Parent then
                    LeftLegGrip = CreateRightGrip(LeftHipHandle)
                end
               
                local Parent = LeftLegGrip.Parent
               
                LeftLegGrip.C1 = CFrame.new()
                LeftLegGrip.C0 = LeftLegGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(Positioning * AccessorySettings.LimbOffset), Smoothness)
                LeftLegGrip.Parent = nil
                LeftLegGrip.Parent = Parent
            end
        else
            do
                local Positioning =
                    VirtualRig.RightLowerLeg.CFrame
                    : Lerp(VirtualRig.RightFoot.CFrame, 0.5)
                    * CFrame.Angles(0, math.rad(180), 0)
                    + Vector3.new(0, 0.5, 0)
               
                MoveRightLeg(Positioning)
            end
           
            do
                local Positioning =
                    VirtualRig.LeftLowerLeg.CFrame
                    : Lerp(VirtualRig.LeftFoot.CFrame, 0.5)
                    * CFrame.Angles(0, math.rad(180), 0)
                    + Vector3.new(0, 0.5, 0)
               
                MoveLeftLeg(Positioning)
            end
        end
    end
     
    warn("VRReady is", VRReady)
     
    local function OnUserCFrameChanged(UserCFrame, Positioning, IgnoreTorso)
        local Positioning = Camera.CFrame * Positioning
       
        if not IgnoreTorso then
            UpdateTorsoPosition()
            UpdateLegPosition()
        end
       
        if not RagdollEnabled then
            if UserCFrame == Enum.UserCFrame.Head and AccessorySettings.Head then
                for _, Table in next, HeadAccessories do
                    local Handle, RightGrip, HatAtt, HeadAtt, BasePart = unpack(Table)
                    local LocalPositioning = Positioning
                   
                    if not RightGrip or not RightGrip.Parent then
                        RightGrip = CreateRightGrip(Handle)
                        Table[2] = RightGrip
                    end
                   
                    local Parent = RightGrip.Parent
                   
                    if BasePart then
                        LocalPositioning = BasePart.CFrame * HeadAtt
                    end
                   
                    RightGrip.C1 = HatAtt
                    RightGrip.C0 = RightGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(LocalPositioning), Smoothness)
                    RightGrip.Parent = nil
                    RightGrip.Parent = Parent
                end
               
            elseif RightHandle and UserCFrame == Enum.UserCFrame.RightHand and AccessorySettings.RightArm then
                local HandPosition = Positioning
                local LocalPositioning = Positioning
               
                if not RightHandGrip or not RightHandGrip.Parent then
                    RightHandGrip = CreateRightGrip(RightHandle)
                end
               
                if AccurateHandPosition then
                    HandPosition = HandPosition * CFrame.new(0, 0, 1)
                end
               
                if not VRReady then
                    local HeadRotation = Camera.CFrame - Camera.CFrame.p
                   
                    HandPosition = VirtualRig.RightUpperArm.CFrame:Lerp(VirtualRig.RightLowerArm.CFrame, 0.5) * AccessorySettings.LimbOffset
                   
                    --LocalPositioning = (HeadRotation + (HandPosition * CFrame.new(0, 0, 1)).p) * CFrame.Angles(math.rad(-45), 0, 0)
                    LocalPositioning = HandPosition * CFrame.new(0, 0, 1) * CFrame.Angles(math.rad(-180), 0, 0)
                   
                    if Point2 then
                        VirtualRig.RightUpperArm.Aim.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                        VirtualRig.RightUpperArm.Aim.CFrame = Camera.CFrame * AccessorySettings.LimbOffset
                    elseif VirtualRig.RightUpperArm.Aim.MaxTorque ~= Vector3.new(0, 0, 0) then
                        VirtualRig.RightUpperArm.Aim.MaxTorque = Vector3.new(0, 0, 0)
                    end
                elseif AccurateHandPosition then
                    LocalPositioning = HandPosition
                end
               
                local Parent = RightHandGrip.Parent
               
                RightHandGrip.C1 = CFrame.new()
                RightHandGrip.C0 = RightHandGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(HandPosition), Smoothness)
                RightHandGrip.Parent = nil
                RightHandGrip.Parent = Parent
               
                --
               
                local EquippedTool = GetExtraTool()
               
                if EquippedTool and EquippedTool:FindFirstChild("Handle") then
                    local EquippedGrip = GetGripForHandle(EquippedTool.Handle)
                    local Parent = EquippedGrip.Parent
                   
                    local ArmBaseCFrame = ArmBase.CFrame
                    if ArmBase.Name == "Right Arm" then
                        ArmBaseCFrame = ArmBaseCFrame
                    end
                   
                    EquippedGrip.C1 = EquippedTool.Grip
                    EquippedGrip.C0 = EquippedGrip.C0:Lerp(ArmBaseCFrame:ToObjectSpace(LocalPositioning), Smoothness)
                    EquippedGrip.Parent = nil
                    EquippedGrip.Parent = Parent
                end
               
            elseif LeftHandle and UserCFrame == Enum.UserCFrame.LeftHand and AccessorySettings.LeftArm then
                local HandPosition = Positioning
               
                if not LeftHandGrip or not LeftHandGrip.Parent then
                    LeftHandGrip = CreateRightGrip(LeftHandle)
                end
               
                if AccurateHandPosition then
                    HandPosition = HandPosition * CFrame.new(0, 0, 1)
                end
               
                if not VRReady then
                    HandPosition = VirtualRig.LeftUpperArm.CFrame:Lerp(VirtualRig.LeftLowerArm.CFrame, 0.5) * AccessorySettings.LimbOffset
                    --warn("Setting HandPosition to hands")
                    if Point1 then
                        VirtualRig.LeftUpperArm.Aim.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                        VirtualRig.LeftUpperArm.Aim.CFrame = Camera.CFrame * AccessorySettings.LimbOffset
                    elseif VirtualRig.LeftUpperArm.Aim.MaxTorque ~= Vector3.new(0, 0, 0) then
                        VirtualRig.LeftUpperArm.Aim.MaxTorque = Vector3.new(0, 0, 0)
                    end
                end
               
                local Parent = LeftHandGrip.Parent
               
                LeftHandGrip.C1 = CFrame.new()
                LeftHandGrip.C0 = LeftHandGrip.C0:Lerp(WeldBase.CFrame:ToObjectSpace(HandPosition), Smoothness)
                LeftHandGrip.Parent = nil
                LeftHandGrip.Parent = Parent
               
            end
        end
       
        if RagdollEnabled then
            if UserCFrame == Enum.UserCFrame.Head and RagdollHeadMovement then
                MoveHead(Positioning)
            elseif UserCFrame == Enum.UserCFrame.RightHand then
                local Positioning = Positioning
               
                if not VRReady then
                    Positioning = VirtualRig.RightUpperArm.CFrame:Lerp(VirtualRig.RightLowerArm.CFrame, 0.5)
                elseif AccurateHandPosition then
                    Positioning = Positioning * CFrame.new(0, 0, 1)
                end
               
                if VRReady then
                    Positioning = Positioning * AccessorySettings.LimbOffset
                end
               
                MoveRightArm(Positioning)
               
                if Point2 then
                    VirtualRig.RightUpperArm.Aim.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                    VirtualRig.RightUpperArm.Aim.CFrame = Camera.CFrame * AccessorySettings.LimbOffset
                elseif VirtualRig.RightUpperArm.Aim.MaxTorque ~= Vector3.new(0, 0, 0) then
                    VirtualRig.RightUpperArm.Aim.MaxTorque = Vector3.new(0, 0, 0)
                end
            elseif UserCFrame == Enum.UserCFrame.LeftHand then
                local Positioning = Positioning
               
                if not VRReady then
                    Positioning = VirtualRig.LeftUpperArm.CFrame:Lerp(VirtualRig.LeftLowerArm.CFrame, 0.5)
                elseif AccurateHandPosition then
                    Positioning = Positioning * CFrame.new(0, 0, 1)
                end
               
                if VRReady then
                    Positioning = Positioning * AccessorySettings.LimbOffset
                end
               
                MoveLeftArm(Positioning)
               
                if Point1 then
                    VirtualRig.LeftUpperArm.Aim.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                    VirtualRig.LeftUpperArm.Aim.CFrame = Camera.CFrame * AccessorySettings.LimbOffset
                elseif VirtualRig.LeftUpperArm.Aim.MaxTorque ~= Vector3.new(0, 0, 0) then
                    VirtualRig.LeftUpperArm.Aim.MaxTorque = Vector3.new(0, 0, 0)
                end
            end
        end
       
        if UserCFrame == Enum.UserCFrame.Head then
            VirtualRig.Head.CFrame = Positioning
           
        elseif UserCFrame == Enum.UserCFrame.RightHand and VRReady then
            VirtualRig.RightHand.CFrame = Positioning
           
        elseif UserCFrame == Enum.UserCFrame.LeftHand and VRReady then
            VirtualRig.LeftHand.CFrame = Positioning
           
        end
       
        if not VRReady and VirtualRig.LeftHand.Anchored then
            VirtualRig.RightHand.Anchored = false
            VirtualRig.LeftHand.Anchored = false
        elseif VRReady and not VirtualRig.LeftHand.Anchored then
            VirtualRig.RightHand.Anchored = true
            VirtualRig.LeftHand.Anchored = true
        end
    end
     
    local CFrameChanged = VRService.UserCFrameChanged:Connect(OnUserCFrameChanged)
     
    local OnStepped = RunService.Stepped:Connect(function()
        for _, Part in pairs(VirtualRig:GetChildren()) do
            if Part:IsA("BasePart") then
                Part.CanCollide = false
            end
        end
       
        if RagdollEnabled then
            for _, Part in pairs(Character:GetChildren()) do
                if Part:IsA("BasePart") then
                    Part.CanCollide = false
                end
            end
        end
       
        if NoCollision then
            for _, Player in pairs(Players:GetPlayers()) do
                if Player ~= Client and Player.Character then
                    local Descendants = Player.Character:GetDescendants()
                    for i = 1, #Descendants do
                        local Part = Descendants[i]
                        if Part:IsA("BasePart") then
                            Part.CanCollide = false
                            Part.Velocity = Vector3.new()
                            Part.RotVelocity = Vector3.new()
                        end
                    end
                end
            end
        end
    end)
     
    local OnRenderStepped = RunService.Stepped:Connect(function()
        Camera.CameraSubject = VirtualBody.Humanoid
       
        if RagdollEnabled then
            Character.HumanoidRootPart.CFrame = VirtualRig.UpperTorso.CFrame
            Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
        end
       
        if not VRReady then
            OnUserCFrameChanged(Enum.UserCFrame.Head, CFrame.new(0, 0, 0))
           
            OnUserCFrameChanged(Enum.UserCFrame.RightHand, CFrame.new(0, 0, 0), true)
            OnUserCFrameChanged(Enum.UserCFrame.LeftHand, CFrame.new(0, 0, 0), true)
        end
    end)
     
    spawn(function()
        while Character and Character.Parent do
            FootYield()
            UpdateFooting()
        end
    end)
     
    --[[
        Non-VR Support + VR Mechanics
    --]]
     
    local OnInput = UserInputService.InputBegan:Connect(function(Input, Processed)
        if not Processed then
            if Input.KeyCode == Enum.KeyCode.LeftControl or Input.KeyCode == Enum.KeyCode.ButtonL2 then
                Tween(VirtualBody.Humanoid, "Elastic", "Out", 1, {
                    CameraOffset = Vector3.new(0, StudsOffset - 1.5, 0)
                })
            end
       
            if Input.KeyCode == Enum.KeyCode.X then
                if RagdollEnabled and RagdollHeadMovement then
                    Network:Unclaim()
                    Respawn()
                end
            end
           
            if Input.KeyCode == Enum.KeyCode.C then
                VirtualBody:MoveTo(Mouse.Hit.p)
                VirtualRig:MoveTo(Mouse.Hit.p)
            end
        end
           
        if Input.KeyCode == Enum.KeyCode.LeftShift or Input.KeyCode == Enum.KeyCode.ButtonR2 then
            Tween(VirtualBody.Humanoid, "Sine", "Out", 1, {
                WalkSpeed = 16
            })
        end
       
        if not VRReady and Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Point1 = true
        end
       
        if not VRReady and Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Point2 = true
        end
       
        if VRReady and Input.KeyCode == Enum.KeyCode.ButtonY then
            Character:BreakJoints()
           
            if RagdollEnabled and RagdollHeadMovement then
                Network:Unclaim()
                Respawn()
            end
        end
    end)
     
    local OnInputEnded = UserInputService.InputEnded:Connect(function(Input, Processed)
        if not Processed then
            if Input.KeyCode == Enum.KeyCode.LeftControl or Input.KeyCode == Enum.KeyCode.ButtonL2 then
                Tween(VirtualBody.Humanoid, "Elastic", "Out", 1, {
                    CameraOffset = Vector3.new(0, StudsOffset, 0)
                })
            end
        end
           
        if Input.KeyCode == Enum.KeyCode.LeftShift or Input.KeyCode == Enum.KeyCode.ButtonR2 then
            Tween(VirtualBody.Humanoid, "Sine", "Out", 1, {
                WalkSpeed = 8
            })
        end
       
        if not VRReady and Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Point1 = false
        end
       
        if not VRReady and Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Point2 = false
        end
    end)
     
    --[[
        Proper Cleanup
    --]]
     
    local OnReset
     
    OnReset = Client.CharacterAdded:Connect(function()
        OnReset:Disconnect();
        CFrameChanged:Disconnect();
        OnStepped:Disconnect();
        OnRenderStepped:Disconnect();
        OnMoving:Disconnect();
        OnInput:Disconnect();
        OnInputEnded:Disconnect();
       
        VirtualRig:Destroy();
        VirtualBody:Destroy();
       
        if RagdollEnabled then
            Network:Unclaim();
        end
       
        if AutoRun then
            delay(2, function()
                Script()
            end)
        end
    end)
     
    if ChatEnabled then
        spawn(ChatHUDFunc)
    end
     
    if ViewportEnabled then
        spawn(ViewHUDFunc)
    end
     
    do
        --[[
            Functions
        --]]
       
        local Players = game:GetService("Players")
         local Client = Players.LocalPlayer
       
        local VRService = game:GetService("VRService")
         local VRReady = VRService.VREnabled
       
        local UserInputService = game:GetService("UserInputService")
        local RunService = game:GetService("RunService")
       
        local Camera = workspace.CurrentCamera
       
        --[[
            Code
        --]]
       
        if VRReady then
            local Pointer = game:GetObjects("rbxassetid://4476173280")[1]
           
            Pointer.Parent = workspace
            Pointer.Beam.Enabled = false
            Pointer.Target.ParticleEmitter.Enabled = false
           
            local RenderStepped = RunService.RenderStepped:Connect(function()
                if Pointer.Beam.Enabled then
                    local RightHand = Camera.CFrame * VRService:GetUserCFrame(Enum.UserCFrame.RightHand)
                    local Target = RightHand * CFrame.new(0, 0, -10)
                   
                    local Line = Ray.new(RightHand.p, (Target.p - RightHand.p).Unit * 128)
                    local Part, Position = workspace:FindPartOnRayWithIgnoreList(Line, {VirtualRig, VirtualBody, Character, Pointer})
                   
                    local Distance = (Position - RightHand.p).Magnitude
                   
                    Pointer.Target.Position = Vector3.new(0, 0, -Distance)
                    Pointer.CFrame = RightHand
                end
            end)
           
            local Input = UserInputService.InputBegan:Connect(function(Input)
                if Input.KeyCode == Enum.KeyCode.ButtonB then
                    Pointer.Beam.Enabled = not Pointer.Beam.Enabled
                    Pointer.Target.ParticleEmitter.Enabled = not Pointer.Target.ParticleEmitter.Enabled
                end
            end)
           
            --
           
            local CharacterAdded
           
            CharacterAdded = Client.CharacterAdded:Connect(function()
                RenderStepped:Disconnect()
                Input:Disconnect()
                CharacterAdded:Disconnect()
               
                Pointer:Destroy()
                Pointer = nil
            end)
        else
            return
        end
    end
     
    end;
     
    Permadeath = function()
        local ch = game.Players.LocalPlayer.Character
        local prt=Instance.new("Model", workspace)
        local z1 =  Instance.new("Part", prt)
        z1.Name="Torso"
        z1.CanCollide = false
        z1.Anchored = true
        local z2  =Instance.new("Part", prt)
        z2.Name="Head"
        z2.Anchored = true
        z2.CanCollide = false
        local z3 =Instance.new("Humanoid", prt)
        z3.Name="Humanoid"
        z1.Position = Vector3.new(0,9999,0)
        z2.Position = Vector3.new(0,9991,0)
        game.Players.LocalPlayer.Character=prt
        wait(5)
        warn("50%")
        game.Players.LocalPlayer.Character=ch
        wait(6)
        warn("100%")
    end;
     
    Respawn = function()
        local ch = game.Players.LocalPlayer.Character
       
        local prt=Instance.new("Model", workspace)
        local z1 =  Instance.new("Part", prt)
        z1.Name="Torso"
        z1.CanCollide = false
        z1.Anchored = true
        local z2  =Instance.new("Part", prt)
        z2.Name="Head"
        z2.Anchored = true
        z2.CanCollide = false
        local z3 =Instance.new("Humanoid", prt)
        z3.Name="Humanoid"
        z1.Position = Vector3.new(0,9999,0)
        z2.Position = Vector3.new(0,9991,0)
        game.Players.LocalPlayer.Character=prt
        wait(5)
        game.Players.LocalPlayer.Character=ch
    end;
     
    ChatHUDFunc = function()
        --[[
            Variables
        --]]
       
        local UserInputService = game:GetService("UserInputService")
        local RunService = game:GetService("RunService")
       
        local VRService = game:GetService("VRService")
         local VRReady = VRService.VREnabled
       
        local Players = game:GetService("Players")
         local Client = Players.LocalPlayer
       
        local ChatHUD = game:GetObjects("rbxassetid://4476067885")[1]
         local GlobalFrame = ChatHUD.GlobalFrame
          local Template = GlobalFrame.Template
         local LocalFrame = ChatHUD.LocalFrame
         local Global = ChatHUD.Global
         local Local = ChatHUD.Local
       
        local Camera = workspace.CurrentCamera
       
        Template.Parent = nil
        ChatHUD.Parent = game:GetService("CoreGui")
       
        --[[
            Code
        --]]
       
        local Highlight = Global.Frame.BackgroundColor3
        local Deselected = Local.Frame.BackgroundColor3
       
        local OpenGlobalTab = function()
            Global.Frame.BackgroundColor3 = Highlight
            Local.Frame.BackgroundColor3 = Deselected
           
            Global.Font = Enum.Font.SourceSansBold
            Local.Font = Enum.Font.SourceSans
           
            GlobalFrame.Visible = true
            LocalFrame.Visible = false
        end
       
        local OpenLocalTab = function()
            Global.Frame.BackgroundColor3 = Deselected
            Local.Frame.BackgroundColor3 = Highlight
           
            Global.Font = Enum.Font.SourceSans
            Local.Font = Enum.Font.SourceSansBold
           
            GlobalFrame.Visible = false
            LocalFrame.Visible = true
        end
       
        Global.MouseButton1Down:Connect(OpenGlobalTab)
        Local.MouseButton1Down:Connect(OpenLocalTab)
        Global.MouseButton1Click:Connect(OpenGlobalTab)
        Local.MouseButton1Click:Connect(OpenLocalTab)
       
        OpenLocalTab()
       
        --
       
        local function GetPlayerDistance(Sender)
            if Sender.Character and Sender.Character:FindFirstChild("Head") then
                return math.floor((Sender.Character.Head.Position - Camera:GetRenderCFrame().p).Magnitude + 0.5)
            end
        end
       
        local function NewGlobal(Message, Sender, Color)
            local Frame = Template:Clone()
           
            Frame.Text = ("[%s]: %s"):format(Sender.Name, Message)
            Frame.User.Text = ("[%s]:"):format(Sender.Name)
            Frame.User.TextColor3 = Color
            Frame.BackgroundColor3 = Color
            Frame.Parent = GlobalFrame
           
            delay(60, function()
                Frame:Destroy()
            end)
        end
       
        local function NewLocal(Message, Sender, Color, Dist)
            local Frame = Template:Clone()
           
            Frame.Text = ("(%s) [%s]: %s"):format(tostring(Dist), Sender.Name, Message)
            Frame.User.Text = ("(%s) [%s]:"):format(tostring(Dist), Sender.Name)
            Frame.User.TextColor3 = Color
            Frame.BackgroundColor3 = Color
            Frame.Parent = LocalFrame
           
            delay(60, function()
                Frame:Destroy()
            end)
        end
       
        local function OnNewChat(Message, Sender, Color)
            if not ChatHUD or not ChatHUD.Parent then return end
           
            NewGlobal(Message, Sender, Color)
           
            local Distance = GetPlayerDistance(Sender)
           
            if Distance and Distance <= ChatLocalRange then
                NewLocal(Message, Sender, Color, Distance)
            end
        end
       
        local function OnPlayerAdded(Player)
            if not ChatHUD or not ChatHUD.Parent then return end
           
            local Color = BrickColor.Random().Color
           
            Player.Chatted:Connect(function(Message)
                OnNewChat(Message, Player, Color)
            end)
        end
       
        Players.PlayerAdded:Connect(OnPlayerAdded)
       
        for _, Player in pairs(Players:GetPlayers()) do
            OnPlayerAdded(Player)
        end
       
        --
       
        local ChatPart = ChatHUD.Part
       
        ChatHUD.Adornee = ChatPart
       
        if VRReady then
            ChatHUD.Parent = game:GetService("CoreGui")
            ChatHUD.Enabled = true
            ChatHUD.AlwaysOnTop = true
           
            local OnInput = UserInputService.InputBegan:Connect(function(Input, Processed)
                if not Processed then
                    if Input.KeyCode == Enum.KeyCode.ButtonX then
                        ChatHUD.Enabled = not ChatHUD.Enabled
                    end
                end
            end)
           
            local RenderStepped = RunService.RenderStepped:Connect(function()
                local LeftHand = VRService:GetUserCFrame(Enum.UserCFrame.LeftHand)
               
                ChatPart.CFrame = Camera.CFrame * LeftHand
            end)
           
            local CharacterAdded
           
            CharacterAdded = Client.CharacterAdded:Connect(function()
                OnInput:Disconnect()
                RenderStepped:Disconnect()
                CharacterAdded:Disconnect()
               
                ChatHUD:Destroy()
                ChatHUD = nil
            end)
        end
       
        wait(9e9)
    end;
     
    ViewHUDFunc = function()
        --[[
            Variables
        --]]
       
        local ViewportRange = ViewportRange or 32
       
        local UserInputService = game:GetService("UserInputService")
        local RunService = game:GetService("RunService")
       
        local VRService = game:GetService("VRService")
         local VRReady = VRService.VREnabled
       
        local Players = game:GetService("Players")
         local Client = Players.LocalPlayer
          local Mouse = Client:GetMouse()
       
        local Camera = workspace.CurrentCamera
         local CameraPort = Camera.CFrame
       
        local ViewHUD = script:FindFirstChild("ViewHUD") or game:GetObjects("rbxassetid://4480405425")[1]
         local Viewport = ViewHUD.Viewport
          local Viewcam = Instance.new("Camera")
         local ViewPart = ViewHUD.Part
       
        ViewHUD.Parent = game:GetService("CoreGui")
       
        Viewcam.Parent = Viewport
        Viewcam.CameraType = Enum.CameraType.Scriptable
        Viewport.CurrentCamera = Viewcam
        Viewport.BackgroundTransparency = 1
       
        --[[
            Code
        --]]
       
        local function Clone(Character)
            local Arc = Character.Archivable
            local Clone;
           
            Character.Archivable = true
            Clone = Character:Clone()
            Character.Archivable = Arc
           
            return Clone
        end
       
        local function GetPart(Name, Parent, Descendants)
            for i = 1, #Descendants do
                local Part = Descendants[i]
               
                if Part.Name == Name and Part.Parent.Name == Parent then
                    return Part
                end
            end
        end
       
        local function OnPlayerAdded(Player)
            if not ViewHUD or not ViewHUD.Parent then return end
           
            local function CharacterAdded(Character)
                if not ViewHUD or not ViewHUD.Parent then return end
               
                Character:WaitForChild("Head")
                Character:WaitForChild("Humanoid")
               
                wait(3)
               
                local FakeChar = Clone(Character)
                local Root = FakeChar:FindFirstChild("HumanoidRootPart") or FakeChar:FindFirstChild("Head")
                local RenderConnection;
               
                local Descendants = FakeChar:GetDescendants()
                local RealDescendants = Character:GetDescendants()
                local Correspondents = {};
               
                FakeChar.Humanoid.DisplayDistanceType = "None"
               
                for i = 1, #Descendants do
                    local Part = Descendants[i]
                    local Real = Part:IsA("BasePart") and GetPart(Part.Name, Part.Parent.Name, RealDescendants)
                   
                    if Part:IsA("BasePart") and Real then
                        Part.Anchored = true
                        Part:BreakJoints()
                       
                        if Part.Parent:IsA("Accessory") then
                            Part.Transparency = 0
                        end
                       
                        table.insert(Correspondents, {Part, Real})
                    end
                end
               
                RenderConnection = RunService.RenderStepped:Connect(function()
                    if not Character or not Character.Parent then
                        RenderConnection:Disconnect()
                        FakeChar:Destroy()
                       
                        return
                    end
                   
                    if (Root and (Root.Position - Camera.CFrame.p).Magnitude <= ViewportRange) or Player == Client or not Root then
                        for i = 1, #Correspondents do
                            local Part, Real = unpack(Correspondents[i])
                           
                            if Part and Real and Part.Parent and Real.Parent then
                                Part.CFrame = Real.CFrame
                            elseif Part.Parent and not Real.Parent then
                                Part:Destroy()
                            end
                        end
                    end
                end)
               
                FakeChar.Parent = Viewcam
            end
           
            Player.CharacterAdded:Connect(CharacterAdded)
           
            if Player.Character then
                spawn(function()
                    CharacterAdded(Player.Character)
                end)
            end
        end
       
        local PlayerAdded = Players.PlayerAdded:Connect(OnPlayerAdded)
       
        for _, Player in pairs(Players:GetPlayers()) do
            OnPlayerAdded(Player)
        end
       
        ViewPart.Size = Vector3.new()
       
        if VRReady then
            Viewport.Position = UDim2.new(.62, 0, .89, 0)
            Viewport.Size = UDim2.new(.3, 0, .3, 0)
            Viewport.AnchorPoint = Vector2.new(.5, 1)
        else
            Viewport.Size = UDim2.new(0.3, 0, 0.3, 0)
        end
       
        local RenderStepped = RunService.RenderStepped:Connect(function()
            local Render = Camera.CFrame
            local Scale = Camera.ViewportSize
           
            if VRReady then
                Render = Render * VRService:GetUserCFrame(Enum.UserCFrame.Head)
            end
           
            CameraPort = CFrame.new(Render.p + Vector3.new(5, 2, 0), Render.p)
           
            Viewport.Camera.CFrame = CameraPort
           
            ViewPart.CFrame = Render * CFrame.new(0, 0, -16)
           
            ViewHUD.Size = UDim2.new(0, Scale.X - 6, 0, Scale.Y - 6)
        end)
           
        --
       
        local CharacterAdded
       
        CharacterAdded = Client.CharacterAdded:Connect(function()
            RenderStepped:Disconnect()
            CharacterAdded:Disconnect()
            PlayerAdded:Disconnect()
           
            ViewHUD:Destroy()
            ViewHUD = nil
        end)
       
        wait(9e9)
    end;
     
    Script()
     
    wait(9e9)("Clicked")
    end)


    section1:addButton("FE Punches", function()
        --[[
    Synapse Xen v1.1.2 by Synapse GP
    VM Hash: 1e3762e7666678e8e7b3c8750ffc28fb03a426572a6e1595932511dda5cfaa80
]]

local SynapseXen_IlllIiIlI=select;local SynapseXen_ilIlliiIIllIlIi=string.byte;local SynapseXen_lIlIiil=string.sub;local SynapseXen_iiiiIilIIii=string.char;local SynapseXen_iIiilIiIllIlilII=type;local SynapseXen_lIllIiIllIIIIiii=table.concat;local unpack=unpack;local setmetatable=setmetatable;local pcall=pcall;local SynapseXen_lillIIili,SynapseXen_lliIiIilIii,SynapseXen_iiIIlliIIIiliil,SynapseXen_IiiilIIIIiIIIlI;if bit and bit.bxor then SynapseXen_lillIIili=bit.bxor;SynapseXen_lliIiIilIii=function(SynapseXen_lilIIlil,SynapseXen_lIlIlliliIiIiI)local SynapseXen_IIIliIlilIIIllIiI=SynapseXen_lillIIili(SynapseXen_lilIIlil,SynapseXen_lIlIlliliIiIiI)if SynapseXen_IIIliIlilIIIllIiI<0 then SynapseXen_IIIliIlilIIIllIiI=4294967296+SynapseXen_IIIliIlilIIIllIiI end;return SynapseXen_IIIliIlilIIIllIiI end else SynapseXen_lillIIili=function(SynapseXen_lilIIlil,SynapseXen_lIlIlliliIiIiI)local SynapseXen_iIiiIIiIli=function(SynapseXen_ililli,SynapseXen_lIIiI)return SynapseXen_ililli%(SynapseXen_lIIiI*2)>=SynapseXen_lIIiI end;local SynapseXen_lIIilliiliIIlI=0;for SynapseXen_IIillIiiiiIi=0,31 do SynapseXen_lIIilliiliIIlI=SynapseXen_lIIilliiliIIlI+(SynapseXen_iIiiIIiIli(SynapseXen_lilIIlil,2^SynapseXen_IIillIiiiiIi)~=SynapseXen_iIiiIIiIli(SynapseXen_lIlIlliliIiIiI,2^SynapseXen_IIillIiiiiIi)and 2^SynapseXen_IIillIiiiiIi or 0)end;return SynapseXen_lIIilliiliIIlI end;SynapseXen_lliIiIilIii=SynapseXen_lillIIili end;SynapseXen_iiIIlliIIIiliil=function(SynapseXen_IlIlllllIIIIlIilii,SynapseXen_llliIiiIii,SynapseXen_IIilIIIlliiiIliI)return(SynapseXen_IlIlllllIIIIlIilii+SynapseXen_llliIiiIii)%SynapseXen_IIilIIIlliiiIliI end;SynapseXen_IiiilIIIIiIIIlI=function(SynapseXen_IlIlllllIIIIlIilii,SynapseXen_llliIiiIii,SynapseXen_IIilIIIlliiiIliI)return(SynapseXen_IlIlllllIIIIlIilii-SynapseXen_llliIiiIii)%SynapseXen_IIilIIIlliiiIliI end;local function SynapseXen_iIiilli(SynapseXen_IIIliIlilIIIllIiI)if SynapseXen_IIIliIlilIIIllIiI<0 then SynapseXen_IIIliIlilIIIllIiI=4294967296+SynapseXen_IIIliIlilIIIllIiI end;return SynapseXen_IIIliIlilIIIllIiI end;local getfenv=getfenv;if not getfenv then getfenv=function()return _ENV end end;local SynapseXen_iIilIIIlllIIiIiiI={}local SynapseXen_IIliiIliIl={}local SynapseXen_IIiIllllliI;local SynapseXen_IliIIlIIlIlIlI;local SynapseXen_lllIiIIIil={}local SynapseXen_lliiililiIi={}for SynapseXen_IIillIiiiiIi=0,255 do local SynapseXen_lliIIlIli,SynapseXen_llllIIilIIIli=SynapseXen_iiiiIilIIii(SynapseXen_IIillIiiiiIi),SynapseXen_iiiiIilIIii(SynapseXen_IIillIiiiiIi,0)SynapseXen_lllIiIIIil[SynapseXen_lliIIlIli]=SynapseXen_llllIIilIIIli;SynapseXen_lliiililiIi[SynapseXen_llllIIilIIIli]=SynapseXen_lliIIlIli end;local function SynapseXen_iiIIlIiIIliIi(SynapseXen_llIlliliI,SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll)if SynapseXen_IIlIiIiiillIlI>=256 then SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll=0,SynapseXen_iIlllliIlIll+1;if SynapseXen_iIlllliIlIll>=256 then SynapseXen_iilIlliI={}SynapseXen_iIlllliIlIll=1 end end;SynapseXen_iilIlliI[SynapseXen_iiiiIilIIii(SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll)]=SynapseXen_llIlliliI;SynapseXen_IIlIiIiiillIlI=SynapseXen_IIlIiIiiillIlI+1;return SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll end;local function SynapseXen_ilIIl(SynapseXen_iliilIlliII)local function SynapseXen_iIiiliiiIIlIlll(SynapseXen_IIlIIlIi)local SynapseXen_iIlllliIlIll='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'SynapseXen_IIlIIlIi=string.gsub(SynapseXen_IIlIIlIi,'[^'..SynapseXen_iIlllliIlIll..'=]','')return SynapseXen_IIlIIlIi:gsub('.',function(SynapseXen_IlIlllllIIIIlIilii)if SynapseXen_IlIlllllIIIIlIilii=='='then return''end;local SynapseXen_iiiIIIlIIIillllIlIi,SynapseXen_lIiIIlll='',SynapseXen_iIlllliIlIll:find(SynapseXen_IlIlllllIIIIlIilii)-1;for SynapseXen_IIillIiiiiIi=6,1,-1 do SynapseXen_iiiIIIlIIIillllIlIi=SynapseXen_iiiIIIlIIIillllIlIi..(SynapseXen_lIiIIlll%2^SynapseXen_IIillIiiiiIi-SynapseXen_lIiIIlll%2^(SynapseXen_IIillIiiiiIi-1)>0 and'1'or'0')end;return SynapseXen_iiiIIIlIIIillllIlIi end):gsub('%d%d%d?%d?%d?%d?%d?%d?',function(SynapseXen_IlIlllllIIIIlIilii)if#SynapseXen_IlIlllllIIIIlIilii~=8 then return''end;local SynapseXen_lIlIllll=0;for SynapseXen_IIillIiiiiIi=1,8 do SynapseXen_lIlIllll=SynapseXen_lIlIllll+(SynapseXen_IlIlllllIIIIlIilii:sub(SynapseXen_IIillIiiiiIi,SynapseXen_IIillIiiiiIi)=='1'and 2^(8-SynapseXen_IIillIiiiiIi)or 0)end;return string.char(SynapseXen_lIlIllll)end)end;SynapseXen_iliilIlliII=SynapseXen_iIiiliiiIIlIlll(SynapseXen_iliilIlliII)local SynapseXen_lilIiiiiiiiIlllI=SynapseXen_lIlIiil(SynapseXen_iliilIlliII,1,1)if SynapseXen_lilIiiiiiiiIlllI=="u"then return SynapseXen_lIlIiil(SynapseXen_iliilIlliII,2)elseif SynapseXen_lilIiiiiiiiIlllI~="c"then error("Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")end;SynapseXen_iliilIlliII=SynapseXen_lIlIiil(SynapseXen_iliilIlliII,2)local SynapseXen_llIllIllIiiIiiIilII=#SynapseXen_iliilIlliII;local SynapseXen_iilIlliI={}local SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll=0,1;local SynapseXen_lIIllllilliII={}local SynapseXen_IIIliIlilIIIllIiI=1;local SynapseXen_illIiliiII=SynapseXen_lIlIiil(SynapseXen_iliilIlliII,1,2)SynapseXen_lIIllllilliII[SynapseXen_IIIliIlilIIIllIiI]=SynapseXen_lliiililiIi[SynapseXen_illIiliiII]or SynapseXen_iilIlliI[SynapseXen_illIiliiII]SynapseXen_IIIliIlilIIIllIiI=SynapseXen_IIIliIlilIIIllIiI+1;for SynapseXen_IIillIiiiiIi=3,SynapseXen_llIllIllIiiIiiIilII,2 do local SynapseXen_liiilllIiIiIlii=SynapseXen_lIlIiil(SynapseXen_iliilIlliII,SynapseXen_IIillIiiiiIi,SynapseXen_IIillIiiiiIi+1)local SynapseXen_liiliII=SynapseXen_lliiililiIi[SynapseXen_illIiliiII]or SynapseXen_iilIlliI[SynapseXen_illIiliiII]if not SynapseXen_liiliII then error("Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")end;local SynapseXen_liIlIlIliIi=SynapseXen_lliiililiIi[SynapseXen_liiilllIiIiIlii]or SynapseXen_iilIlliI[SynapseXen_liiilllIiIiIlii]if SynapseXen_liIlIlIliIi then SynapseXen_lIIllllilliII[SynapseXen_IIIliIlilIIIllIiI]=SynapseXen_liIlIlIliIi;SynapseXen_IIIliIlilIIIllIiI=SynapseXen_IIIliIlilIIIllIiI+1;SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll=SynapseXen_iiIIlIiIIliIi(SynapseXen_liiliII..SynapseXen_lIlIiil(SynapseXen_liIlIlIliIi,1,1),SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll)else local SynapseXen_ilillIliIIllil=SynapseXen_liiliII..SynapseXen_lIlIiil(SynapseXen_liiliII,1,1)SynapseXen_lIIllllilliII[SynapseXen_IIIliIlilIIIllIiI]=SynapseXen_ilillIliIIllil;SynapseXen_IIIliIlilIIIllIiI=SynapseXen_IIIliIlilIIIllIiI+1;SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll=SynapseXen_iiIIlIiIIliIi(SynapseXen_ilillIliIIllil,SynapseXen_iilIlliI,SynapseXen_IIlIiIiiillIlI,SynapseXen_iIlllliIlIll)end;SynapseXen_illIiliiII=SynapseXen_liiilllIiIiIlii end;return SynapseXen_lIllIiIllIIIIiii(SynapseXen_lIIllllilliII)end;local function SynapseXen_IIIlliIllli(SynapseXen_IIlIl,SynapseXen_IIIIllllll,SynapseXen_lIIIIIlliIIIliIIlI)if SynapseXen_lIIIIIlliIIIliIIlI then local SynapseXen_lillIIill=SynapseXen_IIlIl/2^(SynapseXen_IIIIllllll-1)%2^(SynapseXen_lIIIIIlliIIIliIIlI-1-(SynapseXen_IIIIllllll-1)+1)return SynapseXen_lillIIill-SynapseXen_lillIIill%1 else local SynapseXen_lilllIiIiill=2^(SynapseXen_IIIIllllll-1)if SynapseXen_IIlIl%(SynapseXen_lilllIiIiill+SynapseXen_lilllIiIiill)>=SynapseXen_lilllIiIiill then return 1 else return 0 end end end;local function SynapseXen_IiiIlilIililI()local SynapseXen_ilIilIlIlilii=SynapseXen_lillIIili(284364308,SynapseXen_IliIIlIIlIlIlI)while true do if SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(284404033,SynapseXen_IliIIlIIlIlIlI)then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi+34874,SynapseXen_IiiiiiIlIliilillil-41753)-SynapseXen_lillIIili(3981732499,SynapseXen_IIliiIliIl[3])end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii-SynapseXen_lillIIili(2847761747,SynapseXen_IliIIlIIlIlIlI)elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(284405531,SynapseXen_IliIIlIIlIlIlI)then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi+22024,SynapseXen_IiiiiiIlIliilillil+26736)-SynapseXen_lillIIili(2847751205,SynapseXen_IliIIlIIlIlIlI)end;SynapseXen_ilIilIlIlilii=SynapseXen_lillIIili(SynapseXen_ilIilIlIlilii,SynapseXen_lillIIili(2518682266,SynapseXen_IliIIlIIlIlIlI))elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(2395142449,SynapseXen_IIliiIliIl[12])then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi+49477,SynapseXen_IiiiiiIlIliilillil+19239)-SynapseXen_lillIIili(2847736413,SynapseXen_IliIIlIIlIlIlI)end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(135359769,SynapseXen_IIliiIliIl[12])elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(795419322,SynapseXen_IliIIlIIlIlIlI)then return elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(4093999313,SynapseXen_IIliiIliIl[9])then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi-5405,SynapseXen_IiiiiiIlIliilillil-45347)+SynapseXen_lillIIili(680601543,SynapseXen_IIliiIliIl[5])end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(2352500206,SynapseXen_IIliiIliIl[13])elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(284408934,SynapseXen_IliIIlIIlIlIlI)then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi-27275,SynapseXen_IiiiiiIlIliilillil+10717)+SynapseXen_lillIIili(256676943,SynapseXen_IIliiIliIl[11])end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(2847767341,SynapseXen_IliIIlIIlIlIlI)elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(795791881,SynapseXen_IliIIlIIlIlIlI)then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi-3118,SynapseXen_IiiiiiIlIliilillil-33791)+SynapseXen_lillIIili(1296815834,SynapseXen_IIliiIliIl[9])end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(848689883,SynapseXen_IIliiIliIl[1])elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(284364308,SynapseXen_IliIIlIIlIlIlI)then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi-26550,SynapseXen_IiiiiiIlIliilillil+30778)-SynapseXen_lillIIili(2847739002,SynapseXen_IliIIlIIlIlIlI)end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(2847759667,SynapseXen_IliIIlIIlIlIlI)elseif SynapseXen_ilIilIlIlilii==SynapseXen_lillIIili(426191173,SynapseXen_IIliiIliIl[7])then SynapseXen_IIiIllllliI=function(SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil)return SynapseXen_lillIIili(SynapseXen_IliIIilIlIIIilliiIi+5824,SynapseXen_IiiiiiIlIliilillil+47369)+SynapseXen_lillIIili(2687086905,SynapseXen_IIliiIliIl[7])end;SynapseXen_ilIilIlIlilii=SynapseXen_ilIilIlIlilii+SynapseXen_lillIIili(2847768473,SynapseXen_IliIIlIIlIlIlI)end end end;local function SynapseXen_IlliI(SynapseXen_illIlliIliili)local SynapseXen_IIiiillIiIiIil=1;local SynapseXen_iiIilllliiiIiIlIli;local SynapseXen_IliillI;local function SynapseXen_lIllIliIliIIiiii()local SynapseXen_iilIl=SynapseXen_ilIlliiIIllIlIi(SynapseXen_illIlliIliili,SynapseXen_IIiiillIiIiIil,SynapseXen_IIiiillIiIiIil)SynapseXen_IIiiillIiIiIil=SynapseXen_IIiiillIiIiIil+1;return SynapseXen_iilIl end;local function SynapseXen_IliiIIlIlliiilliiii()local SynapseXen_iiIlililIi,SynapseXen_IliIIilIlIIIilliiIi,SynapseXen_IiiiiiIlIliilillil,SynapseXen_lIillIlilIl=SynapseXen_ilIlliiIIllIlIi(SynapseXen_illIlliIliili,SynapseXen_IIiiillIiIiIil,SynapseXen_IIiiillIiIiIil+3)SynapseXen_IIiiillIiIiIil=SynapseXen_IIiiillIiIiIil+4;return SynapseXen_lIillIlilIl*16777216+SynapseXen_IiiiiiIlIliilillil*65536+SynapseXen_IliIIilIlIIIilliiIi*256+SynapseXen_iiIlililIi end;local function SynapseXen_iiiIIIlllli()return SynapseXen_IliiIIlIlliiilliiii()*4294967296+SynapseXen_IliiIIlIlliiilliiii()end;local function SynapseXen_IIiliIi()local SynapseXen_IlllIiilillliIli=SynapseXen_lliIiIilIii(SynapseXen_IliiIIlIlliiilliiii(),SynapseXen_iIilIIIlllIIiIiiI[1440126736]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="pain is gonna use the backspace method on xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1343219140,2349585778)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3278919165,3278990276)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1440126736]=SynapseXen_lillIIili(SynapseXen_lillIIili(2543734118,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1137735963,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2334337529,2884819927,293762280,1321717865,817141670,2602898351,1985085227,2970970230}return SynapseXen_iIilIIIlllIIiIiiI[1440126736]end)(3889))local SynapseXen_IllIiIlIiliIIIlIIII=SynapseXen_lliIiIilIii(SynapseXen_IliiIIlIlliiilliiii(),SynapseXen_iIilIIIlllIIiIiiI[3211022925]or(function()local SynapseXen_IlIlllllIIIIlIilii="level 1 crook = luraph, level 100 boss = xen"SynapseXen_iIilIIIlllIIiIiiI[3211022925]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3303077598,2576428723),SynapseXen_lillIIili(1280567973,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2826127048,2641967372,502920086,4196071680}return SynapseXen_iIilIIIlllIIiIiiI[3211022925]end)())local SynapseXen_liIIli=1;local SynapseXen_llIilII=SynapseXen_IIIlliIllli(SynapseXen_IllIiIlIiliIIIlIIII,1,20)*2^32+SynapseXen_IlllIiilillliIli;local SynapseXen_IliiiIlIiili=SynapseXen_IIIlliIllli(SynapseXen_IllIiIlIiliIIIlIIII,21,31)local SynapseXen_liIilillIiIIIIill=(-1)^SynapseXen_IIIlliIllli(SynapseXen_IllIiIlIiliIIIlIIII,32)if SynapseXen_IliiiIlIiili==0 then if SynapseXen_llIilII==0 then return SynapseXen_liIilillIiIIIIill*0 else SynapseXen_IliiiIlIiili=1;SynapseXen_liIIli=0 end elseif SynapseXen_IliiiIlIiili==2047 then if SynapseXen_llIilII==0 then return SynapseXen_liIilillIiIIIIill*1/0 else return SynapseXen_liIilillIiIIIIill*0/0 end end;return math.ldexp(SynapseXen_liIilillIiIIIIill,SynapseXen_IliiiIlIiili-1023)*(SynapseXen_liIIli+SynapseXen_llIilII/2^52)end;local function SynapseXen_lliiilliilIillIl(SynapseXen_ilIIi)local SynapseXen_llilI;if SynapseXen_ilIIi then SynapseXen_llilI=SynapseXen_lIlIiil(SynapseXen_illIlliIliili,SynapseXen_IIiiillIiIiIil,SynapseXen_IIiiillIiIiIil+SynapseXen_ilIIi-1)SynapseXen_IIiiillIiIiIil=SynapseXen_IIiiillIiIiIil+SynapseXen_ilIIi else SynapseXen_ilIIi=SynapseXen_iiIilllliiiIiIlIli()if SynapseXen_ilIIi==0 then return""end;SynapseXen_llilI=SynapseXen_lIlIiil(SynapseXen_illIlliIliili,SynapseXen_IIiiillIiIiIil,SynapseXen_IIiiillIiIiIil+SynapseXen_ilIIi-1)SynapseXen_IIiiillIiIiIil=SynapseXen_IIiiillIiIiIil+SynapseXen_ilIIi end;return SynapseXen_llilI end;local function SynapseXen_IlIIiIIlIliilI(SynapseXen_llilI)local SynapseXen_lillIIill={}for SynapseXen_IIillIiiiiIi=1,#SynapseXen_llilI do local SynapseXen_lllil=SynapseXen_llilI:sub(SynapseXen_IIillIiiiiIi,SynapseXen_IIillIiiiiIi)SynapseXen_lillIIill[#SynapseXen_lillIIill+1]=string.char(SynapseXen_lillIIili(string.byte(SynapseXen_lllil),SynapseXen_iIilIIIlllIIiIiiI[721508056]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi xen crashes on my axon paste plz help"SynapseXen_iIilIIIlllIIiIiiI[721508056]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(824479739,3911864817),SynapseXen_lillIIili(1119737800,SynapseXen_IIliiIliIl[8]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2702924692,2265305070,2925851808,1242549131,3390075396}return SynapseXen_iIilIIIlllIIiIiiI[721508056]end)()))end;return table.concat(SynapseXen_lillIIill)end;local function SynapseXen_IlllliiIlIiiIiillli()local SynapseXen_iIllilIiiIiIiIIIii={}local SynapseXen_IIilIIiiIIIiiliIil={}local SynapseXen_IiiIiii={}local SynapseXen_lIiIlIIilIIilililll={[SynapseXen_iIilIIIlllIIiIiiI[2212407899]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2396639053,3595037753)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3308081993,3308155260)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2212407899]=SynapseXen_lillIIili(SynapseXen_lillIIili(2339313654,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(597171454,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1394972345,2998539617,1137961080,4057727064}return SynapseXen_iIilIIIlllIIiIiiI[2212407899]end)({},"ilIllliiIl","il",{},"iIliilIilliilIIll","lIiillIiIiIillI","lilllliIIIIiIIl",12564)]=SynapseXen_iIllilIiiIiIiIIIii,[SynapseXen_iIilIIIlllIIiIiiI[1922386785]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"SynapseXen_iIilIIIlllIIiIiiI[1922386785]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(473211507,4251830984),SynapseXen_lillIIili(1714242037,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1361715766,325687812,3200677552}return SynapseXen_iIilIIIlllIIiIiiI[1922386785]end)()]=SynapseXen_IIilIIiiIIIiiliIil,[SynapseXen_iIilIIIlllIIiIiiI[2162822524]or(function()local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"SynapseXen_iIilIIIlllIIiIiiI[2162822524]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3356861403,1494227722),SynapseXen_lillIIili(1110672938,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1430253386,3056899946,3905974235,366285879}return SynapseXen_iIilIIIlllIIiIiiI[2162822524]end)()]=SynapseXen_IiiIiii}SynapseXen_IliiIIlIlliiilliiii()for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lillIIili(SynapseXen_IliillI(),SynapseXen_iIilIIIlllIIiIiiI[282356216]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen detects custom getfenv"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2090085135,4212669103)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1033743266,1033806255)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[282356216]=SynapseXen_lillIIili(SynapseXen_lillIIili(641516807,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1888434605,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{41977087}return SynapseXen_iIilIIIlllIIiIiiI[282356216]end)({},{},8649,{},"IiIiIiiliIl","illIl"))do local SynapseXen_iIiilIiIllIlilII=SynapseXen_lIllIliIliIIiiii()local SynapseXen_iIlliiIliilIllIlIiii;if SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[2052662236]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"SynapseXen_iIilIIIlllIIiIiiI[2052662236]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1675364265,1271116335),SynapseXen_lillIIili(2174994813,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{591639320,2644320752,3891782733,2957588907,3604658163,394627368}return SynapseXen_iIilIIIlllIIiIiiI[2052662236]end)())then SynapseXen_iIlliiIliilIllIlIiii=SynapseXen_lIllIliIliIIiiii()~=0 elseif SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[2524334387]or(function()local SynapseXen_IlIlllllIIIIlIilii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."SynapseXen_iIilIIIlllIIiIiiI[2524334387]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1847833682,2492438872),SynapseXen_lillIIili(3811137750,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2063882560,3095316416,2189512107,2648769554,3243308939}return SynapseXen_iIilIIIlllIIiIiiI[2524334387]end)())then SynapseXen_iIlliiIliilIllIlIiii=SynapseXen_IIiliIi()elseif SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[4135277827]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="pain exist is gonna connect the dots of xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(846500174,4054256408)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1822998072,1823060421)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4135277827]=SynapseXen_lillIIili(SynapseXen_lillIIili(504842215,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1954218057,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2298031181,4126522810,4283943206,3535639327,3724955446}return SynapseXen_iIilIIIlllIIiIiiI[4135277827]end)(4325,{},{},8352,"ililIliIliIliIlllii","IliiIlI",4528))then SynapseXen_iIlliiIliilIllIlIiii=SynapseXen_lIlIiil(SynapseXen_IlIIiIIlIliilI(SynapseXen_lliiilliilIillIl()),1,-2)end;SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_llllIiIIlliIilIlIl-1]=SynapseXen_iIlliiIliilIllIlIiii end;SynapseXen_IliiIIlIlliiilliiii()SynapseXen_lIiIlIIilIIilililll[982698750]=SynapseXen_lillIIili(SynapseXen_lIllIliIliIIiiii(),SynapseXen_iIilIIIlllIIiIiiI[2561653520]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="imagine using some lua minifier tool and thinking you are a badass"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(291489342,3618134116)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4038984417,4038992222)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2561653520]=SynapseXen_lillIIili(SynapseXen_lillIIili(742933861,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1127390714,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4234956687,3371416095,1740518164,3321787374,2268843585,541777922,1407030107}return SynapseXen_iIilIIIlllIIiIiiI[2561653520]end)("lIlIIIilIllIiil","IIIilIiIllIl"))SynapseXen_lIiIlIIilIIilililll[1481819974]=SynapseXen_lillIIili(SynapseXen_lIllIliIliIIiiii(),SynapseXen_iIilIIIlllIIiIiiI[240322654]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3979630598,3117893529)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1603159501,1603228816)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[240322654]=SynapseXen_lillIIili(SynapseXen_lillIIili(448370884,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3890619272,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1129592574,543698809,2898909455,2347671769,3429198049}return SynapseXen_iIilIIIlllIIiIiiI[240322654]end)({},{},{},{},"iIIIiiii",{},2111))SynapseXen_lIllIliIliIIiiii()SynapseXen_lIllIliIliIIiiii()for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lillIIili(SynapseXen_IliillI(),SynapseXen_iIilIIIlllIIiIiiI[3412562986]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi devforum"SynapseXen_iIilIIIlllIIiIiiI[3412562986]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3843295250,28874054),SynapseXen_lillIIili(1069920867,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2077755263}return SynapseXen_iIilIIIlllIIiIiiI[3412562986]end)())do SynapseXen_IliiIIlIlliiilliiii()local SynapseXen_iiliiiliiiiIiIIilI=SynapseXen_lillIIili(SynapseXen_IliiIIlIlliiilliiii(),SynapseXen_iIilIIIlllIIiIiiI[667672391]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="inb4 posted on exploit reports section"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2911019330,669009576)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2160258499,2160298656)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[667672391]=SynapseXen_lillIIili(SynapseXen_lillIIili(2272293244,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1575376223,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1623494557}return SynapseXen_iIilIIIlllIIiIiiI[667672391]end)("iliiiliilll",{},{},{},11784))local SynapseXen_IIiilliIIliiiliI=SynapseXen_lIllIliIliIIiiii()local SynapseXen_iIiilIiIllIlilII=SynapseXen_lIllIliIliIIiiii()SynapseXen_IliiIIlIlliiilliiii()local SynapseXen_iIIiillIiIIililIiiI={[638088808]=SynapseXen_iiliiiliiiiIiIIilI,[1902504146]=SynapseXen_IIiilliIIliiiliI,[1968668134]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,1,6),[1997878473]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,7,14)}if SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[1998549184]or(function()local SynapseXen_IlIlllllIIIIlIilii="sponsored by ironbrew, jk xen is better"SynapseXen_iIilIIIlllIIiIiiI[1998549184]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3521300775,2085958405),SynapseXen_lillIIili(877588070,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{238252679,3806879448}return SynapseXen_iIilIIIlllIIiIiiI[1998549184]end)())then SynapseXen_iIIiillIiIIililIiiI[31674126]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,24,32)SynapseXen_iIIiillIiIIililIiiI[233588846]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,15,23)elseif SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[2543233633]or(function()local SynapseXen_IlIlllllIIIIlIilii="sometimes it be like that"SynapseXen_iIilIIIlllIIiIiiI[2543233633]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2570731749,3589049487),SynapseXen_lillIIili(3848939723,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3901229943,1132148614,506048410,2945221225,3053043914}return SynapseXen_iIilIIIlllIIiIiiI[2543233633]end)())then SynapseXen_iIIiillIiIIililIiiI[518845294]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,15,32)elseif SynapseXen_iIiilIiIllIlilII==(SynapseXen_iIilIIIlllIIiIiiI[619595679]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="wait for someone on devforum to say they are gonna deobfuscate this"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1056243955,1263028247)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3792209668,3792272745)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[619595679]=SynapseXen_lillIIili(SynapseXen_lillIIili(3065241080,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1790065835,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1960274165,3034191123,2970112628,1877519216,793471082,2137585967,326989819,85689030}return SynapseXen_iIilIIIlllIIiIiiI[619595679]end)(14826,{},"iIIlIililIlliIilI"))then SynapseXen_iIIiillIiIIililIiiI[1050029882]=SynapseXen_IIIlliIllli(SynapseXen_iiliiiliiiiIiIIilI,15,32)-131071 end;SynapseXen_IiiIiii[SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_iIIiillIiIIililIiiI end;SynapseXen_IliiIIlIlliiilliiii()for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lillIIili(SynapseXen_IliillI(),SynapseXen_iIilIIIlllIIiIiiI[168783539]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi my 2.5mb script doesn't work with xen please help"SynapseXen_iIilIIIlllIIiIiiI[168783539]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2198007548,1459610271),SynapseXen_lillIIili(3461167891,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3635145773,444313887,709418005,1493337136,4079348143,2979012919,3408049469,1887477170}return SynapseXen_iIilIIIlllIIiIiiI[168783539]end)())do SynapseXen_iIllilIiiIiIiIIIii[SynapseXen_llllIiIIlliIilIlIl-1]=SynapseXen_IlllliiIlIiiIiillli()end;return SynapseXen_lIiIlIIilIIilililll end;do assert(SynapseXen_lliiilliilIillIl(4)=="\27Xen","Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")SynapseXen_IliillI=SynapseXen_IliiIIlIlliiilliiii;SynapseXen_iiIilllliiiIiIlIli=SynapseXen_IliiIIlIlliiilliiii;local SynapseXen_iIIlilIl=SynapseXen_lliiilliilIillIl()SynapseXen_lIllIliIliIIiiii()SynapseXen_IliIIlIIlIlIlI=SynapseXen_iIiilli(SynapseXen_IliillI())SynapseXen_lIllIliIliIIiiii()SynapseXen_lIllIliIliIIiiii()local SynapseXen_iiliiIillIlliiIiIII=0;for SynapseXen_IIillIiiiiIi=1,#SynapseXen_iIIlilIl do local SynapseXen_lllil=SynapseXen_iIIlilIl:sub(SynapseXen_IIillIiiiiIi,SynapseXen_IIillIiiiiIi)SynapseXen_iiliiIillIlliiIiIII=SynapseXen_iiliiIillIlliiIiIII+string.byte(SynapseXen_lllil)end;SynapseXen_iiliiIillIlliiIiIII=SynapseXen_lillIIili(SynapseXen_iiliiIillIlliiIiIII,SynapseXen_IliIIlIIlIlIlI)for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lIllIliIliIIiiii()do SynapseXen_IIliiIliIl[SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_lliIiIilIii(SynapseXen_IliillI(),SynapseXen_iiliiIillIlliiIiIII)end;SynapseXen_IiiIlilIililI()end;return SynapseXen_IlllliiIlIiiIiillli()end;local function SynapseXen_IillIiIlIlIlIlill(...)return SynapseXen_IlllIiIlI('#',...),{...}end;local function SynapseXen_iliiiIllIiliiiI(SynapseXen_lIiIlIIilIIilililll,SynapseXen_iiillIIIlIi,SynapseXen_lIlIiiIiIi)local SynapseXen_IIilIIiiIIIiiliIil=SynapseXen_lIiIlIIilIIilililll[SynapseXen_iIilIIIlllIIiIiiI[1922386785]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"SynapseXen_iIilIIIlllIIiIiiI[1922386785]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(473211507,4251830984),SynapseXen_lillIIili(1714242037,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1361715766,325687812,3200677552}return SynapseXen_iIilIIIlllIIiIiiI[1922386785]end)()]local SynapseXen_iIllilIiiIiIiIIIii=SynapseXen_lIiIlIIilIIilililll[SynapseXen_iIilIIIlllIIiIiiI[2212407899]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2396639053,3595037753)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3308081993,3308155260)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2212407899]=SynapseXen_lillIIili(SynapseXen_lillIIili(2339313654,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(597171454,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1394972345,2998539617,1137961080,4057727064}return SynapseXen_iIilIIIlllIIiIiiI[2212407899]end)({},"ilIllliiIl","il",{},"iIliilIilliilIIll","lIiillIiIiIillI","lilllliIIIIiIIl",12564)]local SynapseXen_IiiIiii=SynapseXen_lIiIlIIilIIilililll[SynapseXen_iIilIIIlllIIiIiiI[2162822524]or(function()local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"SynapseXen_iIilIIIlllIIiIiiI[2162822524]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3356861403,1494227722),SynapseXen_lillIIili(1110672938,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1430253386,3056899946,3905974235,366285879}return SynapseXen_iIilIIIlllIIiIiiI[2162822524]end)()]return function(...)local SynapseXen_iilliIiiiliillIIIllI,SynapseXen_lllilIiIill=1,-1;local SynapseXen_liilIiiI,SynapseXen_IlliiilIIilIli={},SynapseXen_IlllIiIlI('#',...)-1;local SynapseXen_ilIIiIIIllIli=0;local SynapseXen_IlllliiIlillIl={}local SynapseXen_IilIIliiiliII={}local SynapseXen_IliliIlilllliIIlll=setmetatable({},{__index=SynapseXen_IlllliiIlillIl,__newindex=function(SynapseXen_lllllIIIiI,SynapseXen_iIIlilllIIi,SynapseXen_lIIilliiiIIl)if SynapseXen_iIIlilllIIi>SynapseXen_lllilIiIill then SynapseXen_lllilIiIill=SynapseXen_iIIlilllIIi end;SynapseXen_IlllliiIlillIl[SynapseXen_iIIlilllIIi]=SynapseXen_lIIilliiiIIl end})local function SynapseXen_iilIIllIlIiIlilIill()local SynapseXen_iIIiillIiIIililIiiI,SynapseXen_IIllilI;while true do SynapseXen_iIIiillIiIIililIiiI=SynapseXen_IiiIiii[SynapseXen_iilliIiiiliillIIIllI]SynapseXen_IIllilI=SynapseXen_iIIiillIiIIililIiiI[1902504146]SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1;if SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3323382624]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="luraph better then xen bros :pensive:"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1357483731,719659571)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3840401461,3840398871)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3323382624]=SynapseXen_lillIIili(SynapseXen_lillIIili(1151349454,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2534646601,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2543926762,633194745,877280197,347272298,128427580,3408604644,4176884456}return SynapseXen_iIilIIIlllIIiIiiI[3323382624]end)("iIiilillili","lIliiiilIllIli",{},{},{}))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2636291428]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2270146748,2817448326)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1091749430,1091754788)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2636291428]=SynapseXen_lillIIili(SynapseXen_lillIIili(2842951983,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(99826563,SynapseXen_IIliiIliIl[13]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3207669310}return SynapseXen_iIilIIIlllIIiIiiI[2636291428]end)("iiIiIi",14663,"IiiIlliIiIIIiIillli",{})),SynapseXen_ilIIiIIIllIli,256)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[1514833571]or(function()local SynapseXen_IlIlllllIIIIlIilii="yed"SynapseXen_iIilIIIlllIIiIiiI[1514833571]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(810968983,1160511776),SynapseXen_lillIIili(2104222600,SynapseXen_IIliiIliIl[12]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3821218837,2980962536,2720779423,493965678,2596239300,115492950,1605769109,2609828450,838389198}return SynapseXen_iIilIIIlllIIiIiiI[1514833571]end)()),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_lIIliiI=SynapseXen_lllIIiiIIIlIiIilii+2;local SynapseXen_IlIIIliIilliilIi={SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii](SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1],SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2])}for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lllil do SynapseXen_IliliIlilllliIIlll[SynapseXen_lIIliiI+SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_IlIIIliIilliilIi[SynapseXen_llllIiIIlliIilIlIl]end;if SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+3]~=nil then SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2]=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+3]else SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[582747307]or(function()local SynapseXen_IlIlllllIIIIlIilii="my way to go against expwoiting is to have safety measuwes. i 1 wocawscwipt and onwy moduwes. hewe's how it wowks: this scwipt bewow stowes the moduwes in a tabwe fow each moduwe we send the wist with the moduwes and moduwe infowmation and use inyit a function in my moduwe that wiww stowe the info and aftew it has send to aww the moduwes it wiww dewete them. so whenyevew the cwient twies to hack they cant get the moduwes. onwy this peace of wocawscwipt."SynapseXen_iIilIIIlllIIiIiiI[582747307]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3186171511,1249802084),SynapseXen_lillIIili(3994345531,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3475203112,3636505974,1028568718,744484078}return SynapseXen_iIilIIIlllIIiIiiI[582747307]end)())then SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3215801657]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="aspect network better obfuscator"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1187005826,2934876530)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1685407267,1685475892)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3215801657]=SynapseXen_lillIIili(SynapseXen_lillIIili(902393444,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3534414803,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2013218501,1132239633,360653943}return SynapseXen_iIilIIIlllIIiIiiI[3215801657]end)("iIIiI","l","IIIIllllIllliilIll",13991,"II","iIIiiiIl","II","llIIiIIiIlI"))]=SynapseXen_lIlIiiIiIi[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1490906989]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(830573,635323111)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2116010623,2116016341)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1490906989]=SynapseXen_lillIIili(SynapseXen_lillIIili(3588228925,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1502595879,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1106113210,2213262800,2008256359,2588576818,1876824609,4170372155,2725113000,3488228180}return SynapseXen_iIilIIIlllIIiIiiI[1490906989]end)(5642,4322,{},4064,8555,"lllIiI",1709,{},"liIlIllIilIIlilIlll"))]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3602171236]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi xen doesn't work on sk8r please help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1161788138,3884705540)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3722853718,3722843813)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3602171236]=SynapseXen_lillIIili(SynapseXen_lillIIili(1907539383,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1934624722,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1155241165,2200952290,3316238324,982748623}return SynapseXen_iIilIIIlllIIiIiiI[3602171236]end)({},{}))then SynapseXen_lIlIiiIiIi[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[67970848]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="yed"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2424588852,1914421032)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1890687459,1890746023)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[67970848]=SynapseXen_lillIIili(SynapseXen_lillIIili(193245667,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1084312964,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2475922042,1060174693,772678166}return SynapseXen_iIilIIIlllIIiIiiI[67970848]end)("Ii","IilllIi"),512)]=SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3527301812]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="skisploit is the superior obfuscator, clearly."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3688731992,3041248805)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1391147196,1391134515)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3527301812]=SynapseXen_lillIIili(SynapseXen_lillIIili(665631850,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1354125661,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3469236998,530603713,406071984,3193590438}return SynapseXen_iIilIIIlllIIiIiiI[3527301812]end)("iliIiliiIili",{},{},{},"IllIlililiIilliIlil",2848,13569,"lllllllIiIIiIIli"))]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4290032619]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3718316195,181985047)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1155645606,1155706417)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4290032619]=SynapseXen_lillIIili(SynapseXen_lillIIili(3723862104,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3889771866,SynapseXen_IIliiIliIl[3]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1532391290,2263579254,2888166956,724063910,2563649356,718308909}return SynapseXen_iIilIIIlllIIiIiiI[4290032619]end)(6836,"I",{}))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2339221661]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3553243390,476729831)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(625644557,625709846)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2339221661]=SynapseXen_lillIIili(SynapseXen_lillIIili(3376069444,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2762082229,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{281266306,3721393341,3908704175,1618776047,885190767,2259345933,2189233825,2757722062}return SynapseXen_iIilIIIlllIIiIiiI[2339221661]end)({},"iIil",6925,{},{},{},{},{}),256)local SynapseXen_lIlIlIiIliiIIlIli={}for SynapseXen_llllIiIIlliIilIlIl=1,#SynapseXen_IilIIliiiliII do local SynapseXen_llliiIillI=SynapseXen_IilIIliiiliII[SynapseXen_llllIiIIlliIilIlIl]for SynapseXen_lIlIIilIillli=0,#SynapseXen_llliiIillI do local SynapseXen_lilIIlliI=SynapseXen_llliiIillI[SynapseXen_lIlIIilIillli]local SynapseXen_lliilillIl=SynapseXen_lilIIlliI[1]local SynapseXen_IIiiillIiIiIil=SynapseXen_lilIIlliI[2]if SynapseXen_lliilillIl==SynapseXen_IliliIlilllliIIlll and SynapseXen_IIiiillIiIiIil>=SynapseXen_lllIIiiIIIlIiIilii then SynapseXen_lIlIlIiIliiIIlIli[SynapseXen_IIiiillIiIiIil]=SynapseXen_lliilillIl[SynapseXen_IIiiillIiIiIil]SynapseXen_lilIIlliI[1]=SynapseXen_lIlIlIiIliiIIlIli end end end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2986607984]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi my 2.5mb script doesn't work with xen please help"SynapseXen_iIilIIIlllIIiIiiI[2986607984]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3953794399,2012982195),SynapseXen_lillIIili(904775121,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4025173989,3989514298,2106629895,366231159,1651099217,2645637152,3264150180,2125323712}return SynapseXen_iIilIIIlllIIiIiiI[2986607984]end)())then local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3521544600]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="pain exist is gonna connect the dots of xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2178788949,1062438022)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(869695756,869724454)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3521544600]=SynapseXen_lillIIili(SynapseXen_lillIIili(2480213383,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2229626814,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1699859560,3823832586,3735459501,1717181366,675338706,3867764819,1542300639}return SynapseXen_iIilIIIlllIIiIiiI[3521544600]end)(2500,693),512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[90826055]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2388369131,1054677408)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4054312855,4054314882)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[90826055]=SynapseXen_lillIIili(SynapseXen_lillIIili(369915696,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1064597580,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{338016755,73030414,3534434840,484949236,2757909427}return SynapseXen_iIilIIIlllIIiIiiI[90826055]end)("ililliIiiii","iIil","iIIliIliIIiIiiIiIIi",4181),512),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3587241145]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="SECURE API, IMPOSSIBLE TO BYPASS!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3901700274,2456204333)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2709611275,2709675281)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3587241145]=SynapseXen_lillIIili(SynapseXen_lillIIili(1315743938,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2637749942,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3517916648,3682355971,1382011631,2987952311,830056876,313995853}return SynapseXen_iIilIIIlllIIiIiiI[3587241145]end)({},"lil",{},{},{},"l","lIiliillIiiilIII","iiiIiiIIIlIlIli",{})),SynapseXen_ilIIiIIIllIli,256)]=SynapseXen_IIlill%SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1320931762]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2324494214,1363872577)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3287065612,3287130091)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1320931762]=SynapseXen_lillIIili(SynapseXen_lillIIili(264345907,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2109359623,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{99219326,2624801728}return SynapseXen_iIilIIIlllIIiIiiI[1320931762]end)({},{}))then SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[4291301317]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="aspect network better obfuscator"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2281870583,2802884872)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2051239888,2051305810)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4291301317]=SynapseXen_lillIIili(SynapseXen_lillIIili(481234439,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1001205625,SynapseXen_IIliiIliIl[12]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{16054455,2356038838}return SynapseXen_iIilIIIlllIIiIiiI[4291301317]end)({},"lllIiliiiIiIIii"))]=SynapseXen_iiillIIIlIi[SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[1175631070]or(function()local SynapseXen_IlIlllllIIIIlIilii="luraph better then xen bros :pensive:"SynapseXen_iIilIIIlllIIiIiiI[1175631070]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(655334817,286288235),SynapseXen_lillIIili(2679953950,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3321317358,4153939552,4121972836,3609596250,1063084938,2545353568,950265001}return SynapseXen_iIilIIIlllIIiIiiI[1175631070]end)(),262144)]]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[381159685]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi devforum"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1755401632,4008970270)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3395595375,3395660475)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[381159685]=SynapseXen_lillIIili(SynapseXen_lillIIili(1393007980,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2003440835,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{810462678,1281407955,931577924}return SynapseXen_iIilIIIlllIIiIiiI[381159685]end)("IIIlIiIllIil","iliIIIi","IlIilllIllllI",8845,"liilIlI",8625,"IIlillliIllIlII"))then SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2596273221]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1256197158,4183562497)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3425113018,3425133561)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2596273221]=SynapseXen_lillIIili(SynapseXen_lillIIili(326828050,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(159009140,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1363043343}return SynapseXen_iIilIIIlllIIiIiiI[2596273221]end)({},"lllllIiililIlIl","lIlIIiiiIliIllliIll",{}))]=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[1265559681]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2439530957,4152556983)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2888109622,2888134234)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1265559681]=SynapseXen_lillIIili(SynapseXen_lillIIili(368107573,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1010449956,SynapseXen_IIliiIliIl[2]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{648319955,1347085362,3190109095,3753373888,4236569195,3688197835,1648155285,1486766172,3658741395}return SynapseXen_iIilIIIlllIIiIiiI[1265559681]end)(3229))]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3089838453]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"SynapseXen_iIilIIIlllIIiIiiI[3089838453]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3493076858,223848682),SynapseXen_lillIIili(1960831275,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{578006613,3463332497,3949691750,1562033692,272533857}return SynapseXen_iIilIIIlllIIiIiiI[3089838453]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2561871518]or(function()local SynapseXen_IlIlllllIIIIlIilii="sponsored by ironbrew, jk xen is better"SynapseXen_iIilIIIlllIIiIiiI[2561871518]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4069142680,3990661646),SynapseXen_lillIIili(3068721243,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1301049285,1880864422,1915356887,25616702,4086431,1226192571,1733544949,370498661,3197545072,3704181853}return SynapseXen_iIilIIIlllIIiIiiI[2561871518]end)(),256)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]=assert(tonumber(SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]),'`for` initial value must be a number')SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1]=assert(tonumber(SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1]),'`for` limit must be a number')SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2]=assert(tonumber(SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2]),'`for` step must be a number')SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]-SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2]SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+SynapseXen_iIIiillIiIIililIiiI[1050029882]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2043231385]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi xen crashes on my axon paste plz help"SynapseXen_iIilIIIlllIIiIiiI[2043231385]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4256616072,327805463),SynapseXen_lillIIili(1955578998,SynapseXen_IIliiIliIl[8]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{29194442,3352980194,2346143736,2658277971,942871935,489885084,3078272641,1242808744,1459917082}return SynapseXen_iIilIIIlllIIiIiiI[2043231385]end)())then local SynapseXen_IIlill=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[2196078565]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="pain exist is gonna connect the dots of xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(4245564312,2471159986)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(612363223,612362123)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2196078565]=SynapseXen_lillIIili(SynapseXen_lillIIili(160689732,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3320708643,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1265832481,2927977730,2365639142,667544432,59897269,2406687417}return SynapseXen_iIilIIIlllIIiIiiI[2196078565]end)({},2407,{},2171,"IiIIllii",13829,1230,"liliIlIiIillIllIii",12669))local SynapseXen_lllil=SynapseXen_iiIIlliIIIiliil(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[4202666175]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3570436412,1922082290)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(704554637,704565488)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4202666175]=SynapseXen_lillIIili(SynapseXen_lillIIili(4024928716,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2066940837,SynapseXen_IIliiIliIl[1]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{828512,2398954616,1914566850,1299347199,739756143,1709600322}return SynapseXen_iIilIIIlllIIiIiiI[4202666175]end)("ii",{},"lIiIliill"),512),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[446171410]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(392767067,3072270886)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(35916801,35921227)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[446171410]=SynapseXen_lillIIili(SynapseXen_lillIIili(1312381539,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1207028350,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{726608450}return SynapseXen_iIilIIIlllIIiIiiI[446171410]end)("IiIIIIIliiliIIIlII",3493,3574,"IliiiiIIliIIl",4608))]=SynapseXen_IIlill^SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3548750368]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1651299849,1216736918)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4054443061,4054440624)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3548750368]=SynapseXen_lillIIili(SynapseXen_lillIIili(2690314877,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(588050040,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{541477839,1253632204,1947096263}return SynapseXen_iIilIIIlllIIiIiiI[3548750368]end)({},1699,{},{},{}))then SynapseXen_IliliIlilllliIIlll[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3489371525]or(function()local SynapseXen_IlIlllllIIIIlIilii="pain is gonna use the backspace method on xen"SynapseXen_iIilIIIlllIIiIiiI[3489371525]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1133630880,2059793982),SynapseXen_lillIIili(2695688506,SynapseXen_IIliiIliIl[6]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3244190792,199463128,2677743966,867696038,2566444952}return SynapseXen_iIilIIIlllIIiIiiI[3489371525]end)(),256)]=-SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1510506967]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2806499701,3374529122)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3752725930,3752722694)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1510506967]=SynapseXen_lillIIili(SynapseXen_lillIIili(3857889967,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2451188122,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{701379938,502769644,2944171149,4256014885,800575025,3795409961,3262643199,1711851139}return SynapseXen_iIilIIIlllIIiIiiI[1510506967]end)("lIlIIIIlIiiI",7570,{},5051),512)]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2826788529]or(function()local SynapseXen_IlIlllllIIIIlIilii="yed"SynapseXen_iIilIIIlllIIiIiiI[2826788529]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1370075006,4273121090),SynapseXen_lillIIili(111508265,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4129755025,785028090,2543701551,2904162126,2087494391}return SynapseXen_iIilIIIlllIIiIiiI[2826788529]end)())then SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1951151924]or(function()local SynapseXen_IlIlllllIIIIlIilii="luraph better then xen bros :pensive:"SynapseXen_iIilIIIlllIIiIiiI[1951151924]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(488883670,3930261593),SynapseXen_lillIIili(1591355764,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2113401070,1487445835,484206672,404270763,2993498032,1783297110,3495309294,951099653}return SynapseXen_iIilIIIlllIIiIiiI[1951151924]end)())]=SynapseXen_lillIIili(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1426715205]or(function()local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"SynapseXen_iIilIIIlllIIiIiiI[1426715205]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2427876787,3029025707),SynapseXen_lillIIili(2248435893,SynapseXen_IIliiIliIl[4]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1337749185,2098699377,1345843335}return SynapseXen_iIilIIIlllIIiIiiI[1426715205]end)(),512),SynapseXen_ilIIiIIIllIli)~=0;if SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[3278063883]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi devforum"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1613458802,2110796031)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1232138383,1232137196)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3278063883]=SynapseXen_lillIIili(SynapseXen_lillIIili(3838994383,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3769397219,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1324326729,2039119138,2657822635,3142900109,3224689325,4018440766}return SynapseXen_iIilIIIlllIIiIiiI[3278063883]end)({},7086,"liliIiIliIlilllil","llIillil",{},{},{},{}),512)~=0 then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2933344463]or(function()local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iIilIIIlllIIiIiiI[2933344463]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3662256307,1329009903),SynapseXen_lillIIili(2365489392,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3391461199,2595140696,3942372723,3879841043,4149502203,4230252685,3591681841,3685087336}return SynapseXen_iIilIIIlllIIiIiiI[2933344463]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2359185010]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="sponsored by ironbrew, jk xen is better"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1123605042,2820916084)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1026859052,1026927804)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2359185010]=SynapseXen_lillIIili(SynapseXen_lillIIili(2538547952,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3559794865,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3897024652,4191163986,1797164006,153719437,771210646}return SynapseXen_iIilIIIlllIIiIiiI[2359185010]end)(12260,{},"llIIliIiilIII",11653,"llIilIIl","li",11331,"lliiiIiI"))local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[2682067868]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(71770006,468785193)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3009071718,3009078276)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2682067868]=SynapseXen_lillIIili(SynapseXen_lillIIili(2470596607,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(625620536,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{333630871,102108185,2053934838,313006634,3434386885,4172697692,4116712399,3302445977,3710748945,2973916327}return SynapseXen_iIilIIIlllIIiIiiI[2682067868]end)("ll",{},{},{},2905,{},"ilIillIIlIiIiI",14501),512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[1860822674]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3808483394,1901402246)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2303840862,2303835540)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1860822674]=SynapseXen_lillIIili(SynapseXen_lillIIili(573547093,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1011004850,SynapseXen_IIliiIliIl[13]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1473228531,2744641641,321441416,777945064,4228792234,3616522581,3995800985}return SynapseXen_iIilIIIlllIIiIiiI[1860822674]end)(7000,{},"IIiiiiII",10989,"iIIIIlillIllil",11833,9936,"IIiillliiIIllIii","iIilllIIi",9084)),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1]=SynapseXen_IIlill;SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]=SynapseXen_IIlill[SynapseXen_lllil]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2853491470]or(function()local SynapseXen_IlIlllllIIIIlIilii="inb4 posted on exploit reports section"SynapseXen_iIilIIIlllIIiIiiI[2853491470]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(983300478,3755336257),SynapseXen_lillIIili(1291277937,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1234603512,1639839436,2025780192,2078814489,1833387705,1083901039,545940624}return SynapseXen_iIilIIIlllIIiIiiI[2853491470]end)())then SynapseXen_iiillIIIlIi[SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[3739461669]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"SynapseXen_iIilIIIlllIIiIiiI[3739461669]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3915052684,165601283),SynapseXen_lillIIili(2915904559,SynapseXen_IIliiIliIl[9]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2887540590,3865907328,824362566,8425882,2615038291,1662005705,3268807862}return SynapseXen_iIilIIIlllIIiIiiI[3739461669]end)())]]=SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1810394279]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1723256075,65819536)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1949660421,1949720434)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1810394279]=SynapseXen_lillIIili(SynapseXen_lillIIili(767330358,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1202445607,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3775078986}return SynapseXen_iIilIIIlllIIiIiiI[1810394279]end)("iI",13695,"iliiiIIlIiiliIlIi",2955,"iIIlllllillII",14806,5814,9864,6207))]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3437581789]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2250343124,582897329)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3850991880,3851048237)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3437581789]=SynapseXen_lillIIili(SynapseXen_lillIIili(456251280,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3082681843,SynapseXen_IIliiIliIl[12]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2100572693,3325412104,1461771846,2158294164}return SynapseXen_iIilIIIlllIIiIiiI[3437581789]end)(8506,{},10578,{},{},2141))then local SynapseXen_IIlill=SynapseXen_lillIIili(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3058520605]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="can we have an f in chat for ripull"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(999766184,1399777992)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4071305662,4071297136)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3058520605]=SynapseXen_lillIIili(SynapseXen_lillIIili(1058478559,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(4266801384,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1965231743,2288511081,4152498704,3493861201,908748842,464036334}return SynapseXen_iIilIIIlllIIiIiiI[3058520605]end)("lIllii","IliilllIilllillIl","lIll"),512),SynapseXen_ilIIiIIIllIli)local SynapseXen_lllil=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[3418751402]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="wait for someone on devforum to say they are gonna deobfuscate this"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3976656465,806590631)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3190872962,3190876018)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3418751402]=SynapseXen_lillIIili(SynapseXen_lillIIili(3206341221,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3417688964,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3204546076,702775037,138111363,2181821498}return SynapseXen_iIilIIIlllIIiIiiI[3418751402]end)("ill",{},5425,12886))local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[386885406]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi my 2.5mb script doesn't work with xen please help"SynapseXen_iIilIIIlllIIiIiiI[386885406]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1573898458,3223410401),SynapseXen_lillIIili(877512065,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2807436180,1517762326,107407599,74101615,866612971,2553550895,3907656633}return SynapseXen_iIilIIIlllIIiIiiI[386885406]end)())]=SynapseXen_IIlill+SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4193448928]or(function()local SynapseXen_IlIlllllIIIIlIilii="this is a christian obfuscator, no cursing allowed in our scripts"SynapseXen_iIilIIIlllIIiIiiI[4193448928]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3562138674,2940088949),SynapseXen_lillIIili(3536924126,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3145521844,1295369613,2344452272,482533090,759320935,413464573}return SynapseXen_iIilIIIlllIIiIiiI[4193448928]end)())then SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1050114435]or(function()local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iIilIIIlllIIiIiiI[1050114435]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3363459565,2376040076),SynapseXen_lillIIili(3965260767,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3235909624,3932742327,34118740,3426403951,1457573383,1523318706,114110743,2997724041,3786227955,2508753550}return SynapseXen_iIilIIIlllIIiIiiI[1050114435]end)())]=SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3748871018]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi xen doesn't work on sk8r please help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1471683466,3802208187)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3793474325,3793480829)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3748871018]=SynapseXen_lillIIili(SynapseXen_lillIIili(3154791159,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(998644781,SynapseXen_IIliiIliIl[1]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4268199960}return SynapseXen_iIilIIIlllIIiIiiI[3748871018]end)({},{},4210,{}))]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[597866468]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="level 1 crook = luraph, level 100 boss = xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1454666202,883512418)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(867064137,867123523)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[597866468]=SynapseXen_lillIIili(SynapseXen_lillIIili(3581852094,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3211999465,SynapseXen_IIliiIliIl[12]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3332398400,2352512636,505003558,3328057132,1178169705,84797142,4118679884,662311643,1282218847,3228273305}return SynapseXen_iIilIIIlllIIiIiiI[597866468]end)({},13456,"ilIi","IIiIiiIiiIiIIlIiI"))then SynapseXen_IliliIlilllliIIlll[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1657839689]or(function()local SynapseXen_IlIlllllIIIIlIilii="my way to go against expwoiting is to have safety measuwes. i 1 wocawscwipt and onwy moduwes. hewe's how it wowks: this scwipt bewow stowes the moduwes in a tabwe fow each moduwe we send the wist with the moduwes and moduwe infowmation and use inyit a function in my moduwe that wiww stowe the info and aftew it has send to aww the moduwes it wiww dewete them. so whenyevew the cwient twies to hack they cant get the moduwes. onwy this peace of wocawscwipt."SynapseXen_iIilIIIlllIIiIiiI[1657839689]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2919462347,1627458346),SynapseXen_lillIIili(2150104950,SynapseXen_IIliiIliIl[2]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1270855167}return SynapseXen_iIilIIIlllIIiIiiI[1657839689]end)(),256)]=#SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3708134369]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(96631563,269340992)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2227531972,2227597258)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3708134369]=SynapseXen_lillIIili(SynapseXen_lillIIili(3353950077,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2073594144,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2408674731,1871505551,3184364591,279518663,3624943521,1529864737,3308814268,2792110191,381324444}return SynapseXen_iIilIIIlllIIiIiiI[3708134369]end)({},"l",3062)),SynapseXen_ilIIiIIIllIli,512)]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[559887249]or(function()local SynapseXen_IlIlllllIIIIlIilii="pain exist is gonna connect the dots of xen"SynapseXen_iIilIIIlllIIiIiiI[559887249]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2583213148,798900549),SynapseXen_lillIIili(2951071342,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1211209173}return SynapseXen_iIilIIIlllIIiIiiI[559887249]end)())then local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1465465782]or(function()local SynapseXen_IlIlllllIIIIlIilii="SYNAPSE XEN [FE BYPASS] [BETTER THEN LURAPH] [AMAZING] OMG OMG OMG !!!!!!"SynapseXen_iIilIIIlllIIiIiiI[1465465782]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3026176846,4137575513),SynapseXen_lillIIili(3946895914,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{159189245,1040213373,2517283157,2745746878,3317486282,3079260363,397291688,3097972602,2389118263,2507409515}return SynapseXen_iIilIIIlllIIiIiiI[1465465782]end)(),512)local SynapseXen_lllil=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[63729157]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2083714173,1745524105)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(612625411,612612869)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[63729157]=SynapseXen_lillIIili(SynapseXen_lillIIili(3170959658,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(9109561,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4096121931,304453427,3224480283,1026950180,4015180769,3288068072}return SynapseXen_iIilIIIlllIIiIiiI[63729157]end)({},7103,{},{},"iliIIIiiIIl",14131,9825,6419,{}),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1016875277]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="wally bad bird"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2722567521,811714572)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1310849994,1310881117)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1016875277]=SynapseXen_lillIIili(SynapseXen_lillIIili(2495664341,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2941841429,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{825566357,1210174112,2069895848,1133867045,2289850212,4208481353,2853304913}return SynapseXen_iIilIIIlllIIiIiiI[1016875277]end)({}),256)]=SynapseXen_IIlill*SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4271217374]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(4168273750,1652180957)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2864749858,2864821130)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4271217374]=SynapseXen_lillIIili(SynapseXen_lillIIili(162468023,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(974869102,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4088129395,1071787939,491525886,187238322,2463193177,467263512,2550618164,3168785305,2393017726}return SynapseXen_iIilIIIlllIIiIiiI[4271217374]end)(1340,4616,{},13262,1729,{}))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_iiIIlliIIIiliil(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3807806416]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen detects custom getfenv"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1685306228,1865356442)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1381896583,1381889734)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3807806416]=SynapseXen_lillIIili(SynapseXen_lillIIili(1072733001,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(757820056,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{195764898,2806027459,1136016836,2007415545,841507473}return SynapseXen_iIilIIIlllIIiIiiI[3807806416]end)(14890,{},915,4764,"iIIIlIlllI",7633,{},13026,{}),256),SynapseXen_ilIIiIIIllIli,256)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_iIlliliiilliIlI=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+2]local SynapseXen_iIllIlliIi=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]+SynapseXen_iIlliliiilliIlI;SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]=SynapseXen_iIllIlliIi;if SynapseXen_iIlliliiilliIlI>0 then if SynapseXen_iIllIlliIi<=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1]then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+SynapseXen_iIIiillIiIIililIiiI[1050029882]SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+3]=SynapseXen_iIllIlliIi end else if SynapseXen_iIllIlliIi>=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+1]then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+SynapseXen_iIIiillIiIIililIiiI[1050029882]SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+3]=SynapseXen_iIllIlliIi end end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3379972723]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(693250496,4126739776)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2993885897,2993894589)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3379972723]=SynapseXen_lillIIili(SynapseXen_lillIIili(4026377260,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2599157628,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2203975291}return SynapseXen_iIilIIIlllIIiIiiI[3379972723]end)(4417))then local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[479875370]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3415807130,3218167084)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2826050974,2826054091)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[479875370]=SynapseXen_lillIIili(SynapseXen_lillIIili(3825700701,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(972213173,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3960666960,1293003642,2734523052,417544296,1839950856,3086850787,1296537713}return SynapseXen_iIilIIIlllIIiIiiI[479875370]end)({},"lIiIiiIl"),512)local SynapseXen_lllil=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[2223041088]or(function()local SynapseXen_IlIlllllIIIIlIilii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."SynapseXen_iIilIIIlllIIiIiiI[2223041088]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(480646863,577499318),SynapseXen_lillIIili(2541165336,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{55183742,4053955912}return SynapseXen_iIilIIIlllIIiIiiI[2223041088]end)(),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2234721722]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3570269008,1093776060)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1365667504,1365719092)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2234721722]=SynapseXen_lillIIili(SynapseXen_lillIIili(888536900,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(146210240,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3674698808}return SynapseXen_iIilIIIlllIIiIiiI[2234721722]end)({},2946,"IIIllIlilIIiIIlli",5073,"iIiiIliiIiiIiiii","llllIlilliiili"))]=SynapseXen_IIlill/SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1376549284]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="sponsored by ironbrew, jk xen is better"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1110547624,2816673874)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2257007081,2257011886)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1376549284]=SynapseXen_lillIIili(SynapseXen_lillIIili(3128897021,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(4128385720,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3843922684,2706399535,1359112290,4228282943}return SynapseXen_iIilIIIlllIIiIiiI[1376549284]end)({},{},{},2889,"iliI","IiIIlIillIll",{},"liIIIiliIIIlIlllll"))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[4097270858]or(function()local SynapseXen_IlIlllllIIIIlIilii="SYNAPSE XEN [FE BYPASS] [BETTER THEN LURAPH] [AMAZING] OMG OMG OMG !!!!!!"SynapseXen_iIilIIIlllIIiIiiI[4097270858]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2909967071,339921584),SynapseXen_lillIIili(278051829,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{483216289,1072774428,147676610,4015583445,206578269}return SynapseXen_iIilIIIlllIIiIiiI[4097270858]end)(),256)local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1614021739]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3232312524,2133327397)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1427045275,1427041573)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1614021739]=SynapseXen_lillIIili(SynapseXen_lillIIili(4285451317,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1753089230,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{989665073,2766311298,1217386923}return SynapseXen_iIilIIIlllIIiIiiI[1614021739]end)("ili","Iiii",{},{},{}),512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[273684473]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi xen doesn't work on sk8r please help"SynapseXen_iIilIIIlllIIiIiiI[273684473]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1547192495,4205096816),SynapseXen_lillIIili(253758056,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{176608871,2875963927,1319367961,2679045549,2628663847,2899300208}return SynapseXen_iIilIIIlllIIiIiiI[273684473]end)(),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_ilIlliiIIlIil,SynapseXen_IIIliIl;local SynapseXen_lIiIiliIiliiillIll,SynapseXen_iIlllIilIIII;SynapseXen_ilIlliiIIlIil={}if SynapseXen_IIlill~=1 then if SynapseXen_IIlill~=0 then SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllIIiiIIIlIiIilii+SynapseXen_IIlill-1 else SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllilIiIill end;SynapseXen_iIlllIilIIII=0;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_lllIIiiIIIlIiIilii+1,SynapseXen_lIiIiliIiliiillIll do SynapseXen_iIlllIilIIII=SynapseXen_iIlllIilIIII+1;SynapseXen_ilIlliiIIlIil[SynapseXen_iIlllIilIIII]=SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]end;SynapseXen_lIiIiliIiliiillIll,SynapseXen_IIIliIl=SynapseXen_IillIiIlIlIlIlill(SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii](unpack(SynapseXen_ilIlliiIIlIil,1,SynapseXen_lIiIiliIiliiillIll-SynapseXen_lllIIiiIIIlIiIilii)))else SynapseXen_lIiIiliIiliiillIll,SynapseXen_IIIliIl=SynapseXen_IillIiIlIlIlIlill(SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]())end;SynapseXen_lllilIiIill=SynapseXen_lllIIiiIIIlIiIilii-1;if SynapseXen_lllil~=1 then if SynapseXen_lllil~=0 then SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllIIiiIIIlIiIilii+SynapseXen_lllil-2 else SynapseXen_lIiIiliIiliiillIll=SynapseXen_lIiIiliIiliiillIll+SynapseXen_lllIIiiIIIlIiIilii-1 end;SynapseXen_iIlllIilIIII=0;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_lllIIiiIIIlIiIilii,SynapseXen_lIiIiliIiliiillIll do SynapseXen_iIlllIilIIII=SynapseXen_iIlllIilIIII+1;SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_IIIliIl[SynapseXen_iIlllIilIIII]end end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2687546875]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1094439595,749887476)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2037171684,2037175146)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2687546875]=SynapseXen_lillIIili(SynapseXen_lillIIili(1694986855,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2704761388,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3051600361,952271951,637162109,3610797179,487475730,2384274303,942027125,2702797043,2577915003,556238327}return SynapseXen_iIilIIIlllIIiIiiI[2687546875]end)("Ili","lIIIiI","lIlilliilIllII",{},10073,"lllllliIIIIllIIlIIl",4101))then if not not SynapseXen_IliliIlilllliIIlll[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1114412471]or(function()local SynapseXen_IlIlllllIIIIlIilii="wait for someone on devforum to say they are gonna deobfuscate this"SynapseXen_iIilIIIlllIIiIiiI[1114412471]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(478192620,147956530),SynapseXen_lillIIili(1494993305,SynapseXen_IIliiIliIl[9]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3099078764,3672013963,4142360211,2606625304,2411298188,4240642582,3147385934}return SynapseXen_iIilIIIlllIIiIiiI[1114412471]end)(),256)]==(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[712660855]or(function()local SynapseXen_IlIlllllIIIIlIilii="pain exist is gonna connect the dots of xen"SynapseXen_iIilIIIlllIIiIiiI[712660855]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1838931641,1904109281),SynapseXen_lillIIili(330139382,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1234974890,3750732520,2857544655,3011740063,3890183984,3093645205}return SynapseXen_iIilIIIlllIIiIiiI[712660855]end)(),512)==0)then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2494222979]or(function()local SynapseXen_IlIlllllIIIIlIilii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"SynapseXen_iIilIIIlllIIiIiiI[2494222979]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1658817507,1655728202),SynapseXen_lillIIili(2598504958,SynapseXen_IIliiIliIl[8]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3038275040,2645060891,446998696,1107168202,682236466,3756189849,4163552818,1705605427,4061881597}return SynapseXen_iIilIIIlllIIiIiiI[2494222979]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3933836440]or(function()local SynapseXen_IlIlllllIIIIlIilii="skisploit is the superior obfuscator, clearly."SynapseXen_iIilIIIlllIIiIiiI[3933836440]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1902732946,28895647),SynapseXen_lillIIili(1480788969,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2062284412,2821721155,404135763,962531443}return SynapseXen_iIilIIIlllIIiIiiI[3933836440]end)())~=0;local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[208213297]or(function()local SynapseXen_IlIlllllIIIIlIilii="luraph better then xen bros :pensive:"SynapseXen_iIilIIIlllIIiIiiI[208213297]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2709891160,950611473),SynapseXen_lillIIili(3591212137,SynapseXen_IIliiIliIl[2]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{424633623,83263680,3877109572,3077224805,1908528290,1818141348,3611178928,2517520790}return SynapseXen_iIilIIIlllIIiIiiI[208213297]end)(),512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[1418732007]or(function()local SynapseXen_IlIlllllIIIIlIilii="this is so sad, alexa play ripull.mp4"SynapseXen_iIilIIIlllIIiIiiI[1418732007]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(552095691,418969575),SynapseXen_lillIIili(1967390811,SynapseXen_IIliiIliIl[9]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2297534108,123184746,1498339922,379936744,239247789,1674040241,1939956074,268513196}return SynapseXen_iIilIIIlllIIiIiiI[1418732007]end)(),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;if SynapseXen_IIlill==SynapseXen_lllil~=SynapseXen_lllIIiiIIIlIiIilii then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2638442793]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="level 1 crook = luraph, level 100 boss = xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2114822808,2007567141)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3993126757,3993188236)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2638442793]=SynapseXen_lillIIili(SynapseXen_lillIIili(1741531500,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1560223798,SynapseXen_IIliiIliIl[1]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2891073050,2116704662,3474683048,1599216558,4180921490,2871792525}return SynapseXen_iIilIIIlllIIiIiiI[2638442793]end)(5616,4345,14223,"iiillliliIIl",7649))then local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3145220437]or(function()local SynapseXen_IlIlllllIIIIlIilii="pain is gonna use the backspace method on xen"SynapseXen_iIilIIIlllIIiIiiI[3145220437]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3759597628,2248150896),SynapseXen_lillIIili(3484027479,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3932568881,2152958198,86243818}return SynapseXen_iIilIIIlllIIiIiiI[3145220437]end)(),512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[577961911]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="thats how mafia works"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(965896077,2272767351)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1897089327,1897159256)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[577961911]=SynapseXen_lillIIili(SynapseXen_lillIIili(3775159898,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1836424394,SynapseXen_IIliiIliIl[1]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{450906811,4128251550,1508122823,4186490982,1481297707,313587762,1639575523,573888774,1741840106,151267317}return SynapseXen_iIilIIIlllIIiIiiI[577961911]end)(7414),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_iiIIlliIIIiliil(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2775759537]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen detects custom getfenv"SynapseXen_iIilIIIlllIIiIiiI[2775759537]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3841917544,2500645571),SynapseXen_lillIIili(3946899623,SynapseXen_IIliiIliIl[8]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2452928154,2044186583,2853052892}return SynapseXen_iIilIIIlllIIiIiiI[2775759537]end)(),256),SynapseXen_ilIIiIIIllIli,256)]=SynapseXen_IIlill-SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[199037498]or(function()local SynapseXen_IlIlllllIIIIlIilii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."SynapseXen_iIilIIIlllIIiIiiI[199037498]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(421198777,251298184),SynapseXen_lillIIili(3193802050,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1699747346,1469613155,2732952598,2430864477}return SynapseXen_iIilIIIlllIIiIiiI[199037498]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[4175061775]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi devforum"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1663766491,303043342)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(611385963,611393919)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4175061775]=SynapseXen_lillIIili(SynapseXen_lillIIili(2545393573,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1328547772,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{936666347,633928199,1029289180,1617945175,4205634400,3717468132,771568038,4276424150}return SynapseXen_iIilIIIlllIIiIiiI[4175061775]end)("IiliiilIliillII",10529,"iIlIIllIiIlIliIIl"),256)local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[1135556608]or(function()local SynapseXen_IlIlllllIIIIlIilii="pain is gonna use the backspace method on xen"SynapseXen_iIilIIIlllIIiIiiI[1135556608]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4165772717,771437851),SynapseXen_lillIIili(1280237606,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{968539616,897127064,660845016,3321539544,3394882024,990784168,2471519352,1509191293,1475747984,2194543472}return SynapseXen_iIilIIIlllIIiIiiI[1135556608]end)()),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_ilIlliiIIlIil,SynapseXen_IIIliIl;local SynapseXen_lIiIiliIiliiillIll;local SynapseXen_lIlIllIiIIIlI=0;SynapseXen_ilIlliiIIlIil={}if SynapseXen_IIlill~=1 then if SynapseXen_IIlill~=0 then SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllIIiiIIIlIiIilii+SynapseXen_IIlill-1 else SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllilIiIill end;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_lllIIiiIIIlIiIilii+1,SynapseXen_lIiIiliIiliiillIll do SynapseXen_ilIlliiIIlIil[#SynapseXen_ilIlliiIIlIil+1]=SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]end;SynapseXen_IIIliIl={SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii](unpack(SynapseXen_ilIlliiIIlIil,1,SynapseXen_lIiIiliIiliiillIll-SynapseXen_lllIIiiIIIlIiIilii))}else SynapseXen_IIIliIl={SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]()}end;for SynapseXen_iIllIlliIi in next,SynapseXen_IIIliIl do if SynapseXen_iIllIlliIi>SynapseXen_lIlIllIiIIIlI then SynapseXen_lIlIllIiIIIlI=SynapseXen_iIllIlliIi end end;return SynapseXen_IIIliIl,SynapseXen_lIlIllIiIIIlI elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2333450188]or(function()local SynapseXen_IlIlllllIIIIlIilii="imagine using some lua minifier tool and thinking you are a badass"SynapseXen_iIilIIIlllIIiIiiI[2333450188]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2934974031,2173584856),SynapseXen_lillIIili(2260956770,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{254404147}return SynapseXen_iIilIIIlllIIiIiiI[2333450188]end)())then if SynapseXen_lillIIili(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[340812766]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"SynapseXen_iIilIIIlllIIiIiiI[340812766]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(691533760,1908506525),SynapseXen_lillIIili(4047871218,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3326363781,3286290788,1940607795}return SynapseXen_iIilIIIlllIIiIiiI[340812766]end)(),262144),SynapseXen_ilIIiIIIllIli)==(SynapseXen_iIilIIIlllIIiIiiI[1844891478]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"SynapseXen_iIilIIIlllIIiIiiI[1844891478]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4243099828,1830975752),SynapseXen_lillIIili(2740885979,SynapseXen_IIliiIliIl[1]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2883091528,723970769,4023520677,571481363,2159332421,1008664819}return SynapseXen_iIilIIIlllIIiIiiI[1844891478]end)())then SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3218828697]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(524342499,1746533317)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4149785987,4149840496)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3218828697]=SynapseXen_lillIIili(SynapseXen_lillIIili(3372225622,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2529865328,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3700561752,2531082186,136122769,1899306034,221774493,1976847354,302337299,3282271068}return SynapseXen_iIilIIIlllIIiIiiI[3218828697]end)("liIIiilIlIII","IIlIiIlllIlIiili",{}),256)]=SynapseXen_IliIIlIIlIlIlI else SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3218828697]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(524342499,1746533317)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4149785987,4149840496)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3218828697]=SynapseXen_lillIIili(SynapseXen_lillIIili(3372225622,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2529865328,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3700561752,2531082186,136122769,1899306034,221774493,1976847354,302337299,3282271068}return SynapseXen_iIilIIIlllIIiIiiI[3218828697]end)("liIIiilIlIII","IIlIiIlllIlIiili",{}),256)]=SynapseXen_IIliiIliIl[SynapseXen_lillIIili(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[340812766]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen best rerubi paste"SynapseXen_iIilIIIlllIIiIiiI[340812766]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(691533760,1908506525),SynapseXen_lillIIili(4047871218,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3326363781,3286290788,1940607795}return SynapseXen_iIilIIIlllIIiIiiI[340812766]end)(),262144),SynapseXen_ilIIiIIIllIli)]end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[185746739]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="this is so sad, alexa play ripull.mp4"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(245148454,2584298918)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1131944734,1131936676)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[185746739]=SynapseXen_lillIIili(SynapseXen_lillIIili(3408659963,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(4127481202,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3330981427,3460715256,3987404089,115879521,2586726668,2035975475,829488514,4143578923,1128989440,170127345}return SynapseXen_iIilIIIlllIIiIiiI[185746739]end)(10621,14115,"ilIllIlIII",225,8841,9307))then local SynapseXen_lllIiIiliiIIIIIil=SynapseXen_iIllilIiiIiIiIIIii[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[518845294],SynapseXen_iIilIIIlllIIiIiiI[2210192898]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen detects custom getfenv"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(449103776,4122622702)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3265338783,3265334156)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2210192898]=SynapseXen_lillIIili(SynapseXen_lillIIili(1104531221,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2252006083,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{505238930,2758057635,1140611233,4043760381,1702200857,896748878,1003852878,3254846394}return SynapseXen_iIilIIIlllIIiIiiI[2210192898]end)({},2968,{},"liiiIII",{},{}),262144)]local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_lIlili;local SynapseXen_IIiIIiIiiiiiiilIIii;if SynapseXen_lllIiIiliiIIIIIil[982698750]~=0 then SynapseXen_lIlili={}SynapseXen_IIiIIiIiiiiiiilIIii=setmetatable({},{__index=function(SynapseXen_lllllIIIiI,SynapseXen_iIIlilllIIi)local SynapseXen_IliIllIIiIIllII=SynapseXen_lIlili[SynapseXen_iIIlilllIIi]return SynapseXen_IliIllIIiIIllII[1][SynapseXen_IliIllIIiIIllII[2]]end,__newindex=function(SynapseXen_lllllIIIiI,SynapseXen_iIIlilllIIi,SynapseXen_lIIilliiiIIl)local SynapseXen_IliIllIIiIIllII=SynapseXen_lIlili[SynapseXen_iIIlilllIIi]SynapseXen_IliIllIIiIIllII[1][SynapseXen_IliIllIIiIIllII[2]]=SynapseXen_lIIilliiiIIl end})for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_lllIiIiliiIIIIIil[982698750]do local SynapseXen_IilIiil=SynapseXen_IiiIiii[SynapseXen_iilliIiiiliillIIIllI]if SynapseXen_IilIiil[1902504146]==(SynapseXen_iIilIIIlllIIiIiiI[850224607]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi my 2.5mb script doesn't work with xen please help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(679904342,3024402655)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(4282569902,4282623125)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[850224607]=SynapseXen_lillIIili(SynapseXen_lillIIili(321744706,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2158187034,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{130110110,3619705341}return SynapseXen_iIilIIIlllIIiIiiI[850224607]end)({},{},"iiil",5250,"iillIillIIliIi",{},5379,"iliiiIIliIl","lIIlliIllIIliIlIII","liIili"))then SynapseXen_lIlili[SynapseXen_llllIiIIlliIilIlIl-1]={SynapseXen_lliilillIl,SynapseXen_lillIIili(SynapseXen_IilIiil[31674126],SynapseXen_iIilIIIlllIIiIiiI[1124783093]or(function()local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"SynapseXen_iIilIIIlllIIiIiiI[1124783093]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(27982808,3974086151),SynapseXen_lillIIili(2046044,SynapseXen_IIliiIliIl[3]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1128941279,1753435619,3426622725,3075028745,1792634794,2177949522}return SynapseXen_iIilIIIlllIIiIiiI[1124783093]end)())}elseif SynapseXen_IilIiil[1902504146]==(SynapseXen_iIilIIIlllIIiIiiI[2855688064]or(function()local SynapseXen_IlIlllllIIIIlIilii="sponsored by ironbrew, jk xen is better"SynapseXen_iIilIIIlllIIiIiiI[2855688064]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2125024702,27583848),SynapseXen_lillIIili(3711361683,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2583821523}return SynapseXen_iIilIIIlllIIiIiiI[2855688064]end)())then SynapseXen_lIlili[SynapseXen_llllIiIIlliIilIlIl-1]={SynapseXen_lIlIiiIiIi,SynapseXen_lillIIili(SynapseXen_IilIiil[31674126],SynapseXen_iIilIIIlllIIiIiiI[3580195363]or(function()local SynapseXen_IlIlllllIIIIlIilii="skisploit is the superior obfuscator, clearly."SynapseXen_iIilIIIlllIIiIiiI[3580195363]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1435741229,3700070807),SynapseXen_lillIIili(283542476,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2635743001}return SynapseXen_iIilIIIlllIIiIiiI[3580195363]end)())}end;SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end;SynapseXen_IilIIliiiliII[#SynapseXen_IilIIliiiliII+1]=SynapseXen_lIlili end;SynapseXen_lliilillIl[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[4268229530]or(function()local SynapseXen_IlIlllllIIIIlIilii="can we have an f in chat for ripull"SynapseXen_iIilIIIlllIIiIiiI[4268229530]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3949073557,4106912945),SynapseXen_lillIIili(3055211518,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1838134666,2797241552,3574957084}return SynapseXen_iIilIIIlllIIiIiiI[4268229530]end)(),256)]=SynapseXen_iliiiIllIiliiiI(SynapseXen_lllIiIiliiIIIIIil,SynapseXen_iiillIIIlIi,SynapseXen_IIiIIiIiiiiiiilIIii)elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3724519705]or(function()local SynapseXen_IlIlllllIIIIlIilii="inb4 posted on exploit reports section"SynapseXen_iIilIIIlllIIiIiiI[3724519705]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1220497714,1102045410),SynapseXen_lillIIili(2698166115,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3823207625,3762216720}return SynapseXen_iIilIIIlllIIiIiiI[3724519705]end)())then SynapseXen_ilIIiIIIllIli=SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[4263432476]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="my way to go against expwoiting is to have safety measuwes. i 1 wocawscwipt and onwy moduwes. hewe's how it wowks: this scwipt bewow stowes the moduwes in a tabwe fow each moduwe we send the wist with the moduwes and moduwe infowmation and use inyit a function in my moduwe that wiww stowe the info and aftew it has send to aww the moduwes it wiww dewete them. so whenyevew the cwient twies to hack they cant get the moduwes. onwy this peace of wocawscwipt."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(254258454,1589279269)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1283588723,1283595680)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4263432476]=SynapseXen_lillIIili(SynapseXen_lillIIili(463583742,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3893060780,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3566371451,1724216092,574601064,4267456733,3512870450,569008975,424895593}return SynapseXen_iIilIIIlllIIiIiiI[4263432476]end)(7915,{},{},"IiiI",{},"iliIIl","liiIlII"),256)]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2397001969]or(function()local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iIilIIIlllIIiIiiI[2397001969]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3490931856,1932401406),SynapseXen_lillIIili(16901376,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3823151748,3637674999,2515343173,3678409867,407813225,3851658501,2022076928,372340199,2645111649}return SynapseXen_iIilIIIlllIIiIiiI[2397001969]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_lillIIili(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[991069842]or(function()local SynapseXen_IlIlllllIIIIlIilii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"SynapseXen_iIilIIIlllIIiIiiI[991069842]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2937374528,1690808131),SynapseXen_lillIIili(3528702417,SynapseXen_IIliiIliIl[10]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1307759604,620975304}return SynapseXen_iIilIIIlllIIiIiiI[991069842]end)(),256),SynapseXen_ilIIiIIIllIli)~=0;local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[2941932981]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="level 1 crook = luraph, level 100 boss = xen"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3979546062,3744941953)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2817128917,2817123694)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2941932981]=SynapseXen_lillIIili(SynapseXen_lillIIili(4256656460,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1712198787,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1124027665}return SynapseXen_iIilIIIlllIIiIiiI[2941932981]end)(5871,4286)),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[3747975248]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="sometimes it be like that"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3478367805,1068169932)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2761337261,2761404249)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3747975248]=SynapseXen_lillIIili(SynapseXen_lillIIili(1111270663,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2589077317,SynapseXen_IIliiIliIl[5]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4281149359,4177838665}return SynapseXen_iIilIIIlllIIiIiiI[3747975248]end)(8239,{},11445),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;if SynapseXen_IIlill<SynapseXen_lllil~=SynapseXen_lllIIiiIIIlIiIilii then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[571759888]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi xen doesn't work on sk8r please help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1039541434,2529873645)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1366933515,1366939895)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[571759888]=SynapseXen_lillIIili(SynapseXen_lillIIili(2010681229,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2093755024,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2658363254,2688146088,1925363812,174348290}return SynapseXen_iIilIIIlllIIiIiiI[571759888]end)({}))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3306826453]or(function()local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"SynapseXen_iIilIIIlllIIiIiiI[3306826453]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2966845895,2811732757),SynapseXen_lillIIili(3204261956,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2765635886,3062962255,2534064592,2626579238,2495973752,1111905447,637976218,888296673}return SynapseXen_iIilIIIlllIIiIiiI[3306826453]end)(),256)local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3233050271]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1742977386,2660661715)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(588152500,588207584)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3233050271]=SynapseXen_lillIIili(SynapseXen_lillIIili(3929861173,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3136304520,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1042019169,1713364601,3802338855,2249293969,1319005044,2660235727,3318850337,529694861,952922916,559907976}return SynapseXen_iIilIIIlllIIiIiiI[3233050271]end)("li","lilliiillIlliilIli",5830,{},1881,62,{},{},"I"),512)local SynapseXen_lliilillIl,SynapseXen_IIliilIIIliIIiliI=SynapseXen_IliliIlilllliIIlll,SynapseXen_liilIiiI;SynapseXen_lllilIiIill=SynapseXen_lllIIiiIIIlIiIilii-1;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_lllIIiiIIIlIiIilii,SynapseXen_lllIIiiIIIlIiIilii+(SynapseXen_IIlill>0 and SynapseXen_IIlill-1 or SynapseXen_IlliiilIIilIli)do SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_IIliilIIIliIIiliI[SynapseXen_llllIiIIlliIilIlIl-SynapseXen_lllIIiiIIIlIiIilii]end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1470589228]or(function()local SynapseXen_IlIlllllIIIIlIilii="thats how mafia works"SynapseXen_iIilIIIlllIIiIiiI[1470589228]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3687962295,4187329595),SynapseXen_lillIIili(2348524807,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4051482062,3240312872,3580862992,1421766586,3292097065}return SynapseXen_iIilIIIlllIIiIiiI[1470589228]end)())then local SynapseXen_lllil=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[3256517115]or(function()local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"SynapseXen_iIilIIIlllIIiIiiI[3256517115]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(907037279,4153336600),SynapseXen_lillIIili(3909892363,SynapseXen_IIliiIliIl[5]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{883516762}return SynapseXen_iIilIIIlllIIiIiiI[3256517115]end)())local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3387102635]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3385753737,850469970)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1923028981,1923035956)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3387102635]=SynapseXen_lillIIili(SynapseXen_lillIIili(2693511574,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(349627000,SynapseXen_IIliiIliIl[2]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3609464111,1971423933,1462892655,1857117850,1138822157,3704956210,1710323307}return SynapseXen_iIilIIIlllIIiIiiI[3387102635]end)("li",{},"iiIIii",{},{}),256)]=SynapseXen_lliilillIl[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3073295919]or(function()local SynapseXen_IlIlllllIIIIlIilii="now with shitty xor string obfuscation"SynapseXen_iIilIIIlllIIiIiiI[3073295919]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1738453758,3656358790),SynapseXen_lillIIili(663260416,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3767105465,2145318903,2969619588}return SynapseXen_iIilIIIlllIIiIiiI[3073295919]end)())][SynapseXen_lllil]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4112974965]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi xen doesn't work on sk8r please help"SynapseXen_iIilIIIlllIIiIiiI[4112974965]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1703548366,482264974),SynapseXen_lillIIili(3642536114,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4136099651,205277515,61143247,4272516003,618114322,2459165052}return SynapseXen_iIilIIIlllIIiIiiI[4112974965]end)())then local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1762245251]or(function()local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"SynapseXen_iIilIIIlllIIiIiiI[1762245251]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2414310645,759173456),SynapseXen_lillIIili(2864118082,SynapseXen_IIliiIliIl[12]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4114536500,1968478769,1190877110,1438556347,3082873762,2149430889,350085903,3387045186}return SynapseXen_iIilIIIlllIIiIiiI[1762245251]end)()),SynapseXen_ilIIiIIIllIli,256),SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[107157657]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="this is a christian obfuscator, no cursing allowed in our scripts"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2999386983,3053263823)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3966009444,3966081325)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[107157657]=SynapseXen_lillIIili(SynapseXen_lillIIili(112651250,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2712040771,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1746880893,2123649085}return SynapseXen_iIilIIIlllIIiIiiI[107157657]end)("ilIiIilIlliiliIIl",{},{},14855,{},"i"),512)do SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]=nil end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[2849142947]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3760542957,3672776847)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2026527117,2026584597)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2849142947]=SynapseXen_lillIIili(SynapseXen_lillIIili(2484316201,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(123920875,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3746198816,919089025,1646211836,1826023151,1493952422,1833762519,3249330014,424643384,3870666635,3904700363}return SynapseXen_iIilIIIlllIIiIiiI[2849142947]end)("iIlliII",{},3605,"iiilI","lliiilii"))then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+SynapseXen_iIIiillIiIIililIiiI[1050029882]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1950146811]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="wally bad bird"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(933581639,1051343947)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1053428445,1053452426)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1950146811]=SynapseXen_lillIIili(SynapseXen_lillIIili(2147930552,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(548902678,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4101797929,1774570416,952759245,4014871170,3148648756,1954413750,2552319068,860402982}return SynapseXen_iIilIIIlllIIiIiiI[1950146811]end)("iilliIiililll",{},{}))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1749618922]or(function()local SynapseXen_IlIlllllIIIIlIilii="inb4 posted on exploit reports section"SynapseXen_iIilIIIlllIIiIiiI[1749618922]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(759989537,83491605),SynapseXen_lillIIili(2148269523,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1614823055}return SynapseXen_iIilIIIlllIIiIiiI[1749618922]end)()),SynapseXen_ilIIiIIIllIli,256)~=0;local SynapseXen_IIlill=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3708284236]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="xen doesn't come with instance caching, sorry superskater"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2743708921,1268609585)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3770822533,3770827770)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3708284236]=SynapseXen_lillIIili(SynapseXen_lillIIili(3261644362,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2210592095,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{2562407092,2336888471,2327589269,136376329}return SynapseXen_iIilIIIlllIIiIiiI[3708284236]end)("IIIIIllIIillllilll","lIilIIlIiiiiIlIIill",{},3296,{}))local SynapseXen_lllil=SynapseXen_iiIIlliIIIiliil(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[2379767592]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi xen crashes on my axon paste plz help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2742007599,893438041)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2583033765,2583029384)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2379767592]=SynapseXen_lillIIili(SynapseXen_lillIIili(1918535990,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2837465477,SynapseXen_IIliiIliIl[9]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{2397854344,2988331668,3867117384,1773142231,742381117,3569487005}return SynapseXen_iIilIIIlllIIiIiiI[2379767592]end)("lIiIllliillliilIl","lllilllliIllIili","liiIl",{},{}),512),SynapseXen_ilIIiIIIllIli,512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;if SynapseXen_IIlill<=SynapseXen_lllil~=SynapseXen_lllIIiiIIIlIiIilii then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1914006819]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3965014744,3584043923)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1393786358,1393844795)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1914006819]=SynapseXen_lillIIili(SynapseXen_lillIIili(3852470104,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1978178812,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{751339829,3553104005,598530110,2337167700,1459412150,2415423111}return SynapseXen_iIilIIIlllIIiIiiI[1914006819]end)("ilIIIlIIillillilllI"))then SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2063752794]or(function()local SynapseXen_IlIlllllIIIIlIilii="my way to go against expwoiting is to have safety measuwes. i 1 wocawscwipt and onwy moduwes. hewe's how it wowks: this scwipt bewow stowes the moduwes in a tabwe fow each moduwe we send the wist with the moduwes and moduwe infowmation and use inyit a function in my moduwe that wiww stowe the info and aftew it has send to aww the moduwes it wiww dewete them. so whenyevew the cwient twies to hack they cant get the moduwes. onwy this peace of wocawscwipt."SynapseXen_iIilIIIlllIIiIiiI[2063752794]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3915812109,1166532143),SynapseXen_lillIIili(2745355678,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2592495381,189363932,3260510402,4282285316,1399585652,3706097921,3250454677}return SynapseXen_iIilIIIlllIIiIiiI[2063752794]end)(),256)]=not SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[2151295232]or(function()local SynapseXen_IlIlllllIIIIlIilii="this is so sad, alexa play ripull.mp4"SynapseXen_iIilIIIlllIIiIiiI[2151295232]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2611177796,3190645646),SynapseXen_lillIIili(3369928768,SynapseXen_IIliiIliIl[3]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1667490949,3157511712,3334813471}return SynapseXen_iIilIIIlllIIiIiiI[2151295232]end)())]elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1932937074]or(function()local SynapseXen_IlIlllllIIIIlIilii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"SynapseXen_iIilIIIlllIIiIiiI[1932937074]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4249917206,3223517244),SynapseXen_lillIIili(2496451550,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3199305194,1611985699,764837406,586302449,3664328266,415743410,3482936446,2462962140,3537354557}return SynapseXen_iIilIIIlllIIiIiiI[1932937074]end)())then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2362686203]or(function()local SynapseXen_IlIlllllIIIIlIilii="my way to go against expwoiting is to have safety measuwes. i 1 wocawscwipt and onwy moduwes. hewe's how it wowks: this scwipt bewow stowes the moduwes in a tabwe fow each moduwe we send the wist with the moduwes and moduwe infowmation and use inyit a function in my moduwe that wiww stowe the info and aftew it has send to aww the moduwes it wiww dewete them. so whenyevew the cwient twies to hack they cant get the moduwes. onwy this peace of wocawscwipt."SynapseXen_iIilIIIlllIIiIiiI[2362686203]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(826762706,2609457382),SynapseXen_lillIIili(58567480,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3108205478,648071478,3792875852,2899683380,2026485168,3121027297,354787231,2812394854,2929654732,2180956010}return SynapseXen_iIilIIIlllIIiIiiI[2362686203]end)(),256),SynapseXen_ilIIiIIIllIli,256)local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3880811142]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(58192181,3984411172)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2674124550,2674115059)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3880811142]=SynapseXen_lillIIili(SynapseXen_lillIIili(1987204514,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(46421420,SynapseXen_IIliiIliIl[8]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{329910666,3038561987}return SynapseXen_iIilIIIlllIIiIiiI[3880811142]end)({}),512)local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_iIlllIilIIII,SynapseXen_illlIlIIIIIl;local SynapseXen_lIiIiliIiliiillIll;if SynapseXen_IIlill==1 then return elseif SynapseXen_IIlill==0 then SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllilIiIill else SynapseXen_lIiIiliIiliiillIll=SynapseXen_lllIIiiIIIlIiIilii+SynapseXen_IIlill-2 end;SynapseXen_illlIlIIIIIl={}SynapseXen_iIlllIilIIII=0;for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_lllIIiiIIIlIiIilii,SynapseXen_lIiIiliIiliiillIll do SynapseXen_iIlllIilIIII=SynapseXen_iIlllIilIIII+1;SynapseXen_illlIlIIIIIl[SynapseXen_iIlllIilIIII]=SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]end;return SynapseXen_illlIlIIIIIl,SynapseXen_iIlllIilIIII elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[3310639834]or(function()local SynapseXen_IlIlllllIIIIlIilii="aspect network better obfuscator"SynapseXen_iIilIIIlllIIiIiiI[3310639834]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(1380206128,2397054583),SynapseXen_lillIIili(1175945315,SynapseXen_IIliiIliIl[8]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1409492963,753346180,3345752750,1577485556,960513600,4008949528,1632888873,3548351563,489013377,3312969157}return SynapseXen_iIilIIIlllIIiIiiI[3310639834]end)())then local SynapseXen_IIlill=SynapseXen_IliliIlilllliIIlll[SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[817622652]or(function()local SynapseXen_IlIlllllIIIIlIilii="print(bytecode)"SynapseXen_iIilIIIlllIIiIiiI[817622652]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(307590017,1353620065),SynapseXen_lillIIili(3770708343,SynapseXen_IIliiIliIl[4]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{419999639}return SynapseXen_iIilIIIlllIIiIiiI[817622652]end)(),512)]if not not SynapseXen_IIlill==(SynapseXen_lillIIili(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[861585713]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="this is a christian obfuscator, no cursing allowed in our scripts"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(478172542,1038942354)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1097192040,1097190366)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[861585713]=SynapseXen_lillIIili(SynapseXen_lillIIili(2335438865,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(866592071,SynapseXen_IIliiIliIl[6]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1803516,781091480,1392758876,1566005029,2732403486,1948890459}return SynapseXen_iIilIIIlllIIiIiiI[861585713]end)({},{},"IiliIilIIIi",{},{},10912,"liIiIIilIIllI"),512),SynapseXen_ilIIiIIIllIli)==0)then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1 else SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[1157053867]or(function()local SynapseXen_IlIlllllIIIIlIilii="thats how mafia works"SynapseXen_iIilIIIlllIIiIiiI[1157053867]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(3153238270,1939183200),SynapseXen_lillIIili(3341449765,SynapseXen_IIliiIliIl[11]))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4291585478,649789820,2766051489}return SynapseXen_iIilIIIlllIIiIiiI[1157053867]end)(),256)]=SynapseXen_IIlill end elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4030151273]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi my 2.5mb script doesn't work with xen please help"SynapseXen_iIilIIIlllIIiIiiI[4030151273]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(2370849318,1706000866),SynapseXen_lillIIili(95049263,SynapseXen_IIliiIliIl[3]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{412467578,2976784186,4213005441,3219785210,2834061951,173765433,2179268621}return SynapseXen_iIilIIIlllIIiIiiI[4030151273]end)())then local SynapseXen_IIlill,SynapseXen_lllil=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[927314387]or(function()local SynapseXen_IlIlllllIIIIlIilii="epic gamer vision"SynapseXen_iIilIIIlllIIiIiiI[927314387]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(807802397,2274683871),SynapseXen_lillIIili(504241661,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{4287587948,811356152,2249641043,3349513700,1457561955,2641009942}return SynapseXen_iIilIIIlllIIiIiiI[927314387]end)(),512),SynapseXen_ilIIiIIIllIli,512),SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[659900338]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2602039735,3285084897)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3019420033,3019479348)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[659900338]=SynapseXen_lillIIili(SynapseXen_lillIIili(1261583530,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3126415870,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3855480927,1587152270,330628007,2063308755}return SynapseXen_iIilIIIlllIIiIiiI[659900338]end)({},"IIII",{},"IIIiil","iliillilIIlI",{},{}))local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_IIlill>255 then SynapseXen_IIlill=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_IIlill-256]else SynapseXen_IIlill=SynapseXen_lliilillIl[SynapseXen_IIlill]end;if SynapseXen_lllil>255 then SynapseXen_lllil=SynapseXen_IIilIIiiIIIiiliIil[SynapseXen_lllil-256]else SynapseXen_lllil=SynapseXen_lliilillIl[SynapseXen_lllil]end;SynapseXen_lliilillIl[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[2674217529]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi devforum"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1246190301,2579588716)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1159290788,1159354561)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[2674217529]=SynapseXen_lillIIili(SynapseXen_lillIIili(3471940042,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3032837365,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1974699964,1764810507,1187749265,471585116,552842601}return SynapseXen_iIilIIIlllIIiIiiI[2674217529]end)({},12456,"IliiIlIiII",{},"IiiI"))][SynapseXen_IIlill]=SynapseXen_lllil elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4137618607]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="now comes with a free n word pass"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1794406332,2321094792)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2952474109,2952473113)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4137618607]=SynapseXen_lillIIili(SynapseXen_lillIIili(1276689733,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(211665552,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{2944849226,231444223,3526260410}return SynapseXen_iIilIIIlllIIiIiiI[4137618607]end)("IllIliI",{},"IiiIIiii",{},{},5240,"IlIliIiilliIIi","IllliiIl",6958,"ilillIlI"))then SynapseXen_IliliIlilllliIIlll[SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3449421571]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="luraph better then xen bros :pensive:"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(1795003739,730360698)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1437095273,1437144580)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3449421571]=SynapseXen_lillIIili(SynapseXen_lillIIili(4179478510,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3072823438,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3495904001,3431603746,1434099189,745289844,2816713576,148556184,551112015,642319023}return SynapseXen_iIilIIIlllIIiIiiI[3449421571]end)({}),256)]={}elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[4156164161]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="hi xen crashes on my axon paste plz help"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3533602301,2364773069)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(1431636464,1431636796)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[4156164161]=SynapseXen_lillIIili(SynapseXen_lillIIili(48847484,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(4114143462,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1517631912,447543520,3894379304,3844495265,3589407916}return SynapseXen_iIilIIIlllIIiIiiI[4156164161]end)({},"llI","IlI",2878,"IIIiIiiliIi","lIlilllIiilllIlilll",{},1941,9947))then local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;local SynapseXen_IIlill=SynapseXen_IiiilIIIIiIIIlI(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[3286099876]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(476468903,3986716745)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3327403517,3327407270)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3286099876]=SynapseXen_lillIIili(SynapseXen_lillIIili(321193460,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(1120624031,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3876391237,3056048917,2997107476,588333636,4237181954,2514376226,2746281112}return SynapseXen_iIilIIIlllIIiIiiI[3286099876]end)(9286,"IllIIIlIiIilII","iiIIiIIIIIiIIill","liiiiIiI","IliiIIIiilIIiIl",{},"IIiIIIiil","liIllIIIliliIiiiii"),512)local SynapseXen_ilIlIII=SynapseXen_lliilillIl[SynapseXen_IIlill]for SynapseXen_llllIiIIlliIilIlIl=SynapseXen_IIlill+1,SynapseXen_iiIIlliIIIiliil(SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[84666744]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="yed"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(63324173,2567780398)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2712669476,2712671297)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[84666744]=SynapseXen_lillIIili(SynapseXen_lillIIili(582782751,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(298560836,SynapseXen_IliIIlIIlIlIlI))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{1287849774,481540864,2582223661,1590952752}return SynapseXen_iIilIIIlllIIiIiiI[84666744]end)({},4196,{},"IlIiIi",603),512),SynapseXen_ilIIiIIIllIli,512)do SynapseXen_ilIlIII=SynapseXen_ilIlIII..SynapseXen_lliilillIl[SynapseXen_llllIiIIlliIilIlIl]end;SynapseXen_IliliIlilllliIIlll[SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[655723646]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="can we have an f in chat for ripull"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(2104199711,435769096)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(963786704,963778951)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[655723646]=SynapseXen_lillIIili(SynapseXen_lillIIili(1396393776,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(954342646,SynapseXen_IIliiIliIl[11]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{1836690924,3515396440,3507070521,196345698,3278684494,3429317630,1928244737}return SynapseXen_iIilIIIlllIIiIiiI[655723646]end)("IiilliliIIlIIlIiiil","IIIIiiilIi","iiIilIlIlIiIIlliii",2505))]=SynapseXen_ilIlIII elseif SynapseXen_IIllilI==(SynapseXen_iIilIIIlllIIiIiiI[1633893892]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="inb4 posted on exploit reports section"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(3052696128,1880832186)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(245480221,245537207)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[1633893892]=SynapseXen_lillIIili(SynapseXen_lillIIili(2531024936,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(3194402741,SynapseXen_IIliiIliIl[3]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3802628430,1772593878,2142121166,1566934597,3592204892}return SynapseXen_iIilIIIlllIIiIiiI[1633893892]end)({},{},{},11649))then local SynapseXen_lllIIiiIIIlIiIilii=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[1997878473],SynapseXen_iIilIIIlllIIiIiiI[3162216979]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(4072294189,1731619382)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(3041938210,3042009391)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil-SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3162216979]=SynapseXen_lillIIili(SynapseXen_lillIIili(3087720880,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2376996737,SynapseXen_IIliiIliIl[7]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-SynapseXen_IIllilI-#{3574969592,811265706,747715662}return SynapseXen_iIilIIIlllIIiIiiI[3162216979]end)({},"lililiiiIii",3879,1394),256)local SynapseXen_IIlill=SynapseXen_iiIIlliIIIiliil(SynapseXen_iIIiillIiIIililIiiI[31674126],SynapseXen_iIilIIIlllIIiIiiI[2006539795]or(function()local SynapseXen_IlIlllllIIIIlIilii="hi devforum"SynapseXen_iIilIIIlllIIiIiiI[2006539795]=SynapseXen_lillIIili(SynapseXen_IIiIllllliI(4163635175,1897977643),SynapseXen_lillIIili(548406073,SynapseXen_IliIIlIIlIlIlI))-SynapseXen_IIllilI-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{3445172065,1319985767,3975678498,2802133078,3893372487,80865164,408830403,2786726630,866063998}return SynapseXen_iIilIIIlllIIiIiiI[2006539795]end)(),512)local SynapseXen_lllil=SynapseXen_lillIIili(SynapseXen_iIIiillIiIIililIiiI[233588846],SynapseXen_iIilIIIlllIIiIiiI[3481987479]or(function(...)local SynapseXen_IlIlllllIIIIlIilii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."local SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIiIllllliI(702794506,2364179599)local SynapseXen_iliiIIIIiiiiiI={...}for SynapseXen_IIillIiiiiIi,SynapseXen_Iilii in pairs(SynapseXen_iliiIIIIiiiiiI)do local SynapseXen_IliIlIIIlIIllIlI;local SynapseXen_iiIIIiIliiliiIIl=type(SynapseXen_Iilii)if SynapseXen_iiIIIiIliiliiIIl=="number"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii elseif SynapseXen_iiIIIiIliiliiIIl=="string"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_Iilii:len()elseif SynapseXen_iiIIIiIliiliiIIl=="table"then SynapseXen_IliIlIIIlIIllIlI=SynapseXen_IIiIllllliI(2506320214,2506375217)end;SynapseXen_IIIIliliiiiIiiil=SynapseXen_IIIIliliiiiIiiil+SynapseXen_IliIlIIIlIIllIlI end;SynapseXen_iIilIIIlllIIiIiiI[3481987479]=SynapseXen_lillIIili(SynapseXen_lillIIili(3408538124,SynapseXen_IIIIliliiiiIiiil),SynapseXen_lillIIili(2007370375,SynapseXen_IIliiIliIl[10]))-string.len(SynapseXen_IlIlllllIIIIlIilii)-#{145467582,3560106105,1044477835,2077391817,102797013,2138533319,2010636405,3523148384,3678039188}return SynapseXen_iIilIIIlllIIiIiiI[3481987479]end)(10417,5510,{},{},"IllilIiIiII"))local SynapseXen_lliilillIl=SynapseXen_IliliIlilllliIIlll;if SynapseXen_lllil==0 then SynapseXen_iilliIiiiliillIIIllI=SynapseXen_iilliIiiiliillIIIllI+1;SynapseXen_lllil=SynapseXen_IiiIiii[SynapseXen_iilliIiiiliillIIIllI][638088808]end;local SynapseXen_lIIliiI=(SynapseXen_lllil-1)*50;local SynapseXen_lIiIiIill=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii]if SynapseXen_IIlill==0 then SynapseXen_IIlill=SynapseXen_lllilIiIill-SynapseXen_lllIIiiIIIlIiIilii end;for SynapseXen_llllIiIIlliIilIlIl=1,SynapseXen_IIlill do SynapseXen_lIiIiIill[SynapseXen_lIIliiI+SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_lliilillIl[SynapseXen_lllIIiiIIIlIiIilii+SynapseXen_llllIiIIlliIilIlIl]end end end end;local SynapseXen_ilIlliiIIlIil={...}for SynapseXen_llllIiIIlliIilIlIl=0,SynapseXen_IlliiilIIilIli do if SynapseXen_llllIiIIlliIilIlIl>=SynapseXen_lIiIlIIilIIilililll[1481819974]then SynapseXen_liilIiiI[SynapseXen_llllIiIIlliIilIlIl-SynapseXen_lIiIlIIilIIilililll[1481819974]]=SynapseXen_ilIlliiIIlIil[SynapseXen_llllIiIIlliIilIlIl+1]else SynapseXen_IliliIlilllliIIlll[SynapseXen_llllIiIIlliIilIlIl]=SynapseXen_ilIlliiIIlIil[SynapseXen_llllIiIIlliIilIlIl+1]end end;local SynapseXen_IIlill,SynapseXen_lllil=SynapseXen_iilIIllIlIiIlilIill()if SynapseXen_IIlill and SynapseXen_lllil>0 then return unpack(SynapseXen_IIlill,1,SynapseXen_lllil)end;return end end;local function SynapseXen_iiIlIlIiIliiiiiii(SynapseXen_iiIIIlIl,SynapseXen_iiillIIIlIi)local SynapseXen_iiilllll=SynapseXen_IlliI(SynapseXen_iiIIIlIl)return SynapseXen_iliiiIllIiliiiI(SynapseXen_iiilllll,SynapseXen_iiillIIIlIi or getfenv(0)),SynapseXen_iiilllll end;return SynapseXen_iiIlIlIiIliiiiiii(SynapseXen_ilIIl("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"),getfenv())()("Clicked")
        end)


        section1:addButton("FE Rtx", function()
            getgenv().mode = "Summer" -- Choose from Summer and Autumn




local a = game.Lighting
a.Ambient = Color3.fromRGB(33, 33, 33)
a.Brightness = 6.67
a.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
a.ColorShift_Top = Color3.fromRGB(255, 247, 237)
a.EnvironmentDiffuseScale = 0.105
a.EnvironmentSpecularScale = 0.522
a.GlobalShadows = true
a.OutdoorAmbient = Color3.fromRGB(51, 54, 67)
a.ShadowSoftness = 0.04
a.GeographicLatitude = -15.525
a.ExposureCompensation = 0.75
local b = Instance.new("BloomEffect", a)
b.Enabled = true
b.Intensity = 0.04
b.Size = 1900
b.Threshold = 0.915
local c = Instance.new("ColorCorrectionEffect", a)
c.Brightness = 0.176
c.Contrast = 0.39
c.Enabled = true
c.Saturation = 0.2
c.TintColor = Color3.fromRGB(217, 145, 57)
if getgenv().mode == "Summer" then
   c.TintColor = Color3.fromRGB(255, 220, 148)
elseif getgenv().mode == "Autumn" then
   c.TintColor = Color3.fromRGB(217, 145, 57)
else
   warn("No mode selected!")
   print("Please select a mode")
   b:Destroy()
   c:Destroy()
end
local d = Instance.new("DepthOfFieldEffect", a)
d.Enabled = true
d.FarIntensity = 0.077
d.FocusDistance = 21.54
d.InFocusRadius = 20.77
d.NearIntensity = 0.277
local e = Instance.new("ColorCorrectionEffect", a)
e.Brightness = 0
e.Contrast = -0.07
e.Saturation = 0
e.Enabled = true
e.TintColor = Color3.fromRGB(255, 247, 239)
local e2 = Instance.new("ColorCorrectionEffect", a)
e2.Brightness = 0.2
e2.Contrast = 0.45
e2.Saturation = -0.1
e2.Enabled = true
e2.TintColor = Color3.fromRGB(255, 255, 255)
local s = Instance.new("SunRaysEffect", a)
s.Enabled = true
s.Intensity = 0.01
s.Spread = 0.146

print("RTX Graphics loaded \n created by Rawget#0001")("Clicked")
            end)


            section1:addButton("FE Pyromaniac God", function()
                _G.R15ToR6 = false --Set to true if you want R15 To R6.
_G.HatsToAlign = {}

do
	settings().Physics.AllowSleep = false
	
	local Players = game:GetService("Players")
	local Player = Players.LocalPlayer
	local Character = Player.Character
	local RunService = game:GetService("RunService")
	local StarterGui = game:GetService("StarterGui")
	local Workspace = game:GetService("Workspace")
	local Camera = Workspace.CurrentCamera
	
	if _G.R15ToR6 == true and Character:FindFirstChildOfClass("Humanoid").RigType == Enum.HumanoidRigType.R6 then
		_G.R15ToR6 = false
	end
	
	local HasLoaded = game:HttpGetAsync("https://gist.githubusercontent.com/M6HqVBcddw2qaN4s/f220711587497035bce3cd1a9c528a22/raw/QV5*2USpvr%253FdFd@%257Bx+2")
	print(HasLoaded)
	
	local R15ToR6_Offsets = {
		LowerTorso = Vector3.new(0, 1, 0),
		LeftUpperArm = Vector3.new(0, -0.15, 0),
		LeftLowerArm = Vector3.new(0, 0.46, 0),
		LeftHand = Vector3.new(0, 1.05, 0),
		RightUpperArm = Vector3.new(0, -0.15, 0),
		RightLowerArm = Vector3.new(0, 0.46, 0),
		RightHand = Vector3.new(0, 1.05, 0),
		LeftUpperLeg = Vector3.new(0, -0.5, 0),
		LeftLowerLeg = Vector3.new(0, 0.27, 0),
		LeftFoot = Vector3.new(0, 0.85, 0),
		RightUpperLeg = Vector3.new(0, -0.5, 0),
		RightLowerLeg = Vector3.new(0, 0.27, 0),
		RightFoot = Vector3.new(0, 0.85, 0)
	}
	
	local Credits = game:HttpGetAsync("https://gist.githubusercontent.com/M6HqVBcddw2qaN4s/f36755d45c260f4ae67b76346ac15888/raw/SCmf%255E8%255EVd%253E%2523%2522Ggu&")
	
	StarterGui:SetCore("SendNotification", {
		Title = "Nullware Reanimate Reborn (CREDITS)",
		Text = Credits,
		Duration = 3
	})
	
	local Humanoid = Character:FindFirstChildOfClass("Humanoid")
	if Humanoid.RigType == Enum.HumanoidRigType.R15 then
		for _,Scale in pairs(Humanoid:GetChildren()) do
			if Scale:IsA("NumberValue") then
				Scale:Destroy()
			end
		end
	end
	
	local AlignsFolder = Instance.new("Folder")
	AlignsFolder.Name = "NWAligns"
	AlignsFolder.Parent = Workspace
	
	Character.Archivable = true
	
	local ReanimateCharacter
	if _G.R15ToR6 ~= true then
		ReanimateCharacter = Character:Clone()
	elseif _G.R15ToR6 == true then
		ReanimateCharacter = game:GetObjects("rbxassetid://5765675785")[1]
	end
	
	if _G.R15ToR6 == true then
		if ReanimateCharacter:FindFirstChild("Animate") then
			ReanimateCharacter:FindFirstChild("Animate").Disabled = true
		end
		
		if Character:FindFirstChild("Animate") then
			Character:FindFirstChild("Animate").Disabled = true
		end
	end
	
	if _G.R15ToR6 == true then
		for _,Hat in pairs(Character:GetChildren()) do
			if Hat:IsA("Accessory") then
				Hat.Archivable = true
				local HatClone = Hat:Clone()
				local HandleClone = HatClone:FindFirstChild("Handle")
				for _,Weld in pairs(HandleClone:GetChildren()) do
					if Weld:IsA("Weld") and Weld.Name == "AccessoryWeld" and Weld.Part1 then
						local Part0 = Weld.Part1
						
						if string.find(Part0.Name, "Upper") or string.find(Part0.Name, "Lower") or string.find(Part0.Name, "Hand") or string.find(Part0.Name, "Foot") then
							if Part0.Name == "LowerTorso" or Part0.Name == "UpperTorso" then
								Weld.Part1 = ReanimateCharacter:FindFirstChild("Torso")
							else
								if string.find(Part0.Name, "Upper") then
									local Output, AmountOfReplacements = string.gsub(Part0.Name, "Upper", " ")
									Weld.Part1 = ReanimateCharacter:FindFirstChild(Output)
								elseif string.find(Part0.Name, "Lower") then
									local Output, AmountOfReplacements = string.gsub(Part0.Name, "Lower", " ")
									Weld.Part1 = ReanimateCharacter:FindFirstChild(Output)
								elseif string.find(Part0.Name, "Hand") then
									local Output, AmountOfReplacements = string.gsub(Part0.Name, "Hand", " Arm")
									Weld.Part1 = ReanimateCharacter:FindFirstChild(Output)
								elseif string.find(Part0.Name, "Foot") then
									local Output, AmountOfReplacements = string.gsub(Part0.Name, "Foot", " Leg")
									Weld.Part1 = ReanimateCharacter:FindFirstChild(Output)
								end
							end
						else
							Weld.Part1 = ReanimateCharacter:FindFirstChild(Part0.Name)
						end
						
						HatClone.Parent = ReanimateCharacter
					end
				end
			end
		end
	end
	
	ReanimateCharacter.Name = "NullwareReanim"
	
	for _,Motor in pairs(Character:GetDescendants()) do
		if Motor:IsA("Motor6D") then
			local DoNotDelete = {}
			
			if _G.R15ToR6 == true then
				DoNotDelete = {"Root", "Neck", "Waist", "LeftElbow", "LeftWrist", "RightElbow", "RightWrist", "LeftKnee", "LeftAnkle", "RightKnee", "RightAnkle"}
			elseif _G.R15ToR6 ~= true then
				DoNotDelete = {"RootJoint", "Root", "Neck", "LeftWrist", "RightWrist", "LeftAnkle", "RightAnkle"}
			end
			
			if not table.find(DoNotDelete, Motor.Name) then
				Motor:Destroy()
			end
		end
	end
	
	if game.PlaceId ==  2041312716 then
		Character:FindFirstChild("FirstPerson"):Destroy()
		Character:FindFirstChild("Local Ragdoll"):Destroy()
		Character:FindFirstChild("Controls"):Destroy()
		
		for _,RagdollConstraint in pairs(Character:GetChildren()) do
			if RagdollConstraint:IsA("BallSocketConstraint") or socks:IsA("HingeConstraint") then
				RagdollConstraint:Destroy()
			end
		end
	end
	
	local DisabledAligns = {}
	local Connections = {}
	table.insert(Connections, RunService.Stepped:Connect(function()
		for _,Part in pairs(Character:GetChildren()) do
			if Part:IsA("BasePart") then
				Part.CanCollide = false
			end
		end
		
		for _,Object in pairs(ReanimateCharacter:GetDescendants()) do
			if Object:IsA("BasePart") or Object:IsA("Decal") then
				Object.Transparency = 1
			end
		end
	end))
	
	if Character:FindFirstChild("Animate") then
		Character:FindFirstChild("Animate"):Destroy()
		wait()
		for _,AnimationTrack in pairs(Character:FindFirstChildOfClass("Humanoid"):GetPlayingAnimationTracks()) do
			AnimationTrack:Stop()
		end
	end
	
	ReanimateCharacter.Parent = Player.Character
	
	for _,Part0 in pairs(Character:GetChildren()) do
		if Part0:IsA("BasePart") then
			local Part1
			if _G.R15ToR6 ~= true then
				Part1 = ReanimateCharacter:FindFirstChild(Part0.Name)
			elseif _G.R15ToR6 == true then
				if string.find(Part0.Name, "Upper") or string.find(Part0.Name, "Lower") or string.find(Part0.Name, "Hand") or string.find(Part0.Name, "Foot") then
					if Part0.Name == "UpperTorso" or Part0.Name == "LowerTorso" then
						Part1 = ReanimateCharacter:FindFirstChild("Torso")
					else
						if string.find(Part0.Name, "Upper") then
							local Output,AmountOfReplacements = string.gsub(Part0.Name, "Upper", " ")
							Part1 = ReanimateCharacter:FindFirstChild(Output)
						elseif string.find(Part0.Name, "Lower") then
							local Output,AmountOfReplacements = string.gsub(Part0.Name, "Lower", " ")
							Part1 = ReanimateCharacter:FindFirstChild(Output)
						elseif string.find(Part0.Name, "Hand") then
							local Output,AmountOfReplacements = string.gsub(Part0.Name, "Hand", " Arm")
							Part1 = ReanimateCharacter:FindFirstChild(Output)
						elseif string.find(Part0.Name, "Foot") then
							local Output,AmountOfReplacements = string.gsub(Part0.Name, "Foot", " Leg")
							Part1 = ReanimateCharacter:FindFirstChild(Output)
						end
					end
				else
					Part1 = ReanimateCharacter:FindFirstChild(Part0.Name)
				end
			end
			
			local DoNotAlign = {}
			
			if _G.R15ToR6 == true then
				DoNotAlign = {"HumanoidRootPart", "Head", "LeftLowerArm", "LeftHand", "RightLowerArm", "RightHand", "LeftLowerLeg", "LeftFoot", "RightLowerLeg", "RightFoot"}
			elseif _G.R15ToR6 ~= true then
				DoNotAlign = {"HumanoidRootPart", "Head", "LeftHand", "LeftHand", "RightHand", "LeftFoot", "RightFoot"}
			end
			
			local Attachment0 = Instance.new("Attachment")
			if _G.R15ToR6 == true then
				if R15ToR6_Offsets[Part0.Name] then
					Attachment0.Position = R15ToR6_Offsets[Part0.Name]
				end
			end
			Attachment0.Parent = Part0
			local Attachment1 = Instance.new("Attachment")
			Attachment1.Parent = Part1
			local AlignPosition = Instance.new("AlignPosition")
			local AlignOrientation = Instance.new("AlignOrientation")
			AlignPosition.Name = "AP-"..Part0.Name
			AlignOrientation.Name = "AO-"..Part0.Name
			if table.find(DoNotAlign, Part0.Name) then
				AlignPosition.Enabled, AlignOrientation.Enabled = false, false
				table.insert(DisabledAligns, AlignPosition)
				table.insert(DisabledAligns, AlignOrientation)
			end
			AlignPosition.Attachment0, AlignPosition.Attachment1, AlignPosition.MaxForce, AlignPosition.Responsiveness = Attachment0, Attachment1, 9e37, 200
			AlignOrientation.Attachment0, AlignOrientation.Attachment1, AlignOrientation.MaxTorque, AlignOrientation.Responsiveness = Attachment0, Attachment1, 9e37, 200
			AlignPosition.Parent = Part0
			AlignOrientation.Parent = Part0
		elseif Part0:IsA("Accessory") then
			if typeof(_G.HatsToAlign) == "table" and #_G.HatsToAlign ~= 0 then
				if #_G.HatsToAlign == 1 and _G.HatsToAlign[1] == "All" or table.find(_G.HatsToAlign, Part0.Name) then
					local HatName = Part0.Name
					Part0 = Part0:FindFirstChild("Handle")
					Part0:FindFirstChild("AccessoryWeld"):Destroy()
					local Part1 = ReanimateCharacter:FindFirstChild(HatName):FindFirstChild("Handle")
					local Attachment0 = Instance.new("Attachment")
					if ReanimateCharacter:FindFirstChildOfClass("Humanoid").RigType == Enum.HumanoidRigType.R6 then
						if _G.R15ToR6 == true then
							Attachment0.Position = Vector3.new(0, 0.2, 0)
						end
					end
					Attachment0.Parent = Part0
					local Attachment1 = Instance.new("Attachment")
					Attachment1.Parent = Part1
					local AlignPosition = Instance.new("AlignPosition")
					local AlignOrientation = Instance.new("AlignOrientation")
					AlignPosition.Name = "AP-"..Part0.Name
					AlignOrientation.Name = "AO-"..Part0.Name
					AlignPosition.Attachment0, AlignPosition.Attachment1, AlignPosition.MaxForce, AlignPosition.Responsiveness = Attachment0, Attachment1, 9e37, 200
					AlignOrientation.Attachment0, AlignOrientation.Attachment1, AlignOrientation.MaxTorque, AlignOrientation.Responsiveness = Attachment0, Attachment1, 9e37, 200
					AlignPosition.Parent = AlignsFolder
					AlignOrientation.Parent = AlignsFolder
				end

			end
		end
	end
	
	if Character:FindFirstChild("Animate") then
		Character:FindFirstChild("Animate"):Destroy()
	end
	ReanimateCharacter:FindFirstChild("HumanoidRootPart").CFrame = Character:FindFirstChild("HumanoidRootPart").CFrame
	ReanimateCharacter:FindFirstChild("HumanoidRootPart").Size = Vector3.new(7, 2, 7)
	Player.Character = ReanimateCharacter
	Camera.CameraSubject = ReanimateCharacter:FindFirstChildOfClass("Humanoid")
	if _G.R15ToR6 ~= true then
		if ReanimateCharacter:FindFirstChild("Animate") then
			ReanimateCharacter:FindFirstChild("Animate").Disabled = true
			ReanimateCharacter:FindFirstChild("Animate").Disabled = false
		end
	elseif _G.R15ToR6 == true then
		spawn(function()
			loadstring(game:HttpGetAsync("https://gist.githubusercontent.com/M6HqVBcddw2qaN4s/286d4299b9f359b7c458f88006ddf74c/raw/R15Animate"))()
		end)
	end
	
	CharacterConnection = Player.CharacterRemoving:Connect(function(Model)
		if Model == Character then
			if _G.Bypass ~= true then
				for ConnectionIndex,Connection in pairs(Connections) do
					Connection:Disconnect()
					table.remove(Connections, ConnectionIndex)
				end
				for DisabledAlign,_ in pairs(DisabledAligns) do
					table.remove(DisabledAligns, DisabledAlign)
				end
				
				Connections = nil
				DisabledAligns = nil
				
				if Workspace:FindFirstChild("NWAligns") then
					Workspace:FindFirstChild("NWAligns"):Destroy()
				end
				
				CharacterConnection:Disconnect()
				CharacterConnection = nil
			end
		elseif Model == ReanimateCharacter then
			if _G.Bypass ~= true then
				for ConnectionIndex,Connection in pairs(Connections) do
					Connection:Disconnect()
					table.remove(Connections, ConnectionIndex)
				end
				for DisabledAlign,_ in pairs(DisabledAligns) do
					table.remove(DisabledAligns, DisabledAlign)
				end
				
				Connections = nil
				DisabledAligns = nil
				
				if Workspace:FindFirstChild("NWAligns") then
					Workspace:FindFirstChild("NWAligns"):Destroy()
				end
				
				CharacterConnection:Disconnect()
				CharacterConnection = nil
			end
		end
	end)
	
	local ResetBindable = Instance.new("BindableEvent")
	ResetBindable.Event:Connect(function()
		if not Workspace:FindFirstChild(Player.Name):FindFirstChild("NullwareReanim") then
			Player.Character:BreakJoints()
		else
			ReanimateCharacter:FindFirstChild("HumanoidRootPart").Size = Vector3.new(2, 2, 1)
			_G.Bypass = true
			Player.Character = Character
			_G.Bypass = false
			Player.Character:BreakJoints()
			
			for _,DisabledAlign in pairs(DisabledAligns) do
				if DisabledAlign.Parent then
					DisabledAlign.Enabled = true
				end
			end
		end
	end)
	
	StarterGui:SetCore("ResetButtonCallback", ResetBindable)
end





--//====================================================\\--
--||			   CREATED BY SHACKLUSTER
--\\====================================================//--



wait(0.2)



Player = game:GetService("Players").LocalPlayer
PlayerGui = Player.PlayerGui
Cam = workspace.CurrentCamera
Backpack = Player.Backpack
Character = Player.Character
Humanoid = Character.Humanoid
Mouse = Player:GetMouse()
RootPart = Character["HumanoidRootPart"]
Torso = Character["Torso"]
Head = Character["Head"]
RightArm = Character["Right Arm"]
LeftArm = Character["Left Arm"]
RightLeg = Character["Right Leg"]
LeftLeg = Character["Left Leg"]
RootJoint = RootPart["RootJoint"]
Neck = Torso["Neck"]
RightShoulder = Torso["Right Shoulder"]
LeftShoulder = Torso["Left Shoulder"]
RightHip = Torso["Right Hip"]
LeftHip = Torso["Left Hip"]

IT = Instance.new
CF = CFrame.new
VT = Vector3.new
RAD = math.rad
C3 = Color3.new
UD2 = UDim2.new
BRICKC = BrickColor.new
ANGLES = CFrame.Angles
EULER = CFrame.fromEulerAnglesXYZ
COS = math.cos
ACOS = math.acos
SIN = math.sin
ASIN = math.asin
ABS = math.abs
MRANDOM = math.random
FLOOR = math.floor

function CreateMesh(MESH, PARENT, MESHTYPE, MESHID, TEXTUREID, SCALE, OFFSET)
	local NEWMESH = IT(MESH)
	if MESH == "SpecialMesh" then
		NEWMESH.MeshType = MESHTYPE
		if MESHID ~= "nil" and MESHID ~= "" then
			NEWMESH.MeshId = "http://www.roblox.com/asset/?id="..MESHID
		end
		if TEXTUREID ~= "nil" and TEXTUREID ~= "" then
			NEWMESH.TextureId = "http://www.roblox.com/asset/?id="..TEXTUREID
		end
	end
	NEWMESH.Offset = OFFSET or VT(0, 0, 0)
	NEWMESH.Scale = SCALE
	NEWMESH.Parent = PARENT
	return NEWMESH
end

function CreatePart(FORMFACTOR, PARENT, MATERIAL, REFLECTANCE, TRANSPARENCY, BRICKCOLOR, NAME, SIZE, ANCHOR)
	local NEWPART = IT("Part")
	NEWPART.formFactor = FORMFACTOR
	NEWPART.Reflectance = REFLECTANCE
	NEWPART.Transparency = TRANSPARENCY
	NEWPART.CanCollide = false
	NEWPART.Locked = true
	NEWPART.Anchored = true
	if ANCHOR == false then
		NEWPART.Anchored = false
	end
	NEWPART.BrickColor = BRICKC(tostring(BRICKCOLOR))
	NEWPART.Name = NAME
	NEWPART.Size = SIZE
	NEWPART.Position = Torso.Position
	NEWPART.Material = MATERIAL
	NEWPART:BreakJoints()
	NEWPART.Parent = PARENT
	return NEWPART
end

--//=================================\\
--||		  CUSTOMIZATION
--\\=================================//

Player_Size = 1 --Size of the player.
Animation_Speed = 3
Frame_Speed = 1 / 60 -- (1 / 30) OR (1 / 60)

local Speed = 16
local Effects2 = {}

--//=================================\\
--|| 	  END OF CUSTOMIZATION
--\\=================================//

	local function weldBetween(a, b)
	    local weldd = Instance.new("ManualWeld")
	    weldd.Part0 = a
	    weldd.Part1 = b
	    weldd.C0 = CFrame.new()
	    weldd.C1 = b.CFrame:inverse() * a.CFrame
	    weldd.Parent = a
	    return weldd
	end

--//=================================\\
--|| 	      USEFUL VALUES
--\\=================================//

local ROOTC0 = CF(0, 0, 0) * ANGLES(RAD(-90), RAD(0), RAD(180))
local NECKC0 = CF(0, 1, 0) * ANGLES(RAD(-90), RAD(0), RAD(180))
local RIGHTSHOULDERC0 = CF(-0.5, 0, 0) * ANGLES(RAD(0), RAD(90), RAD(0))
local LEFTSHOULDERC0 = CF(0.5, 0, 0) * ANGLES(RAD(0), RAD(-90), RAD(0))
local CHANGEDEFENSE = 0
local CHANGEDAMAGE = 0
local CHANGEMOVEMENT = 0
local ANIM = "Idle"
local ATTACK = false
local EQUIPPED = false
local HOLD = false
local COMBO = 1
local COMBO2 = 1
local Rooted = false
local SINE = 0
local KEYHOLD = false
local NOWALK = false
local CHANGE = 2 / Animation_Speed
local WALKINGANIM = false
local WALK = 0
local VALUE1 = false
local VALUE2 = false
local ROBLOXIDLEANIMATION = IT("Animation")
ROBLOXIDLEANIMATION.Name = "Roblox Idle Animation"
ROBLOXIDLEANIMATION.AnimationId = "http://www.roblox.com/asset/?id=180435571"
--ROBLOXIDLEANIMATION.Parent = Humanoid
local WEAPONGUI = IT("ScreenGui", PlayerGui)
WEAPONGUI.Name = "Weapon GUI"
local Weapon = IT("Model")
Weapon.Name = "Adds"
local Effects = IT("Folder", Weapon)
Effects.Name = "Effects"
local ANIMATOR = Humanoid.Animator
local ANIMATE = Character.Animate
local HITPLAYERSOUNDS = {--[["199149137", "199149186", "199149221", "199149235", "199149269", "199149297"--]]"263032172", "263032182", "263032200", "263032221", "263032252", "263033191"}
local HITARMORSOUNDS = {"199149321", "199149338", "199149367", "199149409", "199149452"}
local HITWEAPONSOUNDS = {"199148971", "199149025", "199149072", "199149109", "199149119"}
local HITBLOCKSOUNDS = {"199148933", "199148947"}
local UNANCHOR = true
local FISTS = {}
local LIGHT = IT("PointLight",Torso)
LIGHT.Range = 10
LIGHT.Brightness = 100
LIGHT.Color = C3(170/255, 85/255, 0)
local SKILL1COOLDOWN = 0
local SKILL2COOLDOWN = 0
local SKILL3COOLDOWN = 0

local SKILLTEXTCOLOR = BRICKC"Deep orange".Color

--//=================================\\
--\\=================================//


--//=================================\\
--|| SAZERENOS' ARTIFICIAL HEARTBEAT
--\\=================================//

ArtificialHB = Instance.new("BindableEvent", script)
ArtificialHB.Name = "ArtificialHB"

script:WaitForChild("ArtificialHB")

frame = Frame_Speed
tf = 0
allowframeloss = false
tossremainder = false
lastframe = tick()
script.ArtificialHB:Fire()

game:GetService("RunService").Heartbeat:connect(function(s, p)
	tf = tf + s
	if tf >= frame then
		if allowframeloss then
			script.ArtificialHB:Fire()
			lastframe = tick()
		else
			for i = 1, math.floor(tf / frame) do
				script.ArtificialHB:Fire()
			end
		lastframe = tick()
		end
		if tossremainder then
			tf = 0
		else
			tf = tf - frame * math.floor(tf / frame)
		end
	end
end)

--//=================================\\
--\\=================================//





--//=================================\\
--|| 	      SOME FUNCTIONS
--\\=================================//

function Raycast(POSITION, DIRECTION, RANGE, IGNOREDECENDANTS)
	return workspace:FindPartOnRay(Ray.new(POSITION, DIRECTION.unit * RANGE), IGNOREDECENDANTS)
end

function PositiveAngle(NUMBER)
	if NUMBER >= 0 then
		NUMBER = 0
	end
	return NUMBER
end

function NegativeAngle(NUMBER)
	if NUMBER <= 0 then
		NUMBER = 0
	end
	return NUMBER
end

function Swait(NUMBER)
	if NUMBER == 0 or NUMBER == nil then
		ArtificialHB.Event:wait()
	else
		for i = 1, NUMBER do
			ArtificialHB.Event:wait()
		end
	end
end

function QuaternionFromCFrame(cf)
	local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components()
	local trace = m00 + m11 + m22
	if trace > 0 then 
		local s = math.sqrt(1 + trace)
		local recip = 0.5 / s
		return (m21 - m12) * recip, (m02 - m20) * recip, (m10 - m01) * recip, s * 0.5
	else
		local i = 0
		if m11 > m00 then
			i = 1
		end
		if m22 > (i == 0 and m00 or m11) then
			i = 2
		end
		if i == 0 then
			local s = math.sqrt(m00 - m11 - m22 + 1)
			local recip = 0.5 / s
			return 0.5 * s, (m10 + m01) * recip, (m20 + m02) * recip, (m21 - m12) * recip
		elseif i == 1 then
			local s = math.sqrt(m11 - m22 - m00 + 1)
			local recip = 0.5 / s
			return (m01 + m10) * recip, 0.5 * s, (m21 + m12) * recip, (m02 - m20) * recip
		elseif i == 2 then
			local s = math.sqrt(m22 - m00 - m11 + 1)
			local recip = 0.5 / s return (m02 + m20) * recip, (m12 + m21) * recip, 0.5 * s, (m10 - m01) * recip
		end
	end
end
 
function QuaternionToCFrame(px, py, pz, x, y, z, w)
	local xs, ys, zs = x + x, y + y, z + z
	local wx, wy, wz = w * xs, w * ys, w * zs
	local xx = x * xs
	local xy = x * ys
	local xz = x * zs
	local yy = y * ys
	local yz = y * zs
	local zz = z * zs
	return CFrame.new(px, py, pz, 1 - (yy + zz), xy - wz, xz + wy, xy + wz, 1 - (xx + zz), yz - wx, xz - wy, yz + wx, 1 - (xx + yy))
end
 
function QuaternionSlerp(a, b, t)
	local cosTheta = a[1] * b[1] + a[2] * b[2] + a[3] * b[3] + a[4] * b[4]
	local startInterp, finishInterp;
	if cosTheta >= 0.0001 then
		if (1 - cosTheta) > 0.0001 then
			local theta = ACOS(cosTheta)
			local invSinTheta = 1 / SIN(theta)
			startInterp = SIN((1 - t) * theta) * invSinTheta
			finishInterp = SIN(t * theta) * invSinTheta
		else
			startInterp = 1 - t
			finishInterp = t
		end
	else
		if (1 + cosTheta) > 0.0001 then
			local theta = ACOS(-cosTheta)
			local invSinTheta = 1 / SIN(theta)
			startInterp = SIN((t - 1) * theta) * invSinTheta
			finishInterp = SIN(t * theta) * invSinTheta
		else
			startInterp = t - 1
			finishInterp = t
		end
	end
	return a[1] * startInterp + b[1] * finishInterp, a[2] * startInterp + b[2] * finishInterp, a[3] * startInterp + b[3] * finishInterp, a[4] * startInterp + b[4] * finishInterp
end

function Clerp(a, b, t)
	local qa = {QuaternionFromCFrame(a)}
	local qb = {QuaternionFromCFrame(b)}
	local ax, ay, az = a.x, a.y, a.z
	local bx, by, bz = b.x, b.y, b.z
	local _t = 1 - t
	return QuaternionToCFrame(_t * ax + t * bx, _t * ay + t * by, _t * az + t * bz, QuaternionSlerp(qa, qb, t))
end

function CreateFrame(PARENT, TRANSPARENCY, BORDERSIZEPIXEL, POSITION, SIZE, COLOR, BORDERCOLOR, NAME)
	local frame = IT("Frame")
	frame.BackgroundTransparency = TRANSPARENCY
	frame.BorderSizePixel = BORDERSIZEPIXEL
	frame.Position = POSITION
	frame.Size = SIZE
	frame.BackgroundColor3 = COLOR
	frame.BorderColor3 = BORDERCOLOR
	frame.Name = NAME
	frame.Parent = PARENT
	return frame
end

function CreateLabel(PARENT, TEXT, TEXTCOLOR, TEXTFONTSIZE, TEXTFONT, TRANSPARENCY, BORDERSIZEPIXEL, STROKETRANSPARENCY, NAME)
	local label = IT("TextLabel")
	label.BackgroundTransparency = 1
	label.Size = UD2(1, 0, 1, 0)
	label.Position = UD2(0, 0, 0, 0)
	label.TextColor3 = TEXTCOLOR
	label.TextStrokeTransparency = STROKETRANSPARENCY
	label.TextTransparency = TRANSPARENCY
	label.FontSize = TEXTFONTSIZE
	label.Font = TEXTFONT
	label.BorderSizePixel = BORDERSIZEPIXEL
	label.TextScaled = false
	label.Text = TEXT
	label.Name = NAME
	label.Parent = PARENT
	return label
end

function NoOutlines(PART)
	PART.TopSurface, PART.BottomSurface, PART.LeftSurface, PART.RightSurface, PART.FrontSurface, PART.BackSurface = 10, 10, 10, 10, 10, 10
end


function CreateWeldOrSnapOrMotor(TYPE, PARENT, PART0, PART1, C0, C1)
	local NEWWELD = IT(TYPE)
	NEWWELD.Part0 = PART0
	NEWWELD.Part1 = PART1
	NEWWELD.C0 = C0
	NEWWELD.C1 = C1
	NEWWELD.Parent = PARENT
	return NEWWELD
end

function CreateSound(ID, PARENT, VOLUME, PITCH)
	local NEWSOUND = nil
	coroutine.resume(coroutine.create(function()
		NEWSOUND = IT("Sound", PARENT)
		NEWSOUND.Volume = VOLUME
		NEWSOUND.Pitch = PITCH
		NEWSOUND.SoundId = "http://www.roblox.com/asset/?id="..ID
		Swait()
		NEWSOUND:play()
		game:GetService("Debris"):AddItem(NEWSOUND, 10)
	end))
	return NEWSOUND
end

function CFrameFromTopBack(at, top, back)
	local right = top:Cross(back)
	return CF(at.x, at.y, at.z, right.x, top.x, back.x, right.y, top.y, back.y, right.z, top.z, back.z)
end

function CreateWave(SIZE,WAIT,CFRAME,DOESROT,ROT,COLOR,GROW)
	local wave = CreatePart(3, Effects, "Neon", 0, 0.5, BRICKC(COLOR), "Effect", VT(0,0,0))
	local mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "20329976", "", SIZE, VT(0,0,-SIZE.X/8))
	wave.CFrame = CFRAME
	coroutine.resume(coroutine.create(function(PART)
		for i = 1, WAIT do
			Swait()
			mesh.Scale = mesh.Scale + GROW
			mesh.Offset = VT(0,0,-(mesh.Scale.X/8))
			if DOESROT == true then
				wave.CFrame = wave.CFrame * CFrame.fromEulerAnglesXYZ(0,ROT,0)
			end
			wave.Transparency = wave.Transparency + (0.5/WAIT)
			if wave.Transparency > 0.99 then
				wave:remove()
			end
		end
	end))
end

function CreateSwirl(SIZE,WAIT,CFRAME,DOESROT,ROT,COLOR,GROW)
	local wave = CreatePart(3, Effects, "Neon", 0, 0.5, BRICKC(COLOR), "Effect", VT(0,0,0))
	local mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "1051557", "", SIZE, VT(0,0,0))
	wave.CFrame = CFRAME
	coroutine.resume(coroutine.create(function(PART)
		for i = 1, WAIT do
			Swait()
			mesh.Scale = mesh.Scale + GROW
			mesh.Offset = VT(0,0,-(mesh.Scale.X/8))
			if DOESROT == true then
				wave.CFrame = wave.CFrame * CFrame.fromEulerAnglesXYZ(0,ROT,0)
			end
			wave.Transparency = wave.Transparency + (0.5/WAIT)
			if wave.Transparency > 0.99 then
				wave:remove()
			end
		end
	end))
end

function CreateRing(SIZE,DOESROT,ROT,WAIT,CFRAME,COLOR,GROW)
	local wave = CreatePart(3, Effects, "Neon", 0, 0.5, BRICKC(COLOR), "Effect", VT(0,0,0))
	local mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "559831844", "", SIZE, VT(0,0,0))
	wave.CFrame = CFRAME
	coroutine.resume(coroutine.create(function(PART)
		for i = 1, WAIT do
			Swait()
			mesh.Scale = mesh.Scale + GROW
			if DOESROT == true then
				wave.CFrame = wave.CFrame * CFrame.fromEulerAnglesXYZ(0,ROT,0)
			end
			wave.Transparency = wave.Transparency + (0.5/WAIT)
			if wave.Transparency > 0.99 then
				wave:remove()
			end
		end
	end))
end

function MagicSphere(SIZE,WAIT,CFRAME,COLOR,GROW)
	local wave = CreatePart(3, Effects, "Neon", 0, 0, BRICKC(COLOR), "Effect", VT(1,1,1), true)
	local mesh = CreateMesh("SpecialMesh", wave, "Sphere", "", "", SIZE, VT(0,0,0))
	wave.CFrame = CFRAME
	coroutine.resume(coroutine.create(function(PART)
		for i = 1, WAIT do
			Swait()
			mesh.Scale = mesh.Scale + GROW
			wave.Transparency = wave.Transparency + (1/WAIT)
			if wave.Transparency > 0.99 then
				wave:remove()
			end
		end
	end))
end

function Slice(KIND,SIZE,WAIT,CFRAME,COLOR,GROW)
	local wave = CreatePart(3, Effects, "Neon", 0, 0.5, BRICKC(COLOR), "Effect", VT(1,1,1), true)
	local mesh = nil
	if KIND == "Base" then
 		mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "448386996", "", VT(0,SIZE/10,SIZE/10), VT(0,0,0))
	elseif KIND == "Thin" then
 		mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "662586858", "", VT(SIZE/10,0,SIZE/10), VT(0,0,0))
	elseif KIND == "Round" then
 		mesh = CreateMesh("SpecialMesh", wave, "FileMesh", "662585058", "", VT(SIZE/10,0,SIZE/10), VT(0,0,0))
	end
	wave.CFrame = CFRAME
	coroutine.resume(coroutine.create(function(PART)
		for i = 1, WAIT do
			Swait()
			mesh.Scale = mesh.Scale + GROW/10
			wave.Transparency = wave.Transparency + (0.5/WAIT)
			if wave.Transparency > 0.99 then
				wave:remove()
			end
		end
	end))
end

function MakeForm(PART,TYPE)
	if TYPE == "Cyl" then
		local MSH = IT("CylinderMesh",PART)
	elseif TYPE == "Ball" then
		local MSH = IT("SpecialMesh",PART)
		MSH.MeshType = "Sphere"
	elseif TYPE == "Wedge" then
		local MSH = IT("SpecialMesh",PART)
		MSH.MeshType = "Wedge"
	end
end

function CheckTableForString(Table, String)
	for i, v in pairs(Table) do
		if string.find(string.lower(String), string.lower(v)) then
			return true
		end
	end
	return false
end

function CheckIntangible(Hit)
	local ProjectileNames = {"Water", "Arrow", "Projectile", "Effect", "Rail", "Lightning", "Bullet"}
	if Hit and Hit.Parent then
		if ((not Hit.CanCollide or CheckTableForString(ProjectileNames, Hit.Name)) and not Hit.Parent:FindFirstChild("Humanoid")) then
			return true
		end
	end
	return false
end

Debris = game:GetService("Debris")

function CastZapRay(StartPos, Vec, Length, Ignore, DelayIfHit)
	local Direction = CFrame.new(StartPos, Vec).lookVector
	local Ignore = ((type(Ignore) == "table" and Ignore) or {Ignore})
	local RayHit, RayPos, RayNormal = game:GetService("Workspace"):FindPartOnRayWithIgnoreList(Ray.new(StartPos, Direction * Length), Ignore)
	if RayHit and CheckIntangible(RayHit) then
		if DelayIfHit then
			wait()
		end
		RayHit, RayPos, RayNormal = CastZapRay((RayPos + (Vec * 0.01)), Vec, (Length - ((StartPos - RayPos).magnitude)), Ignore, DelayIfHit)
	end
	return RayHit, RayPos, RayNormal
end

function turnto(position)
	RootPart.CFrame=CFrame.new(RootPart.CFrame.p,VT(position.X,RootPart.Position.Y,position.Z)) * CFrame.new(0, 0, 0)
end

--//=================================\\
--||	     WEAPON CREATION
--\\=================================//

local EyeSizes={
	NumberSequenceKeypoint.new(0,0.65,0),
	NumberSequenceKeypoint.new(0.5,0.7,0),
	NumberSequenceKeypoint.new(1,0,0)
}
local EyeTrans={
	NumberSequenceKeypoint.new(0,0,0),
	NumberSequenceKeypoint.new(0.5,0,0),
	NumberSequenceKeypoint.new(1,1,0)
}
local PE=Instance.new("ParticleEmitter")
PE.LightEmission=.9
PE.Color = ColorSequence.new(BRICKC("Deep orange").Color,BRICKC("Really red").Color)
PE.Size=NumberSequence.new(EyeSizes)
PE.Transparency=NumberSequence.new(EyeTrans)
PE.Lifetime=NumberRange.new(0.35)
PE.Rotation=NumberRange.new(0,360)
PE.Rate=999
PE.VelocitySpread = 10000
PE.Acceleration = Vector3.new(0,25,0)
PE.ZOffset = 0.5
PE.Drag = 0
PE.Speed = NumberRange.new(0,0,0)
PE.Texture="rbxasset://textures/particles/explosion01_implosion_main.dds"
PE.Name = "PE"
PE.Enabled = true
PE.LockedToPart = true

function fireparticles(art)
	local PARTICLES = PE:Clone()
	PARTICLES.Parent = art
end

local FIST = CreatePart(3, Weapon, "Neon", 0, 1, "Really black", "Fire", VT(1,0,1),false)
FIST.CanCollide = true
CreateWeldOrSnapOrMotor("Weld", FIST, RightArm, FIST, CF(0, -1, 0), CF(0, 0.0, 0))
fireparticles(FIST)
table.insert(FISTS,FIST)

local FIST = CreatePart(3, Weapon, "Neon", 0, 1, "Really black", "Fire", VT(1,0,1),false)
FIST.CanCollide = true
CreateWeldOrSnapOrMotor("Weld", FIST, LeftArm, FIST, CF(0, -1, 0), CF(0, 0.0, 0))
fireparticles(FIST)
table.insert(FISTS,FIST)

local FIST = CreatePart(3, Weapon, "Neon", 0, 1, "Really black", "Fire", VT(1,0,1),false)
FIST.CanCollide = true
CreateWeldOrSnapOrMotor("Weld", FIST, RightLeg, FIST, CF(0, -1, 0), CF(0, 0.0, 0))
fireparticles(FIST)
table.insert(FISTS,FIST)

local FIST = CreatePart(3, Weapon, "Neon", 0, 1, "Really black", "Fire", VT(1,0,1),false)
FIST.CanCollide = true
CreateWeldOrSnapOrMotor("Weld", FIST, LeftLeg, FIST, CF(0, -1, 0), CF(0, 0.0, 0))
fireparticles(FIST)
table.insert(FISTS,FIST)

for e=1,#FISTS do
	if FISTS[e]~=nil then
		local Thing=FISTS[e]
		if Thing~=nil then
			local TOUCHED = Thing.Touched:Connect(function(hit)
				if VALUE1 == true and VALUE2 == false then
					if hit ~= nil then
						if hit.Parent:FindFirstChildOfClass("Humanoid") then
							VALUE2 = true
							ApplyDamage(hit.Parent:FindFirstChildOfClass("Humanoid"),MRANDOM(0,0),0,0)
							CreateSound("131237241", hit, 1, MRANDOM(0,0)/0)
							wait(0.15)
							VALUE2 = false
						end
					end
				end
			end)
		end
	end
end

for _, c in pairs(Weapon:GetChildren()) do
	if c.ClassName == "Part" then
		c.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
	end
end

Weapon.Parent = Character

Humanoid.Died:connect(function()
	ATTACK = true
end)



--//=================================\\
--||	     DAMAGE FUNCTIONS
--\\=================================//

function StatLabel(CFRAME, TEXT, COLOR)
	local STATPART = CreatePart(3, Effects, "SmoothPlastic", 0, 1, "Really black", "Effect", VT())
	STATPART.CFrame = CF(CFRAME.p,CFRAME.p+VT(MRANDOM(-0,0),MRANDOM(0,0),MRANDOM(-0,0)))
	local BODYGYRO = IT("BodyGyro", STATPART)
	game:GetService("Debris"):AddItem(STATPART ,5)
	local BILLBOARDGUI = Instance.new("BillboardGui", STATPART)
	BILLBOARDGUI.Adornee = STATPART
	BILLBOARDGUI.Size = UD2(2.5, 0, 2.5 ,0)
	BILLBOARDGUI.StudsOffset = VT(-2, 2, 0)
	BILLBOARDGUI.AlwaysOnTop = false
	local TEXTLABEL = Instance.new("TextLabel", BILLBOARDGUI)
	TEXTLABEL.BackgroundTransparency = 1
	TEXTLABEL.Size = UD2(2.5, 0, 2.5, 0)
	TEXTLABEL.Text = TEXT
	TEXTLABEL.Font = "Fantasy"
	TEXTLABEL.FontSize="Size42"
	TEXTLABEL.TextColor3 = COLOR
	TEXTLABEL.TextStrokeTransparency = 1
	TEXTLABEL.TextScaled = true
	TEXTLABEL.TextWrapped = true
	coroutine.resume(coroutine.create(function(THEPART, THEBODYPOSITION, THETEXTLABEL)
		for i = 1, 50 do
			Swait()
			STATPART.CFrame = STATPART.CFrame * CF(0,0,-0.2)
			TEXTLABEL.TextTransparency = TEXTLABEL.TextTransparency + (1/50)
			TEXTLABEL.TextStrokeTransparency = TEXTLABEL.TextTransparency
		end
		THEPART.Parent = nil
	end),STATPART, TEXTLABEL)
end

--//=================================\\
--||			DAMAGING
--\\=================================//



		

--//=================================\\
--||	ATTACK FUNCTIONS AND STUFF
--\\=================================//

function BurningPunches()
	ATTACK = true
	Rooted = false
	for i=0, 0.2, 0.1 / Animation_Speed do
		Swait()
		RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(0), RAD(0), RAD(0)), 1 / Animation_Speed)
		Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(0)), 1 / Animation_Speed)
		RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.35, 0+ 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(150), RAD(35), RAD(-5)) * RIGHTSHOULDERC0, 0.15 / Animation_Speed)
		LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.35, 0 + 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(130), RAD(0), RAD(5)) * LEFTSHOULDERC0, 0.15 / Animation_Speed)
		RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 1 / Animation_Speed)
		LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 1 / Animation_Speed)
	end
	VALUE1 = true
	if COMBO == 1 then
		COMBO = 2
		CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
		for i=0, 0.3, 0.1 / Animation_Speed do
			Swait()
			RootPart.CFrame = RootPart.CFrame*CF(0,0,-0.1)
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(0), RAD(0), RAD(-75)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(65)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(-25), RAD(0), RAD(12)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1, 0.5, -0.5) * ANGLES(RAD(90), RAD(0), RAD(25)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
	elseif COMBO == 2 then
		COMBO = 1
		CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
		for i=0, 0.3, 0.1 / Animation_Speed do
			Swait()
			RootPart.CFrame = RootPart.CFrame*CF(0,0,-0.1)
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(0), RAD(0), RAD(85)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(-80)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -1) * ANGLES(RAD(90), RAD(0), RAD(-50)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(-25), RAD(0), RAD(-12)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
	end
	VALUE1 = false
	ATTACK = false
	Rooted = false
end

function BurningKicks()
	ATTACK = true
	Rooted = false
	VALUE1 = true
	NOWALK = true
	if COMBO2 == 1 then
		COMBO2 = 2
		CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
		for i=0, 0.5, 0.1 / Animation_Speed do
			Swait()
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(-25), RAD(0), RAD(45)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(15), RAD(0), RAD(-45)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(12)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, -0.5) * ANGLES(RAD(25), RAD(0), RAD(45)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -0.8 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(45), RAD(90), RAD(0)) * ANGLES(RAD(-38), RAD(0), RAD(0)), 2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
	elseif COMBO2 == 2 then
		COMBO2 = 1
		CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
		for i=0, 0.5, 0.1 / Animation_Speed do
			Swait()
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(-25), RAD(0), RAD(-45)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(15), RAD(0), RAD(45)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -0.5) * ANGLES(RAD(25), RAD(0), RAD(-45)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(-12)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -0.8 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(45), RAD(-90), RAD(0)) * ANGLES(RAD(-38), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
	end
	NOWALK = false
	VALUE1 = false
	ATTACK = false
	Rooted = false
end

function FireField()
	local HITFLOOR,HITPOS,NORMAL = Raycast(RootPart.Position, (CF(RootPart.Position, RootPart.Position + VT(0, -1, 0))).lookVector, 6 * Player_Size, Character)
	if HITFLOOR ~= nil then
		ATTACK = true
		Rooted = false
		for i=0, 1, 0.1 / Animation_Speed do
			Swait()
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.05 * COS(SINE / 12)) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.15 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(-5 - 2.5 * SIN(SINE / 12)), RAD(-15), RAD(0)), 0.15 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(180), RAD(0), RAD(12)) * RIGHTSHOULDERC0, 0.15 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(-12)) * LEFTSHOULDERC0, 0.15 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 0.15 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.05 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 0.15 / Animation_Speed)
		end
		Rooted = true
		CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
		for i=0, 0.3, 0.1 / Animation_Speed do
			Swait()
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, -0.8) * ANGLES(RAD(75), RAD(0), RAD(0)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(15), RAD(0), RAD(0)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -1.2) * ANGLES(RAD(90), RAD(0), RAD(-12)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(-25), RAD(0), RAD(-12)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -0.3, -0.4) * ANGLES(RAD(65), RAD(45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)),2  / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -0.3, -0.4) * ANGLES(RAD(65), RAD(-45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
		local HITFLOOR,HITPOS,NORMAL = Raycast(RootPart.Position, (CF(RootPart.Position, RootPart.Position + VT(0, -1, 0))).lookVector, 6 * Player_Size, Character)
		MagicSphere(VT(3,3,3),45,CF(HITPOS),SKILLTEXTCOLOR,VT(1,1,1))
		CreateSound("438666542", Torso, 1, MRANDOM(11,13)/10)
		AoEDamage(HITPOS,35,25,30,15,2,2,true)
		AoEDamage(HITPOS,25,15,20,15,2,2,false)
		for i = 1, 7 do
			Slice("Thin",0.4,35,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(0),RAD(0))*ANGLES(RAD(MRANDOM(-48,48)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-48,48))),"Deep orange",VT(0.1,0,0.1))
			Slice("Round",0.4,45,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(MRANDOM(-180,180)),RAD(0)),"Deep orange",VT(0.1,0,0.1))
		end
		for i=0, 0.7, 0.1 / Animation_Speed do
			Swait()
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, -0.8) * ANGLES(RAD(75), RAD(0), RAD(0)), 2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(15), RAD(0), RAD(0)), 2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -1.2) * ANGLES(RAD(90), RAD(0), RAD(-12)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(-25), RAD(0), RAD(-12)) * LEFTSHOULDERC0, 2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -0.3, -0.4) * ANGLES(RAD(65), RAD(45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)),2  / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -0.3, -0.4) * ANGLES(RAD(65), RAD(-45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		end
		ATTACK = false
		Rooted = false
	end
end

function BlazingDash()
	ATTACK = true
	Rooted = true
	MagicSphere(VT(3,3,3),45,CF(Torso.Position),SKILLTEXTCOLOR,VT(1,1,1))
	CreateSound("438666542", Torso, 1, MRANDOM(11,13)/10)
	AoEDamage(Torso.Position,25,15,20,15,2,2,false)
	for i=0, 1, 0.1 / Animation_Speed do
		Swait()
		for e=1,#FISTS do
			if FISTS[e]~=nil then
				local Thing=FISTS[e]
				if Thing~=nil then
					if MRANDOM(1,2) == 1 then
						Slice("Thin",0.4,15,Thing.CFrame*ANGLES(RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180))),"Deep orange",VT(-0.01,0,-0.01))
					end
				end
			end
		end
		AoEDamage(Torso.Position,15,2,2,15,2,2,false)
		local HITFLOOR,HITPOS,NORMAL = Raycast(RootPart.Position, (CF(RootPart.Position, RootPart.Position + VT(0, -1, 0))).lookVector, 6 * Player_Size, Character)
		if HITFLOOR then
			AoEDamage(Torso.Position,25,1,1,15,2,2,true)
			Slice("Thin",0.4,75,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(0),RAD(0))*ANGLES(RAD(MRANDOM(-18,18)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-18,18))),"Deep orange",VT(-0.001,0,-0.001))
			Slice("Round",0.4,75,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(MRANDOM(-180,180)),RAD(0)),"Deep orange",VT(-0.01,0,-0.01))
		end
		RootPart.CFrame = RootPart.CFrame * CF(0,0,-4)
		RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(25), RAD(0), RAD(0)), 2 / Animation_Speed)
		Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(0)), 2 / Animation_Speed)
		RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(12)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
		LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.25, 0.5, -1) * ANGLES(RAD(0), RAD(0), RAD(90)) * LEFTSHOULDERC0, 2 / Animation_Speed)
		RightHip.C0 = Clerp(RightHip.C0, CF(1, -0.5, -0.5) * ANGLES(RAD(-25), RAD(90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
		LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1, -0.1) * ANGLES(RAD(-5), RAD(-90), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 2 / Animation_Speed)
	end
	ATTACK = false
	Rooted = false
end

function DancingInferno()
	ATTACK = true
	Rooted = true
	Humanoid.HipHeight = 2
	local GYRO = IT("BodyGyro",RootPart)
	GYRO.D = 100
	GYRO.P = 2000
	GYRO.MaxTorque = VT(0,4000000,0)
	GYRO.cframe = CF(RootPart.Position,Mouse.Hit.p)
	local BULLET = CreatePart(3, Effects, "Neon", 0, 0, "Deep orange", "Dancing Inferno", VT(0,0,0))
	MakeForm(BULLET,"Ball")
	CreateSound("463598785", BULLET, 3, MRANDOM(11,13)/10)
	for i=0, 3, 0.1 / Animation_Speed do
		Swait()
		Slice("Thin",0.4,35,Torso.CFrame*CF(0,0,-2)*ANGLES(RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180))),"Deep orange",VT(-0.01,0,-0.01))
		GYRO.cframe = CF(RootPart.Position,Mouse.Hit.p)
		BULLET.CFrame = Torso.CFrame * CF(0,0,-2)
		BULLET.Size = BULLET.Size + VT(0.02,0.02,0.02)
		RootJoint.C0 = Clerp(RootJoint.C0, ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
		Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0) * ANGLES(RAD(25), RAD(0), RAD(0)), 0.2 / Animation_Speed)
		RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -0.5) * ANGLES(RAD(65), RAD(0), RAD(15)) * RIGHTSHOULDERC0, 1 / Animation_Speed)
		LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, -0.5) * ANGLES(RAD(65), RAD(0), RAD(-15)) * LEFTSHOULDERC0, 1 / Animation_Speed)
		RightHip.C0 = Clerp(RightHip.C0, CF(1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(90), RAD(-5), RAD(40)), 0.2 / Animation_Speed)
		LeftHip.C0 = Clerp(LeftHip.C0, CF(-1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(90), RAD(25), RAD(-35)), 0.2 / Animation_Speed)
	end
	GYRO:remove()
	for i=0, 1, 0.1 / Animation_Speed do
		Swait()
		RootJoint.C0 = Clerp(RootJoint.C0, ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(0), RAD(0), RAD(25)), 0.2 / Animation_Speed)
		Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0) * ANGLES(RAD(25), RAD(0), RAD(-25)), 0.2 / Animation_Speed)
		RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -0.5) * ANGLES(RAD(0), RAD(0), RAD(15)) * RIGHTSHOULDERC0, 1 / Animation_Speed)
		LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, -0.5) * ANGLES(RAD(90), RAD(0), RAD(-15)) * LEFTSHOULDERC0, 1 / Animation_Speed)
		RightHip.C0 = Clerp(RightHip.C0, CF(1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(90), RAD(-5), RAD(40)), 0.2 / Animation_Speed)
		LeftHip.C0 = Clerp(LeftHip.C0, CF(-1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(90), RAD(25), RAD(-35)), 0.2 / Animation_Speed)
	end
	CreateSound("199150686", Torso, 1, MRANDOM(11,13)/10)
	CreateSound("438666542", BULLET, 3, MRANDOM(11,13)/10)
	local DISTANCE = (RootPart.Position - Mouse.Hit.p).Magnitude
	coroutine.resume(coroutine.create(function()
		local IMPACT = false
		BULLET.CFrame = CF(BULLET.Position,RootPart.CFrame*CF(0,-1,-DISTANCE).p)
		for i = 1, 500 do
			Swait()
			BULLET.CFrame = BULLET.CFrame * CF(0,-0.1,-3)
			CreateRing(VT(1,1,0),false,0,15,BULLET.CFrame * ANGLES(RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180))),SKILLTEXTCOLOR,VT(-0.12,-0.12,0))
			local HIT = Raycast(BULLET.Position, BULLET.CFrame.lookVector, 4, Character)
			if HIT ~= nil then
				IMPACT = true
				break
			end
			local HIT = Raycast(BULLET.Position, CF(BULLET.Position,BULLET.Position+VT(0,-1,0)).lookVector, 1, Character)
			if HIT ~= nil then
				IMPACT = true
				break
			end
		end
		if IMPACT == false then
			for i = 1, 40 do
				Swait()
				BULLET.Size = BULLET.Size * 0.9
			end
			BULLET:remove()
		else
			local HITFLOOR,HITPOS,NORMAL = Raycast(BULLET.Position+VT(0,1,0), (CF(BULLET.Position, BULLET.Position + VT(0, -1, 0))).lookVector, 6 * Player_Size, Character)
			for i = 1, 8 do
				for i = 1, 55 do
					Swait()
					if HITFLOOR then
						Slice("Thin",2,35,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(0),RAD(0))*ANGLES(RAD(MRANDOM(-18,18)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-18,18))),"Deep orange",VT(0.001,0,0.001))
						Slice("Round",2,45,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(MRANDOM(-180,180)),RAD(0)),"Deep orange",VT(0,0,0))
					end
					CreateRing(VT(0,0,0),false,0,15,BULLET.CFrame * ANGLES(RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-180,180))),SKILLTEXTCOLOR,VT(-0.12,-0.12,0))
				end
				AoEDamage(BULLET.Position,45,15,20,0,2,2,false)
				AoEDamage(BULLET.Position,75,5,5,-25,2,2,true)
				for i = 1, 3 do
					MagicSphere(VT(3,3,3),45,CF(BULLET.Position),SKILLTEXTCOLOR,VT(1,1,1))
					MagicSphere(VT(2,2,2),45,CF(BULLET.Position),SKILLTEXTCOLOR,VT(1.1,1.1,1.1))
					CreateSound("438666542", BULLET, 1, MRANDOM(11,13)/10)
					for i = 1, 2 do
						Slice("Thin",0.4,65,CF(BULLET.Position)*ANGLES(RAD(MRANDOM(-48,48)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-48,48))),"Deep orange",VT(0.1,0,0.1))
						if HITFLOOR then
							Slice("Round",0.4,i*12,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(MRANDOM(-180,180)),RAD(0)),"Deep orange",VT(0.1,0,0.1))
						end
					end
				end
			end
			BULLET.Transparency = 1
			Debris:AddItem(BULLET,5)
		end
	end))
	for i=0, 0.3, 0.1 / Animation_Speed do
		Swait()
		RootJoint.C0 = Clerp(RootJoint.C0, ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(0), RAD(0), RAD(-25)), 2 / Animation_Speed)
		Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0) * ANGLES(RAD(25), RAD(0), RAD(25)), 1.8 / Animation_Speed)
		RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, -0.5) * ANGLES(RAD(0), RAD(0), RAD(15)) * RIGHTSHOULDERC0, 2 / Animation_Speed)
		LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, -0.5) * ANGLES(RAD(90), RAD(0), RAD(65)) * LEFTSHOULDERC0, 2 / Animation_Speed)
		RightHip.C0 = Clerp(RightHip.C0, CF(1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(90), RAD(-5), RAD(40)), 2 / Animation_Speed)
		LeftHip.C0 = Clerp(LeftHip.C0, CF(-1.6, -1.5, -0.1) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(90), RAD(25), RAD(-35)), 2 / Animation_Speed)
	end
	Humanoid.HipHeight = 0
	ATTACK = false
	Rooted = false
end

--//=================================\\
--||	  ASSIGN THINGS TO KEYS
--\\=================================//

function MouseDown(Mouse)
	if ATTACK == false then
	end
end

function MouseUp(Mouse)
HOLD = false
end

function KeyDown(Key)
	KEYHOLD = true
	if Key == "z" and ATTACK == false then
		BurningPunches()
	end

	if Key == "b" and ATTACK == false then
		BurningKicks()
	end

	if Key == "123123123123123123123123123123" and ATTACK == false and SKILL1COOLDOWN < 1 then
		FireField()
		SKILL1COOLDOWN = 100
	end

	if Key == "2ddddddd1231233312333" and ATTACK == false and SKILL2COOLDOWN < 1 then
		BlazingDash()
		SKILL2COOLDOWN = 150
	end

	if Key == "x" and ATTACK == false and SKILL3COOLDOWN < 1 then
		DancingInferno()
		SKILL3COOLDOWN = 100
	end
end

function KeyUp(Key)
	KEYHOLD = false
end

	Mouse.Button1Down:connect(function(NEWKEY)
		MouseDown(NEWKEY)
	end)
	Mouse.Button1Up:connect(function(NEWKEY)
		MouseUp(NEWKEY)
	end)
	Mouse.KeyDown:connect(function(NEWKEY)
		KeyDown(NEWKEY)
	end)
	Mouse.KeyUp:connect(function(NEWKEY)
		KeyUp(NEWKEY)
	end)

--//=================================\\
--\\=================================//


function unanchor()
	if UNANCHOR == true then
		g = Character:GetChildren()
		for i = 1, #g do
			if g[i].ClassName == "Part" then
				g[i].Anchored = false
			end
		end
	end
end


--//=================================\\
--||	WRAP THE WHOLE SCRIPT UP
--\\=================================//

Humanoid.Changed:connect(function(Jump)
	if Jump == "Jump" and (Disable_Jump == true) then
		Humanoid.Jump = false
	end
end)

while true do
	Swait()
	ANIMATE.Parent = nil
	local IDLEANIMATION = Humanoid:LoadAnimation(ROBLOXIDLEANIMATION)
	IDLEANIMATION:Play()
	SINE = SINE + CHANGE
	local TORSOVELOCITY = (RootPart.Velocity * VT(1, 0, 1)).magnitude
	local TORSOVERTICALVELOCITY = RootPart.Velocity.y
	local LV = Torso.CFrame:pointToObjectSpace(Torso.Velocity - Torso.Position)
	local HITFLOOR,HITPOS,NORMAL = Raycast(RootPart.Position, (CF(RootPart.Position, RootPart.Position + VT(0, -1, 0))).lookVector, 4+Humanoid.HipHeight * Player_Size, Character)
	local WALKSPEEDVALUE = 6 / (Humanoid.WalkSpeed / 16)
	if ANIM == "Walk" and TORSOVELOCITY > 1 and NOWALK == false then
		RootJoint.C1 = Clerp(RootJoint.C1, ROOTC0 * CF(0, 0, -0.15 * COS(SINE / (WALKSPEEDVALUE / 2)) * Player_Size) * ANGLES(RAD(0), RAD(0) - RootPart.RotVelocity.Y / 75, RAD(0)), 2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
		Neck.C1 = Clerp(Neck.C1, CF(0 * Player_Size, -0.5 * Player_Size, 0 * Player_Size) * ANGLES(RAD(-90), RAD(0), RAD(180)) * ANGLES(RAD(2.5 * SIN(SINE / (WALKSPEEDVALUE / 2))), RAD(0), RAD(0) - Head.RotVelocity.Y / 30), 0.2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
		RightHip.C1 = Clerp(RightHip.C1, CF(0.5 * Player_Size, 0.875 * Player_Size - 0.125 * SIN(SINE / WALKSPEEDVALUE) * Player_Size, -0.125 * COS(SINE / WALKSPEEDVALUE) * Player_Size) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(0) - RightLeg.RotVelocity.Y / 75, RAD(0), RAD(76 * COS(SINE / WALKSPEEDVALUE))), 0.2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
		LeftHip.C1 = Clerp(LeftHip.C1, CF(-0.5 * Player_Size, 0.875 * Player_Size + 0.125 * SIN(SINE / WALKSPEEDVALUE) * Player_Size, 0.125 * COS(SINE / WALKSPEEDVALUE) * Player_Size) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(0) + LeftLeg.RotVelocity.Y / 75, RAD(0), RAD(76 * COS(SINE / WALKSPEEDVALUE))), 0.2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
	elseif (ANIM ~= "Walk") or (TORSOVELOCITY < 1) or NOWALK == true then
		RootJoint.C1 = Clerp(RootJoint.C1, ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
		Neck.C1 = Clerp(Neck.C1, CF(0 * Player_Size, -0.5 * Player_Size, 0 * Player_Size) * ANGLES(RAD(-90), RAD(0), RAD(180)) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
		RightHip.C1 = Clerp(RightHip.C1, CF(0.5 * Player_Size, 1 * Player_Size, 0 * Player_Size) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
		LeftHip.C1 = Clerp(LeftHip.C1, CF(-0.5 * Player_Size, 1 * Player_Size, 0 * Player_Size) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
	end
	if TORSOVERTICALVELOCITY > 1 and HITFLOOR == nil then
		ANIM = "Jump"
		if ATTACK == false then
			RootJoint.C0 = Clerp(RootJoint.C0, ROOTC0 * CF(0, 0, 0) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0 * Player_Size, 0 + ((1) - 1)) * ANGLES(RAD(-20), RAD(0), RAD(0)), 0.2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(-40), RAD(0), RAD(20)) * RIGHTSHOULDERC0, 0.2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(-40), RAD(0), RAD(-20)) * LEFTSHOULDERC0, 0.2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1, -0.3) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(-5), RAD(0), RAD(-20)), 0.2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1, -0.3) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(-5), RAD(0), RAD(20)), 0.2 / Animation_Speed)
	    end
	elseif TORSOVERTICALVELOCITY < -1 and HITFLOOR == nil then
		ANIM = "Fall"
		if ATTACK == false then
			RootJoint.C0 = Clerp(RootJoint.C0, ROOTC0 * CF(0, 0, 0 ) * ANGLES(RAD(0), RAD(0), RAD(0)), 0.2 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0 , 0 + ((1) - 1)) * ANGLES(RAD(20), RAD(0), RAD(0)), 0.2 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(60)) * RIGHTSHOULDERC0, 0.2 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.5, 0.5, 0) * ANGLES(RAD(0), RAD(0), RAD(-60)) * LEFTSHOULDERC0, 0.2 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1, 0) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(20)), 0.2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1, 0) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(10)), 0.2 / Animation_Speed)
		end
	elseif TORSOVELOCITY < 1 and HITFLOOR ~= nil then
		ANIM = "Idle"
		if ATTACK == false then
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.15 * COS(SINE / 12)) * ANGLES(RAD(0), RAD(0), RAD(45)), 0.15 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(-45)), 0.15 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.35, 0+ 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(150), RAD(35), RAD(-5)) * RIGHTSHOULDERC0, 0.15 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.35, 0 + 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(130), RAD(0), RAD(5)) * LEFTSHOULDERC0, 0.15 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1, -1 - 0.15 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(45), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 0.15 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.15 * COS(SINE / 12), -0.01) * ANGLES(RAD(0), RAD(-76), RAD(0)) * ANGLES(RAD(-8), RAD(0), RAD(0)), 0.15 / Animation_Speed)
		end
	elseif (TORSOVELOCITY > 1 and HITFLOOR ~= nil) and NOWALK == false then
		ANIM = "Walk"
		WALK = WALK + 1 / Animation_Speed
		if WALK >= 15 - (5 * (Humanoid.WalkSpeed / 16 / Player_Size)) then
			WALK = 0
			if WALKINGANIM == true then
				WALKINGANIM = false
			elseif WALKINGANIM == false then
				WALKINGANIM = true
			end
		end
		--RightHip.C1 = Clerp(RightHip.C1, CF(0.5 * Player_Size, 0.875 * Player_Size - 0.125 * SIN(SINE / WALKSPEEDVALUE) * Player_Size, -0.125 * COS(SINE / WALKSPEEDVALUE) * Player_Size) * ANGLES(RAD(0), RAD(90), RAD(0)) * ANGLES(RAD(0) - RightLeg.RotVelocity.Y / 75, RAD(0), RAD(60 * COS(SINE / WALKSPEEDVALUE))), 0.2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
		--LeftHip.C1 = Clerp(LeftHip.C1, CF(-0.5 * Player_Size, 0.875 * Player_Size + 0.125 * SIN(SINE / WALKSPEEDVALUE) * Player_Size, 0.125 * COS(SINE / WALKSPEEDVALUE) * Player_Size) * ANGLES(RAD(0), RAD(-90), RAD(0)) * ANGLES(RAD(0) + LeftLeg.RotVelocity.Y / 75, RAD(0), RAD(60 * COS(SINE / WALKSPEEDVALUE))), 0.2 * (Humanoid.WalkSpeed / 16) / Animation_Speed)
		if ATTACK == false then
			RootJoint.C0 = Clerp(RootJoint.C0,ROOTC0 * CF(0, 0, 0 + 0.15 * COS(SINE / 12)) * ANGLES(RAD(5), RAD(0), RAD(25)), 0.15 / Animation_Speed)
			Neck.C0 = Clerp(Neck.C0, NECKC0 * CF(0, 0, 0 + ((1) - 1)) * ANGLES(RAD(0 - 2.5 * SIN(SINE / 12)), RAD(0), RAD(-25)), 0.15 / Animation_Speed)
			RightShoulder.C0 = Clerp(RightShoulder.C0, CF(1.35, 0+ 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(150), RAD(35), RAD(-5)) * RIGHTSHOULDERC0, 0.15 / Animation_Speed)
			LeftShoulder.C0 = Clerp(LeftShoulder.C0, CF(-1.35, 0 + 0.15 * COS(SINE / 12), -0.2) * ANGLES(RAD(130), RAD(0), RAD(5)) * LEFTSHOULDERC0, 0.15 / Animation_Speed)
			RightHip.C0 = Clerp(RightHip.C0, CF(1 , -1 - 0.15 * COS(SINE / WALKSPEEDVALUE*2), -0.2+ 0.2 * COS(SINE / WALKSPEEDVALUE)) * ANGLES(RAD(0), RAD(65), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(-15)), 2 / Animation_Speed)
			LeftHip.C0 = Clerp(LeftHip.C0, CF(-1, -1 - 0.15 * COS(SINE / WALKSPEEDVALUE*2), -0.5+ -0.2 * COS(SINE / WALKSPEEDVALUE)) * ANGLES(RAD(0), RAD(-115), RAD(0)) * ANGLES(RAD(0), RAD(0), RAD(15)), 2 / Animation_Speed)
		end
	end
	if HITFLOOR ~= nil then
		Slice("Thin",0.4,35,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(0),RAD(0))*ANGLES(RAD(MRANDOM(-18,18)),RAD(MRANDOM(-180,180)),RAD(MRANDOM(-18,18))),"Deep orange",VT(0.001,0,0.001))
		Slice("Round",0.4,45,CF(HITPOS+VT(0,0.1,0),HITPOS+VT(0,0.1,0)+NORMAL)*ANGLES(RAD(90),RAD(MRANDOM(-180,180)),RAD(0)),"Deep orange",VT(0,0,0))
	end
	unanchor()
	Humanoid.MaxHealth = "inf"
	Humanoid.Health = "inf"
	if Rooted == false then
		Disable_Jump = false
		Humanoid.WalkSpeed = Speed
	elseif Rooted == true then
		Disable_Jump = true
		Humanoid.WalkSpeed = 0
	end
	if SKILL1COOLDOWN > 0 then
		SKILL1COOLDOWN = SKILL1COOLDOWN - 1
	end
	if SKILL2COOLDOWN > 0 then
		SKILL2COOLDOWN = SKILL2COOLDOWN - 1
	end
	if SKILL3COOLDOWN > 0 then
		SKILL3COOLDOWN = SKILL3COOLDOWN - 1
	end
end

--//=================================\\
--\\=================================//





--//====================================================\\--
--||			  		 END OF SCRIPT
--\\====================================================//--("Clicked")
                end)


                section1:addButton("FE BackFlipper (Key Z)", function()
                    wait(5)

--[[ Info ]]--

local ver = "2.00"
local scriptname = "feFlip"


--[[ Keybinds ]]--

local FrontflipKey = Enum.KeyCode.Z
local BackflipKey = Enum.KeyCode.X
local AirjumpKey = Enum.KeyCode.C


--[[ Dependencies ]]--

local ca = game:GetService("ContextActionService")
local zeezy = game:GetService("Players").LocalPlayer
local h = 0.0174533
local antigrav


--[[ Functions ]]--

function zeezyFrontflip(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character.Humanoid:ChangeState("Jumping")
		wait()
		zeezy.Character.Humanoid.Sit = true
		for i = 1,360 do 
			delay(i/720,function()
			zeezy.Character.Humanoid.Sit = true
				zeezy.Character.HumanoidRootPart.CFrame = zeezy.Character.HumanoidRootPart.CFrame * CFrame.Angles(-h,0,0)
			end)
		end
		wait(0.55)
		zeezy.Character.Humanoid.Sit = false
	end
end

function zeezyBackflip(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character.Humanoid:ChangeState("Jumping")
		wait()
		zeezy.Character.Humanoid.Sit = true
		for i = 1,360 do
			delay(i/720,function()
			zeezy.Character.Humanoid.Sit = true
				zeezy.Character.HumanoidRootPart.CFrame = zeezy.Character.HumanoidRootPart.CFrame * CFrame.Angles(h,0,0)
			end)
		end
		wait(0.55)
		zeezy.Character.Humanoid.Sit = false
	end
end

function zeezyAirjump(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character:FindFirstChildOfClass'Humanoid':ChangeState("Seated")
		wait()
		zeezy.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")	
	end
end


--[[ Binds ]]--

ca:BindAction("zeezyFrontflip",zeezyFrontflip,false,FrontflipKey)
ca:BindAction("zeezyBackflip",zeezyBackflip,false,BackflipKey)
ca:BindAction("zeezyAirjump",zeezyAirjump,false,AirjumpKey)

--[[ Load Message ]]--

print(scriptname .. " " .. ver .. " loaded successfully")
print("made by Zeezy#7203")

local notifSound = Instance.new("Sound",workspace)
notifSound.PlaybackSpeed = 1.5
notifSound.Volume = 0.15
notifSound.SoundId = "rbxassetid://170765130"
notifSound.PlayOnRemove = true
notifSound:Destroy()
game.StarterGui:SetCore("SendNotification", {Title = "feFlip", Text = "feFlip loaded successfully!", Icon = "rbxassetid://505845268", Duration = 5, Button1 = "Okay"})("Clicked")
                    end)


                    section1:addButton("FE Memeus", function()
                        -----------------------
--MemeusV2--
-------------------------------------------------------
--A script By makhail07

--This edit by 2003boobear

--Discord Creterisk#2958 (not 2003boobear's discord)
-------------------------------------------------------

local FavIDs = {
	340106355, --Nefl Crystals
	927529620, --Dimension
	876981900, --Fantasy
	398987889, --Ordinary Days
	1117396305, --Oh wait, it's you.
	885996042, --Action Winter Journey
	919231299, --Sprawling Idiot Effigy
	743466274, --Good Day Sunshine
	727411183, --Knife Fight
	1402748531, --The Earth Is Counting On You!
	595230126 --Robot Language
	}

FELOADLIBRARY = {}
loadstring(game:GetObjects("rbxassetid://5209815302")[1].Source)()
Bypass = "death"
loadstring(game:GetObjects("rbxassetid://5325226148")[1].Source)()

--The reality of my life isn't real but a Universe -makhail07
wait()
local Player = game.Players.localPlayer
local Character = workspace.non
local plr = game:service'Players'.LocalPlayer
local Humanoid = Character.Humanoid
local char = Character
local hum = char.Humanoid
local ra = char["Right Arm"]
local la= char["Left Arm"]
local rl= char["Right Leg"]
local ll = char["Left Leg"]
local hed = char.Head
local root = char.HumanoidRootPart
local rootj = root.RootJoint
local tors = char.Torso
local mouse = plr:GetMouse()
local RootCF = CFrame.fromEulerAnglesXYZ(-1.57, 0, 3.14)
local RHCF = CFrame.fromEulerAnglesXYZ(0, 1.6, 0)
local LHCF = CFrame.fromEulerAnglesXYZ(0, -1.6, 0)
local cam = game.Workspace.CurrentCamera
trazx = Instance.new("ParticleEmitter")
c = game.Players.LocalPlayer.Character

--where i put all the warn things

warn ("Well Look at that, I finished it.")
--Looks Like you decided to look though the script. Well, Hello.
warn ("I had a fun time making this edit.")
--I Really DID have fun editing this.
warn ("I hope you Enjoy this. Go have Fun!")
--Just don't abuse.
warn ("Also, the original MemeusV2 was made by makhail07.")
--Support makhail07 for making the original!
warn ("This edit was made by me, 2003boobear.")
--This is one of my best edits BY FAR, though.
Character.Head.face.Texture = "rbxassetid://620619801"

-------------------------------------------------------
--Start Good Stuff--
-------------------------------------------------------
CF = CFrame.new
angles = CFrame.Angles
attack = false
timetofly = true
Euler = CFrame.fromEulerAnglesXYZ
Rad = math.rad
IT = Instance.new
BrickC = BrickColor.new
Cos = math.cos
Acos = math.acos
Sin = math.sin
Asin = math.asin
Abs = math.abs
Mrandom = math.random
Floor = math.floor
random = math.random
radian = math.rad
Vec3 = Vector3.new
cFrame = CFrame.new
Euler = CFrame.fromEulerAnglesXYZ
-------------------------------------------------------
--End Good Stuff--
-------------------------------------------------------
necko = CF(0, 1, 0, -1, -0, -0, 0, 0, 1, 0, 1, 0)
RSH, LSH = nil, nil 
RW = Instance.new("Weld") 
LW = Instance.new("Weld")
RH = tors["Right Hip"]
LH = tors["Left Hip"]
RSH = tors["Right Shoulder"] 
LSH = tors["Left Shoulder"] 
RSH.Parent = nil 
LSH.Parent = nil 
RW.Name = "RW"
RW.Part0 = tors 
RW.C0 = CF(1.5, 0.5, 0)
RW.C1 = CF(0, 0.5, 0) 
RW.Part1 = ra
RW.Parent = tors 
LW.Name = "LW"
LW.Part0 = tors 
LW.C0 = CF(-1.5, 0.5, 0)
LW.C1 = CF(0, 0.5, 0) 
LW.Part1 = la
LW.Parent = tors
Effects = {}

-------------------------------------------------------
--Start HeartBeat--
-------------------------------------------------------
ArtificialHB = Instance.new("BindableEvent", script)
ArtificialHB.Name = "Heartbeat"
script:WaitForChild("Heartbeat")

frame = 1 / 60
tf = 0
allowframeloss = false
tossremainder = false


lastframe = tick()
script.Heartbeat:Fire()


game:GetService("RunService").Heartbeat:connect(function(s, p)
	tf = tf + s
	if tf >= frame then
		if allowframeloss then
			script.Heartbeat:Fire()
			lastframe = tick()
		else
			for i = 1, math.floor(tf / frame) do
				script.Heartbeat:Fire()
			end
			lastframe = tick()
		end
		if tossremainder then
			tf = 0
		else
			tf = tf - frame * math.floor(tf / frame)
		end
	end
end)
-------------------------------------------------------
--End HeartBeat--
-------------------------------------------------------

function CameraEnshaking(Length, Intensity) --Took Straight from StarGlitcher!
	coroutine.resume(coroutine.create(function()
		local intensity = 1 * Intensity
		local rotM = 0.01 * Intensity
		for i = 0, Length, 0.1 do
			swait()
			intensity = intensity - 0.05 * Intensity / Length
			rotM = rotM - 5.0E-4 * Intensity / Length
			hum.CameraOffset = Vec3(radian(random(-intensity, intensity)), radian(random(-intensity, intensity)), radian(random(-intensity, intensity)))
			cam.CFrame = cam.CFrame * cFrame(radian(random(-intensity, intensity)), radian(random(-intensity, intensity)), radian(random(-intensity, intensity))) * Euler(radian(random(-intensity, intensity)) * rotM, radian(random(-intensity, intensity)) * rotM, radian(random(-intensity, intensity)) * rotM)
		end
		Humanoid.CameraOffset = Vec3(0, 0, 0)
	end))
end

        local joyemoji = Instance.new('ParticleEmitter', tors)
        joyemoji.VelocitySpread = 2000
        joyemoji.Lifetime = NumberRange.new(1)
        joyemoji.Speed = NumberRange.new(40)
joy= {}
for i=0, 19 do
  joy[#joy+ 1] = NumberSequenceKeypoint.new(i/19, math.random(1, 1))
end
joyemoji.Size = NumberSequence.new(joy)
        joyemoji.Rate = 0
        joyemoji.LockedToPart = false
        joyemoji.LightEmission = 0
        joyemoji.Texture = "rbxassetid://1176402123"
        joyemoji.Color = ColorSequence.new(BrickColor.new("Institutional white").Color)


        local LIT = Instance.new('ParticleEmitter', tors)
        LIT.VelocitySpread = 2000
        LIT.Lifetime = NumberRange.new(1)
        LIT.Speed = NumberRange.new(45)
nani= {}
for i=0, 19 do
  nani[#nani+ 1] = NumberSequenceKeypoint.new(i/19, math.random(1, 1))
end
LIT.Size = NumberSequence.new(nani)
        LIT.Rate = 0
        LIT.LockedToPart = false
        LIT.LightEmission = 0
        LIT.Texture = "rbxassetid://1492670151"
        LIT.Color = ColorSequence.new(BrickColor.new("Institutional white").Color)

        local toast = Instance.new('ParticleEmitter', tors)
        toast.VelocitySpread = 2000
        toast.Lifetime = NumberRange.new(1)
        toast.Speed = NumberRange.new(60)
toasterstoasttoast= {}
for i=0, 19 do
  toasterstoasttoast[#toasterstoasttoast+ 1] = NumberSequenceKeypoint.new(i/19, math.random(1, 1))
end
toast.Size = NumberSequence.new(toasterstoasttoast)
        toast.Rate = 0
        toast.LockedToPart = false
        toast.LightEmission = 0
        toast.Texture = "rbxassetid://436096230"
        toast.Color = ColorSequence.new(BrickColor.new("Institutional white").Color)

        local ok = Instance.new('ParticleEmitter', tors)
        ok.VelocitySpread = 2000
        ok.Lifetime = NumberRange.new(1)
        ok.Speed = NumberRange.new(50)
cool= {}
for i=0, 19 do
  cool[#cool+ 1] = NumberSequenceKeypoint.new(i/19, math.random(1, 1))
end
ok.Size = NumberSequence.new(cool)
        ok.Rate = 0
        ok.LockedToPart = false
        ok.LightEmission = 0
        ok.Texture = "rbxassetid://636768448"
        ok.Color = ColorSequence.new(BrickColor.new("Institutional white").Color)

-------------------------------------------------------
--Start Kyu's shitty stuff--
-------------------------------------------------------

function ragdoll(model)
    local char = model
    torso = char.HumanoidRootPart
    torso2 = char.Torso
    LW.Parent = nil
    RW.Parent = nil
    LH.Parent = nil
    RH.Parent = nil
		if hum ~= nil then
		hum.PlatformStand = true
		end

		local Head = char:FindFirstChild("Head")
		if Head then
			local Neck = Instance.new("Weld")
			Neck.Name = "Neck"
			Neck.Part0 = torso
			Neck.Part1 = Head
			Neck.C0 = CFrame.new(0, 1.5, 0)
			Neck.C1 = CFrame.new()
			Neck.Parent = torso
		end
		local Limb = char:FindFirstChild("Right Arm")
		if Limb then

			Limb.CFrame = torso.CFrame * CFrame.new(1.5, 0, 0)
			local Joint = Instance.new("Glue")
			Joint.Name = "RightShoulder"
			Joint.Part0 = torso
			Joint.Part1 = Limb
			Joint.C0 = CFrame.new(1.5, 0.5, 0, 0, 0, 1, 0, 1, 0, -1, -0, -0)
			Joint.C1 = CFrame.new(-0, 0.5, 0, 0, 0, 1, 0, 1, 0, -1, -0, -0)
			Joint.Parent = torso

			local B = Instance.new("Part")
			B.TopSurface = 0
			B.BottomSurface = 0
			B.formFactor = "Symmetric"
			B.Size = Vector3.new(1, 1, 1)
			B.Transparency = 1
			B.CFrame = Limb.CFrame * CFrame.new(0, -0.5, 0)
			B.Parent = char
			local W = Instance.new("Weld")
			W.Part0 = Limb
			W.Part1 = B
			W.C0 = CFrame.new(0, -0.5, 0)
			W.Parent = Limb

		end
		local Limb = char:FindFirstChild("Left Arm")
		if Limb then

			Limb.CFrame = torso.CFrame * CFrame.new(-1.5, 0, 0)
			local Joint = Instance.new("Glue")
			Joint.Name = "LeftShoulder"
			Joint.Part0 = torso
			Joint.Part1 = Limb
			Joint.C0 = CFrame.new(-1.5, 0.5, 0, -0, -0, -1, 0, 1, 0, 1, 0, 0)
			Joint.C1 = CFrame.new(0, 0.5, 0, -0, -0, -1, 0, 1, 0, 1, 0, 0)
			Joint.Parent = torso

			local B = Instance.new("Part")
			B.TopSurface = 0
			B.BottomSurface = 0
			B.formFactor = "Symmetric"
			B.Size = Vector3.new(1, 1, 1)
			B.Transparency = 1
			B.CFrame = Limb.CFrame * CFrame.new(0, -0.5, 0)
			B.Parent = char
			local W = Instance.new("Weld")
			W.Part0 = Limb
			W.Part1 = B
			W.C0 = CFrame.new(0, -0.5, 0)
			W.Parent = Limb

		end
		local Limb = char:FindFirstChild("Right Leg")
		if Limb then

			Limb.CFrame = torso.CFrame * CFrame.new(0.5, -2, 0)
			local Joint = Instance.new("Glue")
			Joint.Name = "RightHip"
			Joint.Part0 = torso
			Joint.Part1 = Limb
			Joint.C0 = CFrame.new(0.5, -1, 0, 0, 0, 1, 0, 1, 0, -1, -0, -0)
			Joint.C1 = CFrame.new(0, 1, 0, 0, 0, 1, 0, 1, 0, -1, -0, -0)
			Joint.Parent = torso

			local B = Instance.new("Part")
			B.TopSurface = 0
			B.BottomSurface = 0
			B.formFactor = "Symmetric"
			B.Size = Vector3.new(1, 1, 1)
			B.Transparency = 1
			B.CFrame = Limb.CFrame * CFrame.new(0, -0.5, 0)
			B.Parent = char
			local W = Instance.new("Weld")
			W.Part0 = Limb
			W.Part1 = B
			W.C0 = CFrame.new(0, -0.5, 0)
			W.Parent = Limb

		end
		local Limb = char:FindFirstChild("Left Leg")
		if Limb then

			Limb.CFrame = torso.CFrame * CFrame.new(-0.5, -2, 0)
			local Joint = Instance.new("Glue")
			Joint.Name = "LeftHip"
			Joint.Part0 = torso
			Joint.Part1 = Limb
			Joint.C0 = CFrame.new(-0.5, -1, 0, -0, -0, -1, 0, 1, 0, 1, 0, 0)
			Joint.C1 = CFrame.new(-0, 1, 0, -0, -0, -1, 0, 1, 0, 1, 0, 0)
			Joint.Parent = torso

			local B = Instance.new("Part")
			B.TopSurface = 0
			B.BottomSurface = 0
			B.formFactor = "Symmetric"
			B.Size = Vector3.new(1, 1, 1)
			B.Transparency = 1
			B.CFrame = Limb.CFrame * CFrame.new(0, -0.5, 0)
			B.Parent = char
			local W = Instance.new("Weld")
			W.Part0 = Limb
			W.Part1 = B
			W.C0 = CFrame.new(0, -0.5, 0)
			W.Parent = Limb

		end
		--[
		local Bar = Instance.new("Part")
		Bar.TopSurface = 0
		Bar.BottomSurface = 0
		Bar.formFactor = "Symmetric"
		Bar.Size = Vector3.new(1, 1, 1)
		Bar.Transparency = 1
		Bar.CFrame = torso.CFrame * CFrame.new(0, 0.5, 0)
		Bar.Parent = char
		local Weld = Instance.new("Weld")
		Weld.Part0 = torso
		Weld.Part1 = Bar
		Weld.C0 = CFrame.new(0, 0.5, 0)
		Weld.Parent = torso
		--]]

torso.CFrame = CFrame.new(torso.Position)*CFrame.Angles(math.rad(20),math.rad(torso.Orientation.Y),math.rad(torso.Orientation.Z))

end

-------------------------------------------------------
--End Kyu's shitty stuff--
-------------------------------------------------------

-------------------------------------------------------
--Start Important Functions--
-------------------------------------------------------
function swait(num)
	if num == 0 or num == nil then
		game:service("RunService").Stepped:wait(0)
	else
		for i = 0, num do
			game:service("RunService").Stepped:wait(0)
		end
	end
end
function thread(f)
	coroutine.resume(coroutine.create(f))
end
function clerp(a, b, t)
	local qa = {
		QuaternionFromCFrame(a)
	}
	local qb = {
		QuaternionFromCFrame(b)
	}
	local ax, ay, az = a.x, a.y, a.z
	local bx, by, bz = b.x, b.y, b.z
	local _t = 1 - t
	return QuaternionToCFrame(_t * ax + t * bx, _t * ay + t * by, _t * az + t * bz, QuaternionSlerp(qa, qb, t))
end
function QuaternionFromCFrame(cf)
	local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components()
	local trace = m00 + m11 + m22
	if trace > 0 then
		local s = math.sqrt(1 + trace)
		local recip = 0.5 / s
		return (m21 - m12) * recip, (m02 - m20) * recip, (m10 - m01) * recip, s * 0.5
	else
		local i = 0
		if m00 < m11 then
			i = 1
		end
		if m22 > (i == 0 and m00 or m11) then
			i = 2
		end
		if i == 0 then
			local s = math.sqrt(m00 - m11 - m22 + 1)
			local recip = 0.5 / s
			return 0.5 * s, (m10 + m01) * recip, (m20 + m02) * recip, (m21 - m12) * recip
		elseif i == 1 then
			local s = math.sqrt(m11 - m22 - m00 + 1)
			local recip = 0.5 / s
			return (m01 + m10) * recip, 0.5 * s, (m21 + m12) * recip, (m02 - m20) * recip
		elseif i == 2 then
			local s = math.sqrt(m22 - m00 - m11 + 1)
			local recip = 0.5 / s
			return (m02 + m20) * recip, (m12 + m21) * recip, 0.5 * s, (m10 - m01) * recip
		end
	end
end
function QuaternionToCFrame(px, py, pz, x, y, z, w)
	local xs, ys, zs = x + x, y + y, z + z
	local wx, wy, wz = w * xs, w * ys, w * zs
	local xx = x * xs
	local xy = x * ys
	local xz = x * zs
	local yy = y * ys
	local yz = y * zs
	local zz = z * zs
	return CFrame.new(px, py, pz, 1 - (yy + zz), xy - wz, xz + wy, xy + wz, 1 - (xx + zz), yz - wx, xz - wy, yz + wx, 1 - (xx + yy))
end
function QuaternionSlerp(a, b, t)
	local cosTheta = a[1] * b[1] + a[2] * b[2] + a[3] * b[3] + a[4] * b[4]
	local startInterp, finishInterp
	if cosTheta >= 1.0E-4 then
		if 1 - cosTheta > 1.0E-4 then
			local theta = math.acos(cosTheta)
			local invSinTheta = 1 / Sin(theta)
			startInterp = Sin((1 - t) * theta) * invSinTheta
			finishInterp = Sin(t * theta) * invSinTheta
		else
			startInterp = 1 - t
			finishInterp = t
		end
	elseif 1 + cosTheta > 1.0E-4 then
		local theta = math.acos(-cosTheta)
		local invSinTheta = 1 / Sin(theta)
		startInterp = Sin((t - 1) * theta) * invSinTheta
		finishInterp = Sin(t * theta) * invSinTheta
	else
		startInterp = t - 1
		finishInterp = t
	end
	return a[1] * startInterp + b[1] * finishInterp, a[2] * startInterp + b[2] * finishInterp, a[3] * startInterp + b[3] * finishInterp, a[4] * startInterp + b[4] * finishInterp
end
function rayCast(Position, Direction, Range, Ignore)
	return game:service("Workspace"):FindPartOnRay(Ray.new(Position, Direction.unit * (Range or 999.999)), Ignore)
end

local Create = FELOADLIBRARY.Create

-------------------------------------------------------
--Start Damage Function--
-------------------------------------------------------
function Damage(Part, hit, minim, maxim, knockback, Type, Property, Delay, HitSound, HitPitch)
	if hit.Parent == nil then
		return
	end
	local h = hit.Parent:FindFirstChildOfClass("Humanoid")
	for _, v in pairs(hit.Parent:children()) do
		if v:IsA("Humanoid") then
			h = v
		end
	end

	if h ~= nil and hit.Parent.Name ~= char.Name and hit.Parent:FindFirstChild("Torso") ~= nil then
		if hit.Parent:findFirstChild("DebounceHit") ~= nil then
			if hit.Parent.DebounceHit.Value == true then
				return
			end
		end
		local c = Create("ObjectValue"){
			Name = "creator",
			Value = game:service("Players").LocalPlayer,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
		if HitSound ~= nil and HitPitch ~= nil then
			CFuncs.Sound.Create(HitSound, hit, 1, HitPitch) 
		end
		local Damage = math.random(minim, maxim)
		local blocked = false
		local block = hit.Parent:findFirstChild("Block")
		if block ~= nil then
			if block.className == "IntValue" then
				if block.Value > 0 then
					blocked = true
					block.Value = block.Value - 1
				end
			end
		end
		if blocked == false then
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		else
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		end
		if Type == "Knockdown" then
			local hum = hit.Parent.Humanoid
			hum.PlatformStand = true
			coroutine.resume(coroutine.create(function(HHumanoid)
				swait(1)
				HHumanoid.PlatformStand = false
			end), hum)
			local angle = (hit.Position - (Property.Position + Vector3.new(0, 0, 0))).unit
			local bodvol = Create("BodyVelocity"){
				velocity = angle * knockback,
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			local rl = Create("BodyAngularVelocity"){
				P = 3000,
				maxTorque = Vector3.new(500000, 500000, 500000) * 50000000000000,
				angularvelocity = Vector3.new(math.random(-10, 10), math.random(-10, 10), math.random(-10, 10)),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodvol, .5)
			game:GetService("Debris"):AddItem(rl, .5)
		elseif Type == "Normal" then
			local vp = Create("BodyVelocity"){
				P = 500,
				maxForce = Vector3.new(math.huge, 0, math.huge),
				velocity = Property.CFrame.lookVector * knockback + Property.Velocity / 1.05,
			}
			if knockback > 0 then
				vp.Parent = hit.Parent.Torso
			end
			game:GetService("Debris"):AddItem(vp, .5)
		elseif Type == "Up" then
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, .5)
		elseif Type == "DarkUp" then
			coroutine.resume(coroutine.create(function()
				for i = 0, 1, 0.1 do
					swait()
					Effects.Block.Create(BrickColor.new("Black"), hit.Parent.Torso.CFrame, 5, 5, 5, 1, 1, 1, .08, 1)
				end
			end))
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, 1)
		elseif Type == "Snare" then
			local bp = Create("BodyPosition"){
				P = 2000,
				D = 100,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			game:GetService("Debris"):AddItem(bp, 1)
		elseif Type == "Freeze" then
			local BodPos = Create("BodyPosition"){
				P = 50000,
				D = 1000,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			local BodGy = Create("BodyGyro") {
				maxTorque = Vector3.new(4e+005, 4e+005, 4e+005) * math.huge ,
				P = 20e+003,
				Parent = hit.Parent.Torso,
				cframe = hit.Parent.Torso.CFrame,
			}
			hit.Parent.Torso.Anchored = true
			coroutine.resume(coroutine.create(function(Part) 
				swait(1.5)
				Part.Anchored = false
			end), hit.Parent.Torso)
			game:GetService("Debris"):AddItem(BodPos, 3)
			game:GetService("Debris"):AddItem(BodGy, 3)
		end
		local debounce = Create("BoolValue"){
			Name = "DebounceHit",
			Parent = hit.Parent,
			Value = true,
		}
		game:GetService("Debris"):AddItem(debounce, Delay)
		c = Create("ObjectValue"){
			Name = "creator",
			Value = Player,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
	end
end
-------------------------------------------------------
--End Damage Function--
-------------------------------------------------------

-------------------------------------------------------
--Start Damage Function Customization--
-------------------------------------------------------
function ShowDamage(Pos, Text, Time, Color)
	local Rate = (1 / 30)
	local Pos = (Pos or Vector3.new(0, 0, 0))
	local Text = (Text or "")
	local Time = (Time or 2)
	local Color = (Color or Color3.new(1, 0, 1))
	local EffectPart = CFuncs.Part.Create(workspace, "SmoothPlastic", 0, 1, BrickColor.new(Color), "Effect", Vector3.new(0, 0, 0))
	EffectPart.Anchored = true
	local BillboardGui = Create("BillboardGui"){
		Size = UDim2.new(3, 0, 3, 0),
		Adornee = EffectPart,
		Parent = EffectPart,
	}
	local TextLabel = Create("TextLabel"){
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 1, 0),
		Text = Text,
		Font = "Highway",
		TextColor3 = Color,
		TextScaled = true,
		Parent = BillboardGui,
	}
	game.Debris:AddItem(EffectPart, (Time))
	EffectPart.Parent = game:GetService("Workspace")
	delay(0, function()
		local Frames = (Time / Rate)
		for Frame = 1, Frames do
			wait(Rate)
			local Percent = (Frame / Frames)
			EffectPart.CFrame = CFrame.new(Pos) + Vector3.new(0, Percent, 0)
			TextLabel.TextTransparency = Percent
		end
		if EffectPart and EffectPart.Parent then
			EffectPart:Destroy()
		end
	end)
end
-------------------------------------------------------
--End Damage Function Customization--
-------------------------------------------------------

function MagniDamage(Part, magni, mindam, maxdam, knock, Type)
  for _, c in pairs(workspace:children()) do
    local hum = c:findFirstChild("Humanoid")
    if hum ~= nil then
      local head = c:findFirstChild("Head")
      if head ~= nil then
        local targ = head.Position - Part.Position
        local mag = targ.magnitude
        if magni >= mag and c.Name ~= plr.Name then
          Damage(head, head, mindam, maxdam, knock, Type, root, 0.1, "http://www.roblox.com/asset/?id=231917784", 1.2)
        end
      end
    end
  end
end


CFuncs = {
	Part = {
		Create = function(Parent, Material, Reflectance, Transparency, BColor, Name, Size)
			local Part = Create("Part")({
				Parent = Parent,
				Reflectance = Reflectance,
				Transparency = Transparency,
				CanCollide = false,
				Locked = true,
				BrickColor = BrickColor.new(tostring(BColor)),
				Name = Name,
				Size = Size,
				Material = Material
			})
			RemoveOutlines(Part)
			return Part
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Weld = {
		Create = function(Parent, Part0, Part1, C0, C1)
			local Weld = Create("Weld")({
				Parent = Parent,
				Part0 = Part0,
				Part1 = Part1,
				C0 = C0,
				C1 = C1
			})
			return Weld
		end
	},
	Sound = {
		Create = function(id, par, vol, pit)
			coroutine.resume(coroutine.create(function()
				local S = Create("Sound")({
					Volume = vol,
					Pitch = pit or 1,
					SoundId = id,
					Parent = par or workspace
				})
				wait()
				S:play()
				game:GetService("Debris"):AddItem(S, 6)
			end))
		end
	},
	ParticleEmitter = {
		Create = function(Parent, Color1, Color2, LightEmission, Size, Texture, Transparency, ZOffset, Accel, Drag, LockedToPart, VelocityInheritance, EmissionDirection, Enabled, LifeTime, Rate, Rotation, RotSpeed, Speed, VelocitySpread)
			local fp = Create("ParticleEmitter")({
				Parent = Parent,
				Color = ColorSequence.new(Color1, Color2),
				LightEmission = LightEmission,
				Size = Size,
				Texture = Texture,
				Transparency = Transparency,
				ZOffset = ZOffset,
				Acceleration = Accel,
				Drag = Drag,
				LockedToPart = LockedToPart,
				VelocityInheritance = VelocityInheritance,
				EmissionDirection = EmissionDirection,
				Enabled = Enabled,
				Lifetime = LifeTime,
				Rate = Rate,
				Rotation = Rotation,
				RotSpeed = RotSpeed,
				Speed = Speed,
				VelocitySpread = VelocitySpread
			})
			return fp
		end
	}
}
function RemoveOutlines(part)
	part.TopSurface, part.BottomSurface, part.LeftSurface, part.RightSurface, part.FrontSurface, part.BackSurface = 10, 10, 10, 10, 10, 10
end
function CreatePart(FormFactor, Parent, Material, Reflectance, Transparency, BColor, Name, Size)
	local Part = Create("Part")({
		formFactor = FormFactor,
		Parent = Parent,
		Reflectance = Reflectance,
		Transparency = Transparency,
		CanCollide = false,
		Locked = true,
		BrickColor = BrickColor.new(tostring(BColor)),
		Name = Name,
		Size = Size,
		Material = Material
	})
	RemoveOutlines(Part)
	return Part
end
function CreateMesh(Mesh, Part, MeshType, MeshId, OffSet, Scale)
	local Msh = Create(Mesh)({
		Parent = Part,
		Offset = OffSet,
		Scale = Scale
	})
	if Mesh == "SpecialMesh" then
		Msh.MeshType = MeshType
		Msh.MeshId = MeshId
	end
	return Msh
end
function CreateWeld(Parent, Part0, Part1, C0, C1)
	local Weld = Create("Weld")({
		Parent = Parent,
		Part0 = Part0,
		Part1 = Part1,
		C0 = C0,
		C1 = C1
	})
	return Weld
end


-------------------------------------------------------
--Start Effect Function--
-------------------------------------------------------
EffectModel = Instance.new("Model", char)
Effects = {
  Block = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay, Type)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("BlockMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      if Type == 1 or Type == nil then
        table.insert(Effects, {
          prt,
          "Block1",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      elseif Type == 2 then
        table.insert(Effects, {
          prt,
          "Block2",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      else
        table.insert(Effects, {
          prt,
          "Block3",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      end
    end
  },
  Sphere = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0,0,0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Cylinder = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("CylinderMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Wave = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://20329976", Vector3.new(0, 0, 0), Vector3.new(x1 / 60, y1 / 60, z1 / 60))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3 / 60,
        y3 / 60,
        z3 / 60,
        msh
      })
    end
  },
  Ring = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://3270017", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Break = {
    Create = function(brickcolor, cframe, x1, y1, z1)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new(0.5, 0.5, 0.5))
      prt.Anchored = true
      prt.CFrame = cframe * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      local num = math.random(10, 50) / 1000
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Shatter",
        num,
        prt.CFrame,
        math.random() - math.random(),
        0,
        math.random(50, 100) / 100
      })
    end
  },
Spiral = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://1051557", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
Push = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://437347603", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  }
}
-------------------------------------------------------
--End Effect Function--
-------------------------------------------------------

function CreateSound(ID, PARENT, VOLUME, PITCH) --Very important.
	local NSound = nil
	coroutine.resume(coroutine.create(function()
		NSound = Instance.new("Sound", PARENT)
		NSound.Volume = VOLUME
		NSound.Pitch = PITCH
		NSound.SoundId = "http://www.roblox.com/asset/?id="..ID
		swait()
		NSound:play()
		game:GetService("Debris"):AddItem(NSound, 10)
	end))
	return NSound
end





-------------------------------------------------------
--End Important Functions--
-------------------------------------------------------

chargeup = Instance.new("Sound", hed)
chargeup.SoundId = "http://www.roblox.com/asset/?id=527276541"
chargeup.Volume = 10
chargeup.Pitch = 1
chargeup.Looped = true
chargeup.TimePosition = 1

meme = Instance.new("Sound", hed)
meme.SoundId = "http://www.roblox.com/asset/?id=291151190"
meme.Volume = 10
meme.Pitch = 1
meme.Looped = true
meme.TimePosition = 1

local ohno = Instance.new("Sound")
ohno.Parent = hed
ohno.Volume = 10
ohno.Pitch = 1
ohno.Looped = true

local bass = Instance.new("Sound") --why
bass.Parent = hed
bass.Volume = 7
bass.Pitch = 1
bass.SoundId = "http://www.roblox.com/asset/?id=1087356234"
bass.Looped = true

Cause_Im_having_a_good_time_having_a_good_time = Instance.new("Sound", hed) --DONT STOP ME NOOOOOOOOOWWWWWWWW
Cause_Im_having_a_good_time_having_a_good_time.SoundId = "http://www.roblox.com/asset/?id=672104253"
Cause_Im_having_a_good_time_having_a_good_time.Volume = 10
Cause_Im_having_a_good_time_having_a_good_time.Pitch = 1
Cause_Im_having_a_good_time_having_a_good_time.Looped = false
Cause_Im_having_a_good_time_having_a_good_time.TimePosition = 35.3

STHAP = Instance.new("Sound", hed)
STHAP.SoundId = "http://www.roblox.com/asset/?id=1591656314"
STHAP.Volume = 10
STHAP.Pitch = 1
STHAP.Looped = false

forevergone = Instance.new("Sound", tors)
forevergone.SoundId = "http://www.roblox.com/asset/?id=1286436928"
forevergone.Volume = 10
forevergone.Pitch = 1
forevergone.Looped = true
forevergone.TimePosition = 24

-------------------------------------------------------
--Start Music Option--
-------------------------------------------------------
local Music = Instance.new("Sound",tors)
Music.Volume = 2.5
Music.SoundId = "rbxassetid://"
Music.Looped = true
Music.Pitch = 1 --Pitcher
Music:Play()
-------------------------------------------------------
--End Music Option--
-------------------------------------------------------
--hi fat >:)
-------------------------------------------------------
--Start Attacks N Stuff--
-------------------------------------------------------
local sine=0
function HitboxFunction(Pose, lifetime, siz1, siz2, siz3, Radie, Min, Max, kb, atype)
  local Hitboxpart = Instance.new("Part", EffectModel)
  RemoveOutlines(Hitboxpart)
  Hitboxpart.Size = Vector3.new(siz1, siz2, siz3)
  Hitboxpart.CanCollide = false
  Hitboxpart.Transparency = 1
  Hitboxpart.Anchored = true
  Hitboxpart.CFrame = Pose
  game:GetService("Debris"):AddItem(Hitboxpart, lifetime)
  MagniDamage(Hitboxpart, Radie, Min, Max, kb, atype)
end
function GEtOuT()
	attack = true
	hum.WalkSpeed = 10
        Character.Head.face.Texture = "rbxassetid://494811799"
        CreateSound("814652778", hed, 10, 1)
        CreateSound("537371462", hed, 10, 1)
        local vel3 = Instance.new("BodyVelocity",tors)
        vel3.Velocity = Vector3.new(0,25,0)
        vel3.MaxForce = Vector3.new(10000000,10000000,10000000)
	for i = 0,12,0.1 do
		swait()
		CameraEnshaking(1, 2)
	        HitboxFunction(ll.CFrame, 0.01, 1, 1, 1, 7, 20, 99, 53, "Knockdown")
                rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0-255.45*i)), 0.3)
                tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-40), Rad(0), Rad(0)), 0.3)
                RW.C0 = clerp(RW.C0, CF(1.5, 0.5, 0) * angles(Rad(30), Rad(0), Rad(20)), 0.3)
                LW.C0 = clerp(LW.C0, CF(-1.5, 0.5, 0) * angles(Rad(-20), Rad(0), Rad(-30)), 0.3)
                LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), -0.3) * LHCF * angles(Rad(-5), Rad(0), Rad(20)), 0.15)
                RH.C0 = clerp(RH.C0, CF(1, -1, 0.3) * angles(Rad(0), Rad(90), Rad(-20)), 0.3)
	end
        vel3:Destroy()
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
        Humanoid.JumpPower = 50
	hum.WalkSpeed = 16
end

function GEtOuT2()
	attack = true
	hum.WalkSpeed = 10
        Humanoid.JumpPower = 0
        Character.Head.face.Texture = "rbxassetid://494811799"
        CreateSound("814652778", hed, 10, 1)
        CreateSound("537371462", hed, 10, 1)
        root.Velocity = root.CFrame.lookVector * 20
	for i = 0,12,0.1 do
		swait()
		CameraEnshaking(1, 2)
                root.Velocity = root.CFrame.lookVector * 50
	        HitboxFunction(ll.CFrame, 0.01, 1, 1, 1, 7, 10, 50, 53, "Knockdown")
                rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(0-255.45*i)), 0.3)
                tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-40), Rad(0), Rad(0)), 0.3)
                RW.C0 = clerp(RW.C0, CF(1.5, 0.5, 0) * angles(Rad(30), Rad(0), Rad(20)), 0.3)
                LW.C0 = clerp(LW.C0, CF(-1.5, 0.5, 0) * angles(Rad(-20), Rad(0), Rad(-30)), 0.3)
                LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), -0.3) * LHCF * angles(Rad(-5), Rad(0), Rad(20)), 0.15)
                RH.C0 = clerp(RH.C0, CF(1, -1, 0.3) * angles(Rad(0), Rad(90), Rad(-20)), 0.3)
	end
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
        Humanoid.JumpPower = 50
	hum.WalkSpeed = 16
end
function Flight() --wowthatsdiffrent
attack = true
Character.Head.face.Texture = "rbxassetid://269748407"
local ColorsArray ={ColorSequenceKeypoint.new(0, Color3.new(1,0,0)),
ColorSequenceKeypoint.new(0.16, Color3.new(1,1,1)),
ColorSequenceKeypoint.new(0.32, Color3.new(0,0,1)),
ColorSequenceKeypoint.new(0.48, Color3.new(1,1,1)),
ColorSequenceKeypoint.new(0.64, Color3.new(1,0,0)),
ColorSequenceKeypoint.new(0.80, Color3.new(1,1,1)),
ColorSequenceKeypoint.new(0.96, Color3.new(0,0,1)),
ColorSequenceKeypoint.new(1, Color3.new(1,1,1))}
local vel4 = Instance.new("BodyVelocity",ll)
vel4.Velocity = Vector3.new(0,4,0)
vel4.MaxForce = Vector3.new(10000000,10000000,10000000)
local Atch3 = Instance.new("Attachment",ll)Atch3.Position = Vector3.new(0,0.6,0)
local Atch4 = Instance.new("Attachment",ll)Atch4.Position = Vector3.new(0,-0.6,0)
local Trail2 = Instance.new("Trail",ll)Trail2.Attachment0 = Atch3 Trail2.Attachment1 = Atch4
Trail2.Texture = "rbxassetid://22636887" Trail2.Lifetime = 0.2 Trail2.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0,0),NumberSequenceKeypoint.new(1,0,0)})
Trail2.Color = ColorSequence.new(ColorsArray) Trail2.LightEmission = 1 
Trail2.Enabled = true
local Atch5 = Instance.new("Attachment",rl)Atch5.Position = Vector3.new(0,0.6,0)
local Atch6 = Instance.new("Attachment",rl)Atch6.Position = Vector3.new(0,-0.6,0)
local Trail3 = Instance.new("Trail",rl)Trail3.Attachment0 = Atch5 Trail3.Attachment1 = Atch6
Trail3.Texture = "rbxassetid://22636887" Trail3.Lifetime = 0.2 Trail3.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0,0),NumberSequenceKeypoint.new(1,0,0)})
Trail3.Color = ColorSequence.new(ColorsArray) Trail3.LightEmission = 1 
Trail3.Enabled = true
local Atch7 = Instance.new("Attachment",ra)Atch7.Position = Vector3.new(0,0.6,0)
local Atch8 = Instance.new("Attachment",ra)Atch8.Position = Vector3.new(0,-0.6,0)
local Trail4 = Instance.new("Trail",ra)Trail4.Attachment0 = Atch7 Trail4.Attachment1 = Atch8
Trail4.Texture = "rbxassetid://22636887" Trail4.Lifetime = 0.2 Trail4.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0,0),NumberSequenceKeypoint.new(1,0,0)})
Trail4.Color = ColorSequence.new(ColorsArray) Trail4.LightEmission = 1 
Trail4.Enabled = true
local Atch9 = Instance.new("Attachment",la)Atch9.Position = Vector3.new(0,0.6,0)
local Atch10 = Instance.new("Attachment",la)Atch10.Position = Vector3.new(0,-0.6,0)
local Trail5 = Instance.new("Trail",la)Trail5.Attachment0 = Atch9 Trail5.Attachment1 = Atch10
Trail5.Texture = "rbxassetid://22636887" Trail5.Lifetime = 0.2 Trail5.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0,0),NumberSequenceKeypoint.new(1,0,0)})
Trail5.Color = ColorSequence.new(ColorsArray) Trail5.LightEmission = 1 
Trail5.Enabled = true
local Atch1 = Instance.new("Attachment",Torso)Atch1.Position = Vector3.new(0,2,0)
local Atch2 = Instance.new("Attachment",Torso)Atch2.Position = Vector3.new(0,-2.5,0)
local Trail = Instance.new("Trail",Torso)Trail.Attachment0 = Atch1 Trail.Attachment1 = Atch2
Trail.Texture = "rbxassetid://22636887" Trail.Lifetime = 0.2 Trail.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0,0),NumberSequenceKeypoint.new(1,0,0)})
Trail.Color = ColorSequence.new(ColorsArray) Trail.LightEmission = 1 
Trail.Enabled = false
ragdoll(char)
wait(1)
Character.Head.face.Texture = "rbxassetid://249062487"
CreateSound("948494432", hed, 10, 1)
wait(2)
Character.Head.face.Texture = "rbxassetid://269748407"
CreateSound("633394595", hed, 10, 1)
wait(2)
Character.Head.face.Texture = "rbxassetid://494811799"
STHAP:play()
wait(11)
forevergone:play()
end

function OBJECTION()
	attack = true
	hum.WalkSpeed = 10
        Character.Head.face.Texture = "rbxassetid://55831869"
	CreateSound("330859085", hed, 10, 1)
	for i = 0,8,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(180), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function Hello()
	attack = true
	hum.WalkSpeed = 10
        Character.Head.face.Texture = "rbxassetid://334668738"
	CreateSound("855338765", hed, 10, 0.9)
	for i = 0,3,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(180), Rad(20), Rad(-5)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function Victory()
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://616284160"
        Humanoid.Jump = true
        CreateSound("130834939", hed, 10, 1)
        for i = 0,3.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(180), Rad(20), Rad(-5)), 0.1)
        end
        Humanoid.Jump = true
        for i = 0,3.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(40)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(40)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-180), Rad(-25), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-0)), 0.1)
        end
        Humanoid.Jump = true
        for i = 0,3.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(180), Rad(20), Rad(-5)), 0.1)
        end
        Humanoid.Jump = true
        for i = 0,3.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(40)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(40)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-180), Rad(-25), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-0)), 0.1)
        end
        Humanoid.Jump = true
        for i = 0,3.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-40)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(180), Rad(20), Rad(-5)), 0.1)
        end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function ShutTheHellUp()
	attack = true
	hum.WalkSpeed = 2.01
        Character.Head.face.Texture = "rbxassetid://963148419"
	CreateSound("336377340", hed, 10, 1)
	for i = 0,3,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,1.2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(20), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,1.2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-5), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,1.2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(20), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,1.2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.2) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-5), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(120), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	for i = 0,2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(90), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
	end
	hum.WalkSpeed = 16
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
end

function SpinMeDad() --YOU SPIN ME RIGHT ROUND BABY RIGHT ROUND
	attack = true
	hum.WalkSpeed = 5
        Humanoid.JumpPower = 175
        Character.Head.face.Texture = "rbxassetid://1223903433"
	CreateSound("145799973", hed, 10, 1)
        local vel2 = Instance.new("BodyVelocity",tors)
        vel2.Velocity = Vector3.new(0,1.2,0)
        vel2.MaxForce = Vector3.new(10000000,10000000,10000000)
	for i = 0,60,0.1 do
	        HitboxFunction(ll.CFrame, 0.01, 1, 1, 1, 7, 5, 20, 53, "Knockdown")
		swait()
		CameraEnshaking(1, 1)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0-255.45*i)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(90)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-90)), 0.1)
	end
	hum.WalkSpeed = 16
        vel2:Destroy()
        Character.Head.face.Texture = "rbxassetid://620619801"
        Humanoid.JumpPower = 50
	attack = false
end

function EndMySufferingV2() --why
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://202210455"
        local A = math.random(1,5)
        if A == 1 then
            meme.SoundId = "rbxassetid://295810519"
        end
        if A == 2 then
            meme.SoundId = "rbxassetid://1124778077"
        end
        if A == 3 then
            meme.SoundId = "rbxassetid://464157070"
        end
        if A == 4 then
            meme.SoundId = "rbxassetid://146334595"
        end
        if A == 5 then
            meme.SoundId = "rbxassetid://145536915"
        end
        meme:Play()
        bass:Play()
        joyemoji.Rate = 70
        LIT.Rate = 70
        ok.Rate = 70
        toast.Rate = 70
        
	for i = 0,50,0.1 do
		swait()
	CameraEnshaking(1, 10)
        bass.Parent = hed
        meme.Parent = hed
	rootj.C0=clerp(rootj.C0,RootCF*CF(0,0,-0.1+0.1*math.cos(sine/20))*angles(math.rad(15),math.rad(-10),math.rad(0)),0.15)
	tors.Neck.C0=clerp(tors.Neck.C0,necko*angles(math.rad(35),math.rad(0),math.rad(0)),.3)
	RH.C0=clerp(RH.C0,CF(1,-.9-0.1*math.cos(sine/20),.025*math.cos(sine/20))*RHCF*angles(math.rad(-5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,CF(-1,-.9-0.1*math.cos(sine/20),.025*math.cos(sine/20))*LHCF*angles(math.rad(-5),math.rad(-0),math.rad(-20)),0.15)
	RW.C0 = clerp(RW.C0, CFrame.new(1.1, 0.5+0.1*math.sin(sine/30), -0.6) * angles(math.rad(-0), math.rad(10), math.rad(-110)), 0.1)
	LW.C0 = clerp(LW.C0, CFrame.new(-1.5, 0.5+0.1*math.sin(sine/30), 0.055*math.cos(sine/20)) * angles(math.rad(-0), math.rad(-10), math.rad(-105)), 0.1)
	end
        bass:Stop()
        meme:Stop()
        joyemoji.Rate = 0
        LIT.Rate = 0
        ok.Rate = 0
        toast.Rate = 0
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
	hum.WalkSpeed = 16
end

function HELP()
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://307972876"
	CreateSound("1123321019", hed, 10, 1)
	for i = 0,15,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
  	CreateSound("198462271", hed, 10, 1)
	for i = 0,8,0.1 do
                Character.Head.face.Texture = "rbxassetid://341497730"
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
	for i = 0,8,0.1 do
                Character.Head.face.Texture = "rbxassetid://341497730"
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(60), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
  	CreateSound("948494432", hed, 10, 1)
	for i = 0,7.5,0.1 do
                Character.Head.face.Texture = "rbxassetid://249062487"
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(60), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
  	CreateSound("1542642349", hed, 10, 1)
	for i = 0,10,0.1 do
                Character.Head.face.Texture = "rbxassetid://270636807"
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
  	CreateSound("269597232", hed, 10, 1)
	for i = 0,6,0.1 do
                Character.Head.face.Texture = "rbxassetid://265057155"
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-0.7, -0.01 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-0.9, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function Choose()
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://374187112"
	CreateSound("130784263", hed, 10, 1)
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(110), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(110), Rad(0), Rad(0)), 0.1)
	end
	for i = 0,5,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(35), Rad(0), Rad(-10)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(35), Rad(0), Rad(10)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function slap()
	attack = true
	hum.WalkSpeed = 10
	CreateSound("146163534", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://293603561"
	CameraEnshaking(1, 2)
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(20), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(20), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(115 + 4), Rad(45), Rad(50)), 0.1)
	end
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
	hum.WalkSpeed = 16
end

function MYSPAGHETTTTTTT() --ow
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("1282149571", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://1329282756"
	CameraEnshaking(1, 2.2)
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(20), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(20), Rad(0), Rad(5)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(115 + 4), Rad(45), Rad(50)), 0.1)
	end
	for i = 0,5,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(110), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(110), Rad(0), Rad(0)), 0.1)
	end
	for i = 0,6,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(35), Rad(0), Rad(-10)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(35), Rad(0), Rad(10)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end


function dead()
	attack = true
	hum.WalkSpeed = 0.20
	CreateSound("137225991", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://297512410"
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(90), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(180), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(270), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(90), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(180), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(270), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
	for i = 0,1.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-90)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(140)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-140)), 0.1)
	end
        Character.Head.face.Texture = "rbxassetid://273309187"
	for i = 0,9,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -2.59 + 0.1) * angles(Rad(-90), Rad(90), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(30)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-30)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function hap() --much hap
	attack = true
	hum.WalkSpeed = 0.10
	CreateSound("363808674", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://315792941"
	for i = 0,12,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(180)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-0)), 0.1)
	end
	CreateSound("233168827", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://335761015"
	for i = 0,10,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(180)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-0)), 0.1)
	end
        CreateSound("363808674", hed, 10, 1)
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function HAAAAA() --KONO POWA
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://305068389"
        chargeup.Pitch = 1
	for i = 0,7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 1 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(-0)), 0.1)
	end
        Character.Head.face.Texture = "rbxassetid://313921371"
        chargeup:play()
	for i = 0,30,0.1 do
		swait()
		CameraEnshaking(1, 2)
                chargeup.Parent = hed
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(15), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(-0)), 0.1)
	end
        chargeup:stop()
        chargeup.Pitch = 1.1
        chargeup.TimePosition = 1
        chargeup:play()
        Character.Head.face.Texture = "rbxassetid://304942859"
        for i, v in pairs(c:children()) do
        if v.ClassName == "Part" then
        local tra = trazx:clone()
        tra.Parent = v
        tra.LightEmission = 1
        tra.Color = ColorSequence.new(Color3.new(0, 0.6666666666666666, 1))
        tra.Rate = 15
        tra.Rotation = NumberRange.new(-5, 5)
        tra.Lifetime = NumberRange.new(1.5, 2)
        tra.Size = NumberSequence.new({
          NumberSequenceKeypoint.new(0, 0.1, 0),
          NumberSequenceKeypoint.new(1, 0, 0)
        })
        tra.Transparency = NumberSequence.new({
          NumberSequenceKeypoint.new(0, 1, 0),
          NumberSequenceKeypoint.new(0.135, 0, 0),
          NumberSequenceKeypoint.new(0.875, 0, 0),
          NumberSequenceKeypoint.new(1, 1, 0)
        })
        tra.Speed = NumberRange.new(0.5)
        tra.VelocitySpread = 360
        tra.VelocityInheritance = 0.5
        tra.ZOffset = 2
        tra.Acceleration = Vector3.new(0, 2.5, 0)
      end
    end
    local tra = trazx:clone()
    tra.Parent = c.HumanoidRootPart
    tra.Texture = "rbxassetid://347730682"
    tra.LightEmission = 0.8
    tra.Color = ColorSequence.new(Color3.new(0, 0.6666666666666666, 1))
    tra.Rate = 250
    tra.Rotation = NumberRange.new(-5, 5)
    tra.Lifetime = NumberRange.new(0.75)
    tra.Size = NumberSequence.new({
      NumberSequenceKeypoint.new(0, 4.81, 0.875),
      NumberSequenceKeypoint.new(1, 2.13, 0.875)
    })
    tra.Transparency = NumberSequence.new({
      NumberSequenceKeypoint.new(0, 1, 0),
      NumberSequenceKeypoint.new(0.0399, 0.85, 0),
      NumberSequenceKeypoint.new(0.394, 0.9, 0),
      NumberSequenceKeypoint.new(0.699, 1, 0),
      NumberSequenceKeypoint.new(1, 1, 0)
    })
    tra.Speed = NumberRange.new(15)
    tra.VelocitySpread = 360
    tra.VelocityInheritance = 0.5
    tra.ZOffset = 3.5
    tra.Acceleration = Vector3.new(0, 25, 0)
	for i = 0,35,0.1 do
		swait()
                ohno.Parent = hed
		CameraEnshaking(1, 3)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(60), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(-0)), 0.1)
	end
        chargeup:stop()
        chargeup.Pitch = 1.3
        chargeup.TimePosition = 1
        chargeup:play()
        tra:Destroy()
        tra:Destroy()
        Character.Head.face.Texture = "rbxassetid://280233855"
    local tra = trazx:clone()
    tra.Parent = c.HumanoidRootPart
    tra.Texture = "rbxassetid://347730682"
    tra.LightEmission = 0.8
    tra.Color = ColorSequence.new(Color3.new(1, 0, 0))
    tra.Rate = 250
    tra.Rotation = NumberRange.new(-5, 5)
    tra.Lifetime = NumberRange.new(0.3)
    tra.Size = NumberSequence.new({
      NumberSequenceKeypoint.new(0, 8, 0.875),
      NumberSequenceKeypoint.new(1, 10, 0.875)
    })
    tra.Transparency = NumberSequence.new({
      NumberSequenceKeypoint.new(0, 1, 0),
      NumberSequenceKeypoint.new(0.0399, 0.531, 0),
      NumberSequenceKeypoint.new(0.394, 0.906, 0),
      NumberSequenceKeypoint.new(0.699, 1, 0),
      NumberSequenceKeypoint.new(1, 1, 0)
    })
	for i = 0,32,0.1 do
		swait()
		CameraEnshaking(1, 5)
                chargeup.Parent = hed
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-65), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.4, 0.0000000005 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(150), Rad(0), Rad(-0)), 0.1)
	end
        chargeup:stop()
        CreateSound("681582832", hed, 10, 1)
        game.Players.LocalPlayer.Character:BreakJoints()
        local S = Instance.new("Explosion",workspace)    
        S.Position = tors.Position
        S.BlastPressure = 9
        S.BlastRadius = 30
        S.ExplosionType = 0
	attack = false
	hum.WalkSpeed = 16
        Character.Head.face.Texture = "rbxassetid://295197013"
        tra:Destroy()
	CameraEnshaking(4, 30)
        error("WARNING, TO MUCH ENERGY.")
end

function NEN()
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("230292011", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://705269463"
	for i = 0,4,0.1 do
		swait()
		CameraEnshaking(1, 3)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-90), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function FLYSKYHIGH()
	attack = true
        timetofly = false
	hum.WalkSpeed = 0.05
        Character.Head.face.Texture = "rbxassetid://705269463"
        Cause_Im_having_a_good_time_having_a_good_time:Play()
        Cause_Im_having_a_good_time_having_a_good_time.TimePosition = 35.3
        Humanoid.JumpPower = 0
	for i = 0,300,0.1 do --thatsalongtime
		swait()
		CameraEnshaking(1, 7)
	        HitboxFunction(ll.CFrame, 0.01, 1, 1, 1, 7, 75, 500, 100, "Knockdown")
                Cause_Im_having_a_good_time_having_a_good_time.Parent = hed
                root.Velocity = root.CFrame.lookVector * 225
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0-255.45*i), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0-255.45*i)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0-255.45*i)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-75), Rad(0), Rad(0)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-75), Rad(0), Rad(0)), 0.1)
	end
        Cause_Im_having_a_good_time_having_a_good_time:Stop()
	attack = false
        Humanoid.JumpPower = 50
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
        wait(45)
        timetofly = true
        warn("You can FLY SKY HIGH Now! Go Nuts!") --please dont go nuts
end


function highnoon()
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("495316660", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://155195214"
	CameraEnshaking(2, 4)
	local Blobby = Instance.new("Part", char)
Blobby.Name = "Blob"
Blobby.CanCollide = false
Blobby.BrickColor = BrickColor.new("Really black")
Blobby.Transparency = 0
Blobby.Material = "Plastic"
Blobby.Size = Vector3.new(1, 1, 2)
Blobby.TopSurface = Enum.SurfaceType.Smooth
Blobby.BottomSurface = Enum.SurfaceType.Smooth

local Weld = Instance.new("Weld", Blobby)
Weld.Part0 = ra
Weld.Part1 = Blobby
Weld.C1 = CFrame.new(0, -.4, -1.6) *angles(Rad(180), Rad(0), Rad(180))
Weld.C0 = CFrame.Angles(math.rad(-90),0,0)

local M2 = Instance.new("SpecialMesh")
M2.Parent = Blobby
M2.MeshId = "http://www.roblox.com/asset/?id=432256490"
M2.TextureId = "http://www.roblox.com/asset/?id=432256526"
M2.Scale = Vector3.new(.002, .002, .002)
	for i = 0,7.75,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(90)), 0.2)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(30), Rad(0), Rad(0)), 0.2)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.2)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(-.6), Rad(180)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-45), Rad(-.6), Rad(136 - 4.5 * Sin(sine / 20))), 0.2)
        end
	for i = 0,16.5,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(90)), 0.2)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(30), Rad(0), Rad(0)), 0.2)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.2)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(-.6), Rad(90)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-45), Rad(-.6), Rad(136 - 4.5 * Sin(sine / 20))), 0.2)
	end
	Blobby.Transparency = 1
	Blobby:Destroy()
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function somuchcancerwhy() --o no
	attack = true
	hum.WalkSpeed = 0.10
        Character.Head.face.Texture = "rbxassetid://315074049"
        local A = math.random(1,13)
        if A == 1 then
            ohno.SoundId = "rbxassetid://295810519"
            ohno.TimePosition = 1
        end
        if A == 2 then
            ohno.SoundId = "rbxassetid://488472970"
            ohno.TimePosition = 2
        end
        if A == 3 then
            ohno.SoundId = "rbxassetid://917045199"
            ohno.TimePosition = 3
        end
        if A == 4 then
            ohno.SoundId = "rbxassetid://324205173"
            ohno.TimePosition = 1
        end
        if A == 5 then
            ohno.SoundId = "rbxassetid://376134741"
            ohno.TimePosition = 8
        end
        if A == 6 then
            ohno.SoundId = "rbxassetid://164147183"
            ohno.TimePosition = 0
        end
        if A == 7 then
            ohno.SoundId = "rbxassetid://825526716"
            ohno.TimePosition = 1
        end
        if A == 8 then
            ohno.SoundId = "rbxassetid://185460366"
            ohno.TimePosition = 0
        end
        if A == 9 then
            ohno.SoundId = "rbxassetid://273319633"
            ohno.TimePosition = 1
        end
        if A == 10 then
            ohno.SoundId = "rbxassetid://506212392"
            ohno.TimePosition = 2
        end
        if A == 11 then
            ohno.SoundId = "rbxassetid://708297448"
            ohno.TimePosition = 4
        end
        if A == 12 then
            ohno.SoundId = "rbxassetid://497199103"
            ohno.TimePosition = 9
        end
        if A == 13 then
            ohno.SoundId = "rbxassetid://152833989"
            ohno.TimePosition = 1
        end
        ohno:Play()
	for i = 0,100,0.1 do
		swait()
     		CameraEnshaking(2, 3)
                ohno.Parent = hed
	        char.Torso.Neck.C0 = char.Torso.Neck.C0 * CFrame.Angles(math.random(-10,10),math.random(-10,10),math.random(-10,10))
	end
	attack = false
        ohno:Stop()
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function WRY() --WRYYYYYYY
	attack = true
	hum.WalkSpeed = 0.30
	CreateSound("794081034", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://396389196"
	for i = 0,2,0.1 do
		swait()
		CameraEnshaking(1, 2)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(30), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(140), Rad(60)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(-140), Rad(-60)), 0.1)
	end
	for i = 0,14.7,0.1 do
		swait()
		CameraEnshaking(1, 3)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 1, -1 + 0.1) * angles(Rad(-75), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(65), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-70)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1.1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(70)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(45), Rad(0), Rad(40)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(45), Rad(-0), Rad(-40)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function BOI()
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("390901873", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://282463320"
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(50), Rad(90)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(-50), Rad(-90)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(30), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(140), Rad(60)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(-140), Rad(-60)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function WhatHuh()
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("130766865", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://276732672"
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
 	for i = 0,6.7,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(-26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
	for i = 0,8.1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
	for i = 0,1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(40), Rad(-26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
	for i = 0,1,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(40), Rad(26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
 	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(-26), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(120)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-120)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function NothingPersonal()
	if mouse.Target.Parent ~= char and mouse.Target.Parent.Parent ~= char and mouse.Target.Parent:FindFirstChildOfClass("Humanoid") ~= nil then
		local HITBODY = mouse.Target.Parent
		local TORS = HITBODY:FindFirstChild("Torso") or HITBODY:FindFirstChild("UpperTorso")
		local HEAD = HITBODY:FindFirstChild("Head")
		local HUMAN = mouse.Target.Parent:FindFirstChildOfClass("Humanoid")
		if TORS ~= nil and HUMAN ~= nil then
	attack = true
	root.CFrame = TORS.CFrame * CFrame.new(-1,0,3)
	TORS.Anchored = true
	hum.WalkSpeed = 0
        Character.Head.face.Texture = "rbxassetid://40770311"
	CreateSound("1255922819", hed, 10, 1)
	CameraEnshaking(2, 4)
		end
		wait(3.5)
		for i = 0,9,0.1 do
			swait()
			for i = 1,2 do
	                HitboxFunction(ll.CFrame, 0.01, 1, 1, 1, 7, 1, 10, 53, "Knockdown")
                        CameraEnshaking(1, 7)
			Effects.Sphere.Create(BrickColor.new("Persimmon"), TORS.CFrame*CFrame.new(math.random(-200,200)/100,math.random(-300,200)/100,math.random(-100,100)/100), 1, 1, 1, 15, 15, 15, 0.2)
		    end
		end
		wait(.5)
		TORS.Anchored = false
		attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
	end
end

function VeryMuchWorrying()
	attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://111523405"
	CreateSound("1395854043", hed, 10, 1)
	for i = 0,14,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.3, 0.9 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-145)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.3, 0.9 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(145)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function Ashes() --Straight from... Whatever it was called.
        attack = true
	hum.WalkSpeed = 1.01
        Character.Head.face.Texture = "rbxassetid://360687027"
	CreateSound("290084602", tors, 10, 1)
	for i = 0,6.2,0.1 do
			swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-30), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-5), Rad(0), Rad(-0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-0), Rad(0), Rad(145)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-0), Rad(0), Rad(-145)), 0.1)
	end
	for i = 0,6.2,0.1 do
			swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(20), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-5), Rad(0), Rad(20)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-5), Rad(0), Rad(-20)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-30), Rad(0), Rad(15)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-30), Rad(0), Rad(-15)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function AnotherOne() --WhAT ANOTHER ONE
	attack = true
	hum.WalkSpeed = 1.01
	local icri = CreateSound("1205111204", hed, 10, 1)
	swait(165)
	local FRAME = tors.CFrame
	repeat
		swait()
                Character.Head.face.Texture = "rbxassetid://582931093"
		CameraEnshaking(1, 10)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.3, 0.9 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(90)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.3, 0.9 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-90)), 0.1)
		tors.CFrame = FRAME * CF(0,1,0)
		swait()
		tors.CFrame = FRAME
	until icri.Playing == false
        Character.Head.face.Texture = "rbxassetid://620619801"
	attack = false
	hum.WalkSpeed = 16
end

function Dance()
	attack = true
	hum.WalkSpeed = 1.01
	CreateSound("838766490", hed, 10, 1)
        Character.Head.face.Texture = "rbxassetid://258591579"
	for i = 0,2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,4,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(-0), Rad(-180)), 0.1)
	end
	for i = 0,3,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(-30)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(50), Rad(0), Rad(180)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-50), Rad(-0), Rad(-180)), 0.1)
	end
	attack = false
        Character.Head.face.Texture = "rbxassetid://620619801"
	hum.WalkSpeed = 16
end

function kyu_will_break_your_neck_asdf_longest_function_name_ever_xd()
attack = true
        Character.Head.face.Texture = "rbxassetid://266304560"
	for i = 0,6,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.3, 0.9 + 0.05 * Sin(sine / 30), 0.2 * Cos(sine / 20)) * angles(Rad(170), Rad(0), Rad(-15)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.3, 0.8 + 0.05 * Sin(sine / 30), -0.025 * Cos(sine / 20)) * angles(Rad(140), Rad(0), Rad(15)), 0.1)
	end
    CreateSound("1093102664", hed, 10, 1)
	CameraEnshaking(3, 8)
	for i = 0,2,0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1) * angles(Rad(5), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(10), Rad(40), Rad(0)), 0.4)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 , 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.3, 0.9 + 0.05 * Sin(sine / 30), 0.2 * Cos(sine / 20)) * angles(Rad(200), Rad(0), Rad(-40)), 0.4)
		LW.C0 = clerp(LW.C0, CF(-1.3, 0.8 + 0.05 * Sin(sine / 30), -0.025 * Cos(sine / 20)) * angles(Rad(40), Rad(0), Rad(40)), 0.4)
	end
Character.Head.face.Texture = "rbxassetid://30128383"
hum.MaxHealth = 0
ragdoll(char)
CreateSound("534269232", hed, 5, 1)
error("Seems like you just died.")
end

MoreTaunts = false
mouse.KeyDown:connect(function(key)
	if attack == false then
		if MoreTaunts == false then
		if key == 'q' then
			GEtOuT()
                elseif key == 'e' then
                        GEtOuT2()
                elseif key == 'x' then
                        OBJECTION()
                elseif key == 'n' then
                        BOI()
                elseif key == 'u' then
                        Victory()
                elseif key == '3' then
                        hap()
                elseif key == '6' then
                        Flight()
                elseif key == '9' and timetofly then
                        FLYSKYHIGH()
                elseif key == '9' then
                        local A = math.random(1,10)
                        if A == 1 then
                            warn ("This has a Cooldown, Please wait. :>")
                        end
                        if A == 2 then
                            warn ("You can't Fly All day, you know.")
                        end
                        if A == 3 then
                            warn ("Calm down there.")
                        end
                        if A == 4 then
                            warn ("Take a Break.")
                        end
                        if A == 5 then
                            warn ("*Elevator Music plays in the backround*")
                        end
                        if A == 6 then
                            warn ("I know, You want to FLY SKY HIGH, but wait a little bit.")
                        end
                        if A == 7 then
                            warn ("Can you wait a LITTLE Longer?")
                        end
                        if A == 8 then
                            warn ("Like a tiger defying the laws of gravity...")
                        end
                        if A == 9 then
                            warn ("DON'T STOP ME NNNNNOOOOOOOOWWWW")
                        end
                        if A == 10 then
                            warn ("Oh, I'm burnin' through the sky, Yeah!")
                        end
                elseif key == 'k' then
                        Hello()
                elseif key == '5' then
                        HAAAAA()
                elseif key == '4' then
                        Dance()
                elseif key == '1' then
                        HELP()
		elseif key == '2' then
			dead()
                elseif key == 'j' then
                        WhatHuh()
		elseif key == 'l' then
			ShutTheHellUp()
                elseif key == 'c' then
                        Choose()
		elseif key == 'r' then
			MYSPAGHETTTTTTT()
		elseif key == 't' then
			SpinMeDad()
		elseif key == 'y' then
			EndMySufferingV2()
		elseif key == 'f' then
			NEN()
		elseif key == 'z' then
			NothingPersonal()
		elseif key == '7' then
			somuchcancerwhy()
		elseif key == '8' then
			highnoon()
		elseif key == 'v' then
			VeryMuchWorrying()
                elseif key == 'b' then
                        Ashes()
                elseif key == 'p' then
                        kyu_will_break_your_neck_asdf_longest_function_name_ever_xd()
                elseif key == 'g' then
                        AnotherOne()
                elseif key == 'h' then
                        slap()
                elseif key == 'm' then
                        WRY()
		end
		end
		end
	end)

-------------------------------------------------------
--End Attacks N Stuff--
-------------------------------------------------------




while jumping do
 Humanoid.Jump = true
 wait(0.9)
end




-------------------------------------------------------
--Start Animations--
-------------------------------------------------------
local equipped = false
local idle = 0
local change = 1
local val = 0
local toim = 0
local idleanim = 0.4
hum.Animator.Parent = nil
while true do
	swait()
	sine = sine + change
	local torvel = (root.Velocity * Vector3.new(1, 0, 1)).magnitude
	local velderp = root.Velocity.y
	hitfloor, posfloor = rayCast(root.Position, CFrame.new(root.Position, root.Position - Vector3.new(0, 1, 0)).lookVector, 4, char)
	if equipped == true or equipped == false then
		if attack == false then
			idle = idle + 1
		else
			idle = 0
		end
		if 1 < root.Velocity.y and hitfloor == nil then
			Anim = "Jump"
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(10), Rad(0), Rad(0)), 0.3)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-40), Rad(0), Rad(0)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.5, 0.5, 0) * angles(Rad(30), Rad(0), Rad(20)), 0.3)
				LW.C0 = clerp(LW.C0, CF(-1.5, 0.5, 0) * angles(Rad(-20), Rad(0), Rad(-30)), 0.3)
				LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), -0.3) * LHCF * angles(Rad(-5), Rad(0), Rad(20)), 0.15)
				RH.C0 = clerp(RH.C0, CF(1, -1, 0.3) * angles(Rad(0), Rad(90), Rad(-20)), 0.3)
			end
		elseif -1 > root.Velocity.y and hitfloor == nil then
			Anim = "Fall"
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(-5), Rad(0), Rad(0)), 0.3)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(40), Rad(0), Rad(0)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.5, 0.5, 0) * angles(Rad(30), Rad(0), Rad(20)), 0.3)
				LW.C0 = clerp(LW.C0, CF(-1.5, 0.5, 0) * angles(Rad(-20), Rad(0), Rad(-30)), 0.3)
				LH.C0=clerp(LH.C0, CF(-1,-.4-0.1 * Cos(sine / 20), -.6) * LHCF * angles(Rad(-5), Rad(-0), Rad(20)), 0.15)
				RH.C0=clerp(RH.C0, CF(1,-.3-0.1 * Cos(sine / 20), -.6) * angles(Rad(0), Rad(90), Rad(-20)), .3)
			end
		elseif torvel < 1 and hitfloor ~= nil then
			Anim = "Idle"
			change = 1
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
				RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(30 * Cos(sine / 20)), Rad(0), Rad(5)), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(30 * Cos(sine / 20)), Rad(0), Rad(-5)), 0.1)
			end
		elseif tors.Velocity.magnitude < 50 and hitfloor ~= nil then
			Anim = "Walk"
			change = 1
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.175 + 0.025 * Cos(sine / 3.5) + -Sin(sine / 3.5) / 7) * angles(Rad(9-2.5 * Cos(sine / 3.5)), Rad(0), Rad(10 * Cos(sine / 7))), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
				RH.C0 = clerp(RH.C0, CFrame.new(1, -0.925 - 0.5 * math.cos(sine / 7) / 2, 0.5 * math.cos(sine / 7) / 2) * angles(math.rad(-15 - 35 * math.cos(sine / 7)) + -math.sin(sine / 7) / 2.5, math.rad(90 - 2 * math.cos(sine / 7)), math.rad(0)) * angles(math.rad(0 + 2.5 * math.cos(sine / 7)), math.rad(0), math.rad(0)), 0.3)
         		LH.C0 = clerp(LH.C0, CFrame.new(-1, -0.925 + 0.5 * math.cos(sine / 7) / 2, -0.5 * math.cos(sine / 7) / 2) * angles(math.rad(-15 + 35 * math.cos(sine / 7)) + math.sin(sine / 7) / 2.5, math.rad(-90 - 2 * math.cos(sine / 7)), math.rad(0)) * angles(math.rad(0 - 2.5 * math.cos(sine / 7)), math.rad(0), math.rad(0)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(70) * Cos(sine / 7) , Rad(0), Rad(5)), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(-70) * Cos(sine / 7) , Rad(0),	Rad(-5)), 0.1)
			end
		end
	end
	if 0 < #Effects then
		for e = 1, #Effects do
			if Effects[e] ~= nil then
				local Thing = Effects[e]
				if Thing ~= nil then
					local Part = Thing[1]
					local Mode = Thing[2]
					local Delay = Thing[3]
					local IncX = Thing[4]
					local IncY = Thing[5]
					local IncZ = Thing[6]
					if 1 >= Thing[1].Transparency then
						if Thing[2] == "Block1" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block2" then
							Thing[1].CFrame = Thing[1].CFrame + Vector3.new(0, 0, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block3" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50)) + Vector3.new(0, 0.15, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Cylinder" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Blood" then
							local Mesh = Thing[7]
							Thing[1].CFrame = Thing[1].CFrame * Vector3.new(0, 0.5, 0)
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Elec" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[7], Thing[8], Thing[9])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Disappear" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Shatter" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
							Thing[4] = Thing[4] * CFrame.new(0, Thing[7], 0)
							Thing[1].CFrame = Thing[4] * CFrame.fromEulerAnglesXYZ(Thing[6], 0, 0)
							Thing[6] = Thing[6] + Thing[5]
						end
					else
						Part.Parent = nil
						table.remove(Effects, e)
					end
				end
			end
		end
	end
end
-------------------------------------------------------
--End Animations And Script--
-------------------------------------------------------

--cool beans boibiparti("Clicked")
                        end)


						section1:addButton("Fe Animations R15", function()
							loadstring(game:HttpGet("https://gitlab.com/Tsuniox/lua-stuff/-/raw/master/R15GUI.lua"))()("Clicked")
							end)


							section1:addButton("FE Big Head (Rthro)", function()
								loadstring(game:HttpGetAsync('https://raw.githubusercontent.com/ProjectBoring/Head-Resize-v1/main/Head%20Size%20Obfuscated.lua'))()("Clicked")
								end)


								section1:addButton("FE Pose", function()
									--[[
	fePose v3 by xyz#9892
	Bonus : use this image as a t-shirt with 'Heli' for fun times https://files.catbox.moe/0fr6f3.png

    Synapse Xen v1.1.2 by Synapse GP
    VM Hash: 0f3cf787bb24e72f24af8822bd081bccc97e63b53d643d301a614c666b73d514
]]

local SynapseXen_iIilillll=select;local SynapseXen_lilliIllI=string.byte;local SynapseXen_IiiIiIi=string.sub;local SynapseXen_Iillililllli=string.char;local SynapseXen_iilliIilIiiili=type;local SynapseXen_iIilIliill=table.concat;local unpack=unpack;local setmetatable=setmetatable;local pcall=pcall;local SynapseXen_iIliiIIlIll,SynapseXen_llliIlIliil,SynapseXen_iIIlIlliIliI,SynapseXen_IllillliiIl;if bit and bit.bxor then SynapseXen_iIliiIIlIll=bit.bxor;SynapseXen_llliIlIliil=function(SynapseXen_lllIiillilIliii,SynapseXen_IlIiIIl)local SynapseXen_iiliIIiil=SynapseXen_iIliiIIlIll(SynapseXen_lllIiillilIliii,SynapseXen_IlIiIIl)if SynapseXen_iiliIIiil<0 then SynapseXen_iiliIIiil=4294967296+SynapseXen_iiliIIiil end;return SynapseXen_iiliIIiil end else SynapseXen_iIliiIIlIll=function(SynapseXen_lllIiillilIliii,SynapseXen_IlIiIIl)local SynapseXen_IIiiliI=function(SynapseXen_lIIiliiiIllllIlllIii,SynapseXen_IIIlIilI)return SynapseXen_lIIiliiiIllllIlllIii%(SynapseXen_IIIlIilI*2)>=SynapseXen_IIIlIilI end;local SynapseXen_liilIIllIIIl=0;for SynapseXen_iiIII=0,31 do SynapseXen_liilIIllIIIl=SynapseXen_liilIIllIIIl+(SynapseXen_IIiiliI(SynapseXen_lllIiillilIliii,2^SynapseXen_iiIII)~=SynapseXen_IIiiliI(SynapseXen_IlIiIIl,2^SynapseXen_iiIII)and 2^SynapseXen_iiIII or 0)end;return SynapseXen_liilIIllIIIl end;SynapseXen_llliIlIliil=SynapseXen_iIliiIIlIll end;SynapseXen_iIIlIlliIliI=function(SynapseXen_iiIiIii,SynapseXen_llilIiIlllIil,SynapseXen_liiillIiiliIlii)return(SynapseXen_iiIiIii+SynapseXen_llilIiIlllIil)%SynapseXen_liiillIiiliIlii end;SynapseXen_IllillliiIl=function(SynapseXen_iiIiIii,SynapseXen_llilIiIlllIil,SynapseXen_liiillIiiliIlii)return(SynapseXen_iiIiIii-SynapseXen_llilIiIlllIil)%SynapseXen_liiillIiiliIlii end;local function SynapseXen_lIiillilIllIlIiIi(SynapseXen_iiliIIiil)if SynapseXen_iiliIIiil<0 then SynapseXen_iiliIIiil=4294967296+SynapseXen_iiliIIiil end;return SynapseXen_iiliIIiil end;local getfenv=getfenv;if not getfenv then getfenv=function()return _ENV end end;local SynapseXen_iiillI={}local SynapseXen_ilIiII={}local SynapseXen_lliiiIiIIIlilI;local SynapseXen_iIiiIlilll;local SynapseXen_lIIIiI={}local SynapseXen_illIllIIillllIIl={}for SynapseXen_iiIII=0,255 do local SynapseXen_lllIlI,SynapseXen_IIiIIlillI=SynapseXen_Iillililllli(SynapseXen_iiIII),SynapseXen_Iillililllli(SynapseXen_iiIII,0)SynapseXen_lIIIiI[SynapseXen_lllIlI]=SynapseXen_IIiIIlillI;SynapseXen_illIllIIillllIIl[SynapseXen_IIiIIlillI]=SynapseXen_lllIlI end;local function SynapseXen_lilIlIiIIlll(SynapseXen_IiiIiIIlIIlllI,SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli)if SynapseXen_iIlilllIIiliiliII>=256 then SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli=0,SynapseXen_IIlIlIliIIiiilIIli+1;if SynapseXen_IIlIlIliIIiiilIIli>=256 then SynapseXen_llliII={}SynapseXen_IIlIlIliIIiiilIIli=1 end end;SynapseXen_llliII[SynapseXen_Iillililllli(SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli)]=SynapseXen_IiiIiIIlIIlllI;SynapseXen_iIlilllIIiliiliII=SynapseXen_iIlilllIIiliiliII+1;return SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli end;local function SynapseXen_iIIliIiIiilllliIilli(SynapseXen_lIIlIiiiilIlIIi)local function SynapseXen_liIiilliiIilIllI(SynapseXen_lIliliIIIiIliilll)local SynapseXen_IIlIlIliIIiiilIIli='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'SynapseXen_lIliliIIIiIliilll=string.gsub(SynapseXen_lIliliIIIiIliilll,'[^'..SynapseXen_IIlIlIliIIiiilIIli..'=]','')return SynapseXen_lIliliIIIiIliilll:gsub('.',function(SynapseXen_iiIiIii)if SynapseXen_iiIiIii=='='then return''end;local SynapseXen_lilIlllil,SynapseXen_iiIIillII='',SynapseXen_IIlIlIliIIiiilIIli:find(SynapseXen_iiIiIii)-1;for SynapseXen_iiIII=6,1,-1 do SynapseXen_lilIlllil=SynapseXen_lilIlllil..(SynapseXen_iiIIillII%2^SynapseXen_iiIII-SynapseXen_iiIIillII%2^(SynapseXen_iiIII-1)>0 and'1'or'0')end;return SynapseXen_lilIlllil end):gsub('%d%d%d?%d?%d?%d?%d?%d?',function(SynapseXen_iiIiIii)if#SynapseXen_iiIiIii~=8 then return''end;local SynapseXen_lIiIIiiIlill=0;for SynapseXen_iiIII=1,8 do SynapseXen_lIiIIiiIlill=SynapseXen_lIiIIiiIlill+(SynapseXen_iiIiIii:sub(SynapseXen_iiIII,SynapseXen_iiIII)=='1'and 2^(8-SynapseXen_iiIII)or 0)end;return string.char(SynapseXen_lIiIIiiIlill)end)end;SynapseXen_lIIlIiiiilIlIIi=SynapseXen_liIiilliiIilIllI(SynapseXen_lIIlIiiiilIlIIi)local SynapseXen_lIlIIl=SynapseXen_IiiIiIi(SynapseXen_lIIlIiiiilIlIIi,1,1)if SynapseXen_lIlIIl=="u"then return SynapseXen_IiiIiIi(SynapseXen_lIIlIiiiilIlIIi,2)elseif SynapseXen_lIlIIl~="c"then error("Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")end;SynapseXen_lIIlIiiiilIlIIi=SynapseXen_IiiIiIi(SynapseXen_lIIlIiiiilIlIIi,2)local SynapseXen_iililiiIlIIl=#SynapseXen_lIIlIiiiilIlIIi;local SynapseXen_llliII={}local SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli=0,1;local SynapseXen_IIiiiiiIIIIIiIlIliI={}local SynapseXen_iiliIIiil=1;local SynapseXen_IllililI=SynapseXen_IiiIiIi(SynapseXen_lIIlIiiiilIlIIi,1,2)SynapseXen_IIiiiiiIIIIIiIlIliI[SynapseXen_iiliIIiil]=SynapseXen_illIllIIillllIIl[SynapseXen_IllililI]or SynapseXen_llliII[SynapseXen_IllililI]SynapseXen_iiliIIiil=SynapseXen_iiliIIiil+1;for SynapseXen_iiIII=3,SynapseXen_iililiiIlIIl,2 do local SynapseXen_liIliIIlllIlIli=SynapseXen_IiiIiIi(SynapseXen_lIIlIiiiilIlIIi,SynapseXen_iiIII,SynapseXen_iiIII+1)local SynapseXen_IIlililiIillli=SynapseXen_illIllIIillllIIl[SynapseXen_IllililI]or SynapseXen_llliII[SynapseXen_IllililI]if not SynapseXen_IIlililiIillli then error("Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")end;local SynapseXen_lIlllIIilIiIli=SynapseXen_illIllIIillllIIl[SynapseXen_liIliIIlllIlIli]or SynapseXen_llliII[SynapseXen_liIliIIlllIlIli]if SynapseXen_lIlllIIilIiIli then SynapseXen_IIiiiiiIIIIIiIlIliI[SynapseXen_iiliIIiil]=SynapseXen_lIlllIIilIiIli;SynapseXen_iiliIIiil=SynapseXen_iiliIIiil+1;SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli=SynapseXen_lilIlIiIIlll(SynapseXen_IIlililiIillli..SynapseXen_IiiIiIi(SynapseXen_lIlllIIilIiIli,1,1),SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli)else local SynapseXen_lliIlllIlliIIIiIIii=SynapseXen_IIlililiIillli..SynapseXen_IiiIiIi(SynapseXen_IIlililiIillli,1,1)SynapseXen_IIiiiiiIIIIIiIlIliI[SynapseXen_iiliIIiil]=SynapseXen_lliIlllIlliIIIiIIii;SynapseXen_iiliIIiil=SynapseXen_iiliIIiil+1;SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli=SynapseXen_lilIlIiIIlll(SynapseXen_lliIlllIlliIIIiIIii,SynapseXen_llliII,SynapseXen_iIlilllIIiliiliII,SynapseXen_IIlIlIliIIiiilIIli)end;SynapseXen_IllililI=SynapseXen_liIliIIlllIlIli end;return SynapseXen_iIilIliill(SynapseXen_IIiiiiiIIIIIiIlIliI)end;local function SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIiilIIilil,SynapseXen_ilIllIiII,SynapseXen_lliIi)if SynapseXen_lliIi then local SynapseXen_ilIiliIlllllIli=SynapseXen_IIllIIiilIIilil/2^(SynapseXen_ilIllIiII-1)%2^(SynapseXen_lliIi-1-(SynapseXen_ilIllIiII-1)+1)return SynapseXen_ilIiliIlllllIli-SynapseXen_ilIiliIlllllIli%1 else local SynapseXen_IIlIilI=2^(SynapseXen_ilIllIiII-1)if SynapseXen_IIllIIiilIIilil%(SynapseXen_IIlIilI+SynapseXen_IIlIilI)>=SynapseXen_IIlIilI then return 1 else return 0 end end end;local function SynapseXen_iiIiIiIiillIill()local SynapseXen_iIIIIiiIll=SynapseXen_iIliiIIlIll(2929888148,SynapseXen_iIiiIlilll)while true do if SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(3472549852,SynapseXen_iIiiIlilll)then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-38955,SynapseXen_lIiilIiil-13404)+SynapseXen_iIliiIIlIll(2685354864,SynapseXen_iIiiIlilll)end;SynapseXen_iIIIIiiIll=SynapseXen_iIIIIiiIll-SynapseXen_iIliiIIlIll(4025885346,SynapseXen_ilIiII[4])elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(3710622187,SynapseXen_iIiiIlilll)then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-6605,SynapseXen_lIiilIiil-8548)-SynapseXen_iIliiIIlIll(4025904275,SynapseXen_ilIiII[4])end;SynapseXen_iIIIIiiIll=SynapseXen_iIliiIIlIll(SynapseXen_iIIIIiiIll,SynapseXen_iIliiIIlIll(3017715608,SynapseXen_iIiiIlilll))elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(3472547819,SynapseXen_iIiiIlilll)then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-22260,SynapseXen_lIiilIiil+18364)-SynapseXen_iIliiIIlIll(441325045,SynapseXen_ilIiII[3])end;SynapseXen_iIIIIiiIll=SynapseXen_iIliiIIlIll(SynapseXen_iIIIIiiIll,SynapseXen_iIliiIIlIll(398802931,SynapseXen_iIiiIlilll))elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(289157059,SynapseXen_iIiiIlilll)then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII+21780,SynapseXen_lIiilIiil-30413)-SynapseXen_iIliiIIlIll(441380668,SynapseXen_ilIiII[3])end;SynapseXen_iIIIIiiIll=SynapseXen_iIIIIiiIll+SynapseXen_iIliiIIlIll(2685373110,SynapseXen_iIiiIlilll)elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(3379591250,SynapseXen_ilIiII[2])then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-34214,SynapseXen_lIiilIiil+41329)+SynapseXen_iIliiIIlIll(2685365800,SynapseXen_iIiiIlilll)end;SynapseXen_iIIIIiiIll=SynapseXen_iIliiIIlIll(SynapseXen_iIIIIiiIll,SynapseXen_iIliiIIlIll(1349055484,SynapseXen_ilIiII[4]))elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(2813786483,SynapseXen_ilIiII[5])then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII+43561,SynapseXen_lIiilIiil-39595)-SynapseXen_iIliiIIlIll(2685402581,SynapseXen_iIiiIlilll)end;SynapseXen_iIIIIiiIll=SynapseXen_iIIIIiiIll+SynapseXen_iIliiIIlIll(377815323,SynapseXen_ilIiII[5])elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(2876906814,SynapseXen_ilIiII[3])then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII+31913,SynapseXen_lIiilIiil-19894)+SynapseXen_iIliiIIlIll(377842943,SynapseXen_ilIiII[5])end;SynapseXen_iIIIIiiIll=SynapseXen_iIIIIiiIll+SynapseXen_iIliiIIlIll(2685392722,SynapseXen_iIiiIlilll)elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(2463259085,SynapseXen_ilIiII[4])then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-3758,SynapseXen_lIiilIiil+39323)+SynapseXen_iIliiIIlIll(2685389667,SynapseXen_iIiiIlilll)end;SynapseXen_iIIIIiiIll=SynapseXen_iIIIIiiIll-SynapseXen_iIliiIIlIll(2685393747,SynapseXen_iIiiIlilll)elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(289124653,SynapseXen_iIiiIlilll)then SynapseXen_lliiiIiIIIlilI=function(SynapseXen_IIillIIII,SynapseXen_lIiilIiil)return SynapseXen_iIliiIIlIll(SynapseXen_IIillIIII-262,SynapseXen_lIiilIiil+23787)+SynapseXen_iIliiIIlIll(3353158259,SynapseXen_ilIiII[2])end;SynapseXen_iIIIIiiIll=SynapseXen_iIliiIIlIll(SynapseXen_iIIIIiiIll,SynapseXen_iIliiIIlIll(1814028909,SynapseXen_iIiiIlilll))elseif SynapseXen_iIIIIiiIll==SynapseXen_iIliiIIlIll(2033201591,SynapseXen_iIiiIlilll)then return end end end;local function SynapseXen_lillilI(SynapseXen_iiliIIliiIII)local SynapseXen_llIIi=1;local SynapseXen_iIiIlIiillilIIlIiIlI;local SynapseXen_iIIliililIIIliiIilll;local function SynapseXen_liliIlililI()local SynapseXen_IiilIIlIli=SynapseXen_lilliIllI(SynapseXen_iiliIIliiIII,SynapseXen_llIIi,SynapseXen_llIIi)SynapseXen_llIIi=SynapseXen_llIIi+1;return SynapseXen_IiilIIlIli end;local function SynapseXen_iiIiI()local SynapseXen_IIillliliiIIlIIIli,SynapseXen_IIillIIII,SynapseXen_lIiilIiil,SynapseXen_lIllIIIIill=SynapseXen_lilliIllI(SynapseXen_iiliIIliiIII,SynapseXen_llIIi,SynapseXen_llIIi+3)SynapseXen_llIIi=SynapseXen_llIIi+4;return SynapseXen_lIllIIIIill*16777216+SynapseXen_lIiilIiil*65536+SynapseXen_IIillIIII*256+SynapseXen_IIillliliiIIlIIIli end;local function SynapseXen_lIiiIiiIll()return SynapseXen_iiIiI()*4294967296+SynapseXen_iiIiI()end;local function SynapseXen_IIlliI()local SynapseXen_liiiii=SynapseXen_llliIlIliil(SynapseXen_iiIiI(),SynapseXen_iiillI[2867172854]or(function(...)local SynapseXen_iiIiIii="this is a christian obfuscator, no cursing allowed in our scripts"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(933783939,2135690172)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2357443719,2357381213)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2867172854]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1465324680,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4030617846,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3257630531,3179008585,3260192027,3358550061,1508340460}return SynapseXen_iiillI[2867172854]end)("IIiIIIlIiIII",{}))local SynapseXen_iiIlIiiIiilIiIliiIiI=SynapseXen_llliIlIliil(SynapseXen_iiIiI(),SynapseXen_iiillI[20274114]or(function()local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"SynapseXen_iiillI[20274114]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(632397218,1390566717),SynapseXen_iIliiIIlIll(1500795330,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{2684170833,2926351254,3600757111,2689563137}return SynapseXen_iiillI[20274114]end)())local SynapseXen_illllllIiiiiII=1;local SynapseXen_lIlIlIII=SynapseXen_IllIIlilllIilii(SynapseXen_iiIlIiiIiilIiIliiIiI,1,20)*2^32+SynapseXen_liiiii;local SynapseXen_IIlIiliIIilIiii=SynapseXen_IllIIlilllIilii(SynapseXen_iiIlIiiIiilIiIliiIiI,21,31)local SynapseXen_liililiIlI=(-1)^SynapseXen_IllIIlilllIilii(SynapseXen_iiIlIiiIiilIiIliiIiI,32)if SynapseXen_IIlIiliIIilIiii==0 then if SynapseXen_lIlIlIII==0 then return SynapseXen_liililiIlI*0 else SynapseXen_IIlIiliIIilIiii=1;SynapseXen_illllllIiiiiII=0 end elseif SynapseXen_IIlIiliIIilIiii==2047 then if SynapseXen_lIlIlIII==0 then return SynapseXen_liililiIlI*1/0 else return SynapseXen_liililiIlI*0/0 end end;return math.ldexp(SynapseXen_liililiIlI,SynapseXen_IIlIiliIIilIiii-1023)*(SynapseXen_illllllIiiiiII+SynapseXen_lIlIlIII/2^52)end;local function SynapseXen_liiilIiiIlIIiII(SynapseXen_iliIIIlIillIiiiil)local SynapseXen_iiiiiIIIiI;if SynapseXen_iliIIIlIillIiiiil then SynapseXen_iiiiiIIIiI=SynapseXen_IiiIiIi(SynapseXen_iiliIIliiIII,SynapseXen_llIIi,SynapseXen_llIIi+SynapseXen_iliIIIlIillIiiiil-1)SynapseXen_llIIi=SynapseXen_llIIi+SynapseXen_iliIIIlIillIiiiil else SynapseXen_iliIIIlIillIiiiil=SynapseXen_iIiIlIiillilIIlIiIlI()if SynapseXen_iliIIIlIillIiiiil==0 then return""end;SynapseXen_iiiiiIIIiI=SynapseXen_IiiIiIi(SynapseXen_iiliIIliiIII,SynapseXen_llIIi,SynapseXen_llIIi+SynapseXen_iliIIIlIillIiiiil-1)SynapseXen_llIIi=SynapseXen_llIIi+SynapseXen_iliIIIlIillIiiiil end;return SynapseXen_iiiiiIIIiI end;local function SynapseXen_llIIiiiIlliIIIil(SynapseXen_iiiiiIIIiI)local SynapseXen_ilIiliIlllllIli={}for SynapseXen_iiIII=1,#SynapseXen_iiiiiIIIiI do local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iiiiiIIIiI:sub(SynapseXen_iiIII,SynapseXen_iiIII)SynapseXen_ilIiliIlllllIli[#SynapseXen_ilIiliIlllllIli+1]=string.char(SynapseXen_iIliiIIlIll(string.byte(SynapseXen_IIiiiiliiIiiilliiIIl),SynapseXen_iiillI[1231556952]or(function()local SynapseXen_iiIiIii="print(bytecode)"SynapseXen_iiillI[1231556952]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(339349408,3497896674),SynapseXen_iIliiIIlIll(60742304,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{1903766033}return SynapseXen_iiillI[1231556952]end)()))end;return table.concat(SynapseXen_ilIiliIlllllIli)end;local function SynapseXen_lIlIIIIiIiIIIIllli()local SynapseXen_iIlIlI={}local SynapseXen_IlIIii={}local SynapseXen_ilIIlIIii={}local SynapseXen_IiliIIiIillIlll={[SynapseXen_iiillI[812408351]or(function()local SynapseXen_iiIiIii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"SynapseXen_iiillI[812408351]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1711067959,397089193),SynapseXen_iIliiIIlIll(2301518975,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3580245669,1294566774,2918343162}return SynapseXen_iiillI[812408351]end)()]=SynapseXen_IlIIii,[SynapseXen_iiillI[131045904]or(function(...)local SynapseXen_iiIiIii="epic gamer vision"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3392666309,3418601261)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4041999943,4041977591)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[131045904]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(830323467,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3192302592,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1869655077,4128433151,1744875165,891606754,3839983238,1966702642,1876543644,4056702267,262975930,550332910}return SynapseXen_iiillI[131045904]end)(528,"liliiiIli","liIilIl",{},{},"llIlllilIilIIlIIIiI",{},14877,{})]=SynapseXen_iIlIlI,[SynapseXen_iiillI[1466143109]or(function()local SynapseXen_iiIiIii="luraph better then xen bros :pensive:"SynapseXen_iiillI[1466143109]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3626654986,3155797014),SynapseXen_iIliiIIlIll(3226459732,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{409892361}return SynapseXen_iiillI[1466143109]end)()]=SynapseXen_ilIIlIIii}SynapseXen_liliIlililI()for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_iIliiIIlIll(SynapseXen_iIIliililIIIliiIilll(),SynapseXen_iiillI[3405566945]or(function(...)local SynapseXen_iiIiIii="xen detects custom getfenv"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1632463251,206054784)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3834795761,3834729382)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3405566945]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3490230041,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2060004376,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1853689763,2807962519,2468021334,829637965,3104367071}return SynapseXen_iiillI[3405566945]end)({},"iliilIliIiiIilIl",{},{},{},{},8306,{},{}))do local SynapseXen_IIllIIIliil=SynapseXen_iIliiIIlIll(SynapseXen_iiIiI(),SynapseXen_iiillI[2517750359]or(function(...)local SynapseXen_iiIiIii="SYNAPSE XEN [FE BYPASS] [BETTER THEN LURAPH] [AMAZING] OMG OMG OMG !!!!!!"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(115532603,1326382293)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2712826930,2712803534)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2517750359]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1285152470,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4225894558,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1702102669,2512347900,2727922265,1133469430,4161576706,1216605081,1832694869}return SynapseXen_iiillI[2517750359]end)({},"IIIIIlll","Iill",{},{},{},4402,"ilIilIIliiiliilIl",{}))local SynapseXen_iIIIlIli=SynapseXen_liliIlililI()SynapseXen_iiIiI()local SynapseXen_iilliIilIiiili=SynapseXen_liliIlililI()local SynapseXen_lIIiiI={[103297735]=SynapseXen_IIllIIIliil,[1706407771]=SynapseXen_iIIIlIli,[1153474865]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,1,6),[909370254]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,7,14)}if SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[157518667]or(function()local SynapseXen_iiIiIii="now with shitty xor string obfuscation"SynapseXen_iiillI[157518667]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4024365487,3921233318),SynapseXen_iIliiIIlIll(283352073,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{740013425}return SynapseXen_iiillI[157518667]end)())then SynapseXen_lIIiiI[1776583566]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,24,32)SynapseXen_lIIiiI[1617625881]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,15,23)elseif SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[4012033107]or(function(...)local SynapseXen_iiIiIii="now comes with a free n word pass"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(492320702,4177840271)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1499523126,1499510316)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[4012033107]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3314694406,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(999174296,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{3910128531,2409893811,1371247290}return SynapseXen_iiillI[4012033107]end)({},{},{},"II"))then SynapseXen_lIIiiI[1786993118]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,15,32)elseif SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[225785438]or(function(...)local SynapseXen_iiIiIii="pain is gonna use the backspace method on xen"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3999079419,970034332)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1595942352,1595892883)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[225785438]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1709837649,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1975254696,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{838102761,168907897,2096250338,185183760,2983752828,435215687,629088503,3134824394}return SynapseXen_iiillI[225785438]end)("iiiiiilIIillIi",{}))then SynapseXen_lIIiiI[1300471799]=SynapseXen_IllIIlilllIilii(SynapseXen_IIllIIIliil,15,32)-131071 end;SynapseXen_ilIIlIIii[SynapseXen_IiIilIiIIliiiil]=SynapseXen_lIIiiI end;for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_iIliiIIlIll(SynapseXen_iIIliililIIIliiIilll(),SynapseXen_iiillI[707113510]or(function(...)local SynapseXen_iiIiIii="hi my 2.5mb script doesn't work with xen please help"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1261763073,205904140)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2551731234,2551661895)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[707113510]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2446157264,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3281549808,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1693395079,4015926431,3149288360,324453220,1083261133,3799337158,1324476082,1516510424}return SynapseXen_iiillI[707113510]end)("IiiliIIlIlIliIllli",11255,"IllIlIIli",1528,8067,8527,{},9453,6292))do SynapseXen_liliIlililI()local SynapseXen_iilliIilIiiili=SynapseXen_liliIlililI()SynapseXen_iiIiI()local SynapseXen_lIlil;if SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[2966794722]or(function(...)local SynapseXen_iiIiIii="wow xen is shit buy luraph ok"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2995287169,421040458)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3413025232,3412975176)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2966794722]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3471217248,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3313104759,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1642801663,3696477400,2215348919}return SynapseXen_iiillI[2966794722]end)("IIiilIlllIliilIIIl",2225,"llIIiiiilii",14458))then SynapseXen_lIlil=SynapseXen_liliIlililI()~=0 elseif SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[3521202008]or(function()local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"SynapseXen_iiillI[3521202008]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(947900258,4040845864),SynapseXen_iIliiIIlIll(659780593,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{3234692239,1385360342,4100469837}return SynapseXen_iiillI[3521202008]end)())then SynapseXen_lIlil=SynapseXen_IIlliI()elseif SynapseXen_iilliIilIiiili==(SynapseXen_iiillI[4083901107]or(function()local SynapseXen_iiIiIii="xen best rerubi paste"SynapseXen_iiillI[4083901107]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1721949300,1640925226),SynapseXen_iIliiIIlIll(300851759,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{237051250,64639938,34244744,1138010818,1745494109,3579755847,4082688232,730180073,2036826862}return SynapseXen_iiillI[4083901107]end)())then SynapseXen_lIlil=SynapseXen_IiiIiIi(SynapseXen_llIIiiiIlliIIIil(SynapseXen_liiilIiiIlIIiII()),1,-2)end;SynapseXen_iIlIlI[SynapseXen_IiIilIiIIliiiil-1]=SynapseXen_lIlil end;SynapseXen_iiIiI()SynapseXen_liliIlililI()SynapseXen_iiIiI()SynapseXen_IiliIIiIillIlll[1439547849]=SynapseXen_iIliiIIlIll(SynapseXen_liliIlililI(),SynapseXen_iiillI[1806801961]or(function()local SynapseXen_iiIiIii="this is so sad, alexa play ripull.mp4"SynapseXen_iiillI[1806801961]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4067942171,1795807206),SynapseXen_iIliiIIlIll(1988840659,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{1699667299,762107901,1600940560,2642672117,2108480232,3031940368,3126204375,4237255588,208716470}return SynapseXen_iiillI[1806801961]end)())SynapseXen_iiIiI()SynapseXen_iiIiI()SynapseXen_IiliIIiIillIlll[1958689028]=SynapseXen_iIliiIIlIll(SynapseXen_liliIlililI(),SynapseXen_iiillI[2664981758]or(function(...)local SynapseXen_iiIiIii="wait for someone on devforum to say they are gonna deobfuscate this"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1953313623,1225295472)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2679833299,2679812083)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2664981758]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3401843359,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(813514598,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{4033184031,2251834055,213711793,568506984,3969821210,3268685690}return SynapseXen_iiillI[2664981758]end)({},"lIIiIIliliIiIIliilI","IlIIliiii","lIiIiiIiiiliili",{},3693,{},{},9144))SynapseXen_iiIiI()SynapseXen_liliIlililI()for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_iIliiIIlIll(SynapseXen_iIIliililIIIliiIilll(),SynapseXen_iiillI[2490865411]or(function()local SynapseXen_iiIiIii="pain exist is gonna connect the dots of xen"SynapseXen_iiillI[2490865411]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3701531044,829857593),SynapseXen_iIliiIIlIll(698844634,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2905455578,1611857136,1811737207,863066616,651249224,3517295390,2175423480,1480560352,3384969668}return SynapseXen_iiillI[2490865411]end)())do SynapseXen_IlIIii[SynapseXen_IiIilIiIIliiiil-1]=SynapseXen_lIlIIIIiIiIIIIllli()end;return SynapseXen_IiliIIiIillIlll end;do assert(SynapseXen_liiilIiiIlIIiII(4)=="\27Xen","Synapse Xen - Failed to verify bytecode. Please make sure your Lua implementation supports non-null terminated strings.")SynapseXen_iIIliililIIIliiIilll=SynapseXen_iiIiI;SynapseXen_iIiIlIiillilIIlIiIlI=SynapseXen_iiIiI;local SynapseXen_liiIlliIllIIiIIliIll=SynapseXen_liiilIiiIlIIiII()SynapseXen_iiIiI()SynapseXen_iIiiIlilll=SynapseXen_lIiillilIllIlIiIi(SynapseXen_iIIliililIIIliiIilll())SynapseXen_liliIlililI()SynapseXen_liliIlililI()local SynapseXen_IllIiII=0;for SynapseXen_iiIII=1,#SynapseXen_liiIlliIllIIiIIliIll do local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_liiIlliIllIIiIIliIll:sub(SynapseXen_iiIII,SynapseXen_iiIII)SynapseXen_IllIiII=SynapseXen_IllIiII+string.byte(SynapseXen_IIiiiiliiIiiilliiIIl)end;SynapseXen_IllIiII=SynapseXen_iIliiIIlIll(SynapseXen_IllIiII,SynapseXen_iIiiIlilll)for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_liliIlililI()do SynapseXen_ilIiII[SynapseXen_IiIilIiIIliiiil]=SynapseXen_llliIlIliil(SynapseXen_iIIliililIIIliiIilll(),SynapseXen_IllIiII)end;SynapseXen_iiIiIiIiillIill()end;return SynapseXen_lIlIIIIiIiIIIIllli()end;local function SynapseXen_IiiiIliIilliIi(...)return SynapseXen_iIilillll('#',...),{...}end;local function SynapseXen_IiIil(SynapseXen_IiliIIiIillIlll,SynapseXen_iiIiiiIiIiiII,SynapseXen_IlillliIIilIlI)local SynapseXen_iIlIlI=SynapseXen_IiliIIiIillIlll[SynapseXen_iiillI[131045904]or(function(...)local SynapseXen_iiIiIii="epic gamer vision"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3392666309,3418601261)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4041999943,4041977591)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[131045904]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(830323467,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3192302592,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1869655077,4128433151,1744875165,891606754,3839983238,1966702642,1876543644,4056702267,262975930,550332910}return SynapseXen_iiillI[131045904]end)(528,"liliiiIli","liIilIl",{},{},"llIlllilIilIIlIIIiI",{},14877,{})]local SynapseXen_IlIIii=SynapseXen_IiliIIiIillIlll[SynapseXen_iiillI[812408351]or(function()local SynapseXen_iiIiIii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"SynapseXen_iiillI[812408351]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1711067959,397089193),SynapseXen_iIliiIIlIll(2301518975,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3580245669,1294566774,2918343162}return SynapseXen_iiillI[812408351]end)()]local SynapseXen_ilIIlIIii=SynapseXen_IiliIIiIillIlll[SynapseXen_iiillI[1466143109]or(function()local SynapseXen_iiIiIii="luraph better then xen bros :pensive:"SynapseXen_iiillI[1466143109]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3626654986,3155797014),SynapseXen_iIliiIIlIll(3226459732,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{409892361}return SynapseXen_iiillI[1466143109]end)()]return function(...)local SynapseXen_iIIIIIil,SynapseXen_liillliiliiIlI=1,-1;local SynapseXen_lIIiilIIliIIiI,SynapseXen_IIliiIllI={},SynapseXen_iIilillll('#',...)-1;local SynapseXen_liIlIlIiiIIii=0;local SynapseXen_llliiiiliIIIilI={}local SynapseXen_lililili={}local SynapseXen_lllIII=setmetatable({},{__index=SynapseXen_llliiiiliIIIilI,__newindex=function(SynapseXen_IiliIIiiIlIliIIilI,SynapseXen_illlii,SynapseXen_iIIllilIiIlllIi)if SynapseXen_illlii>SynapseXen_liillliiliiIlI then SynapseXen_liillliiliiIlI=SynapseXen_illlii end;SynapseXen_llliiiiliIIIilI[SynapseXen_illlii]=SynapseXen_iIIllilIiIlllIi end})local function SynapseXen_iilIIlliiIiIlIIIl()local SynapseXen_lIIiiI,SynapseXen_IiliiIiilIill;while true do SynapseXen_lIIiiI=SynapseXen_ilIIlIIii[SynapseXen_iIIIIIil]SynapseXen_IiliiIiilIill=SynapseXen_lIIiiI[1706407771]SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1;if SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3071709437]or(function(...)local SynapseXen_iiIiIii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1879732815,2695768623)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2460442841,2460427542)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3071709437]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3564991014,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(308065377,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{687299111,3922015705,3243025195,3469578164}return SynapseXen_iiillI[3071709437]end)({},9348,"llllil",4507,9738,{}))then local SynapseXen_llliIIiiIIlillil=SynapseXen_IlIIii[SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[3864568643]or(function()local SynapseXen_iiIiIii="hi devforum"SynapseXen_iiillI[3864568643]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2627680134,910168214),SynapseXen_iIliiIIlIll(3160119938,SynapseXen_ilIiII[5]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{162748741,1126114575,3131242394}return SynapseXen_iiillI[3864568643]end)()),SynapseXen_liIlIlIiiIIii,262144)]local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_liiIliiIi;local SynapseXen_Iiilli;if SynapseXen_llliIIiiIIlillil[1439547849]~=0 then SynapseXen_liiIliiIi={}SynapseXen_Iiilli=setmetatable({},{__index=function(SynapseXen_IiliIIiiIlIliIIilI,SynapseXen_illlii)local SynapseXen_lililii=SynapseXen_liiIliiIi[SynapseXen_illlii]return SynapseXen_lililii[1][SynapseXen_lililii[2]]end,__newindex=function(SynapseXen_IiliIIiiIlIliIIilI,SynapseXen_illlii,SynapseXen_iIIllilIiIlllIi)local SynapseXen_lililii=SynapseXen_liiIliiIi[SynapseXen_illlii]SynapseXen_lililii[1][SynapseXen_lililii[2]]=SynapseXen_iIIllilIiIlllIi end})for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_llliIIiiIIlillil[1439547849]do local SynapseXen_iiiIlililllIiI=SynapseXen_ilIIlIIii[SynapseXen_iIIIIIil]if SynapseXen_iiiIlililllIiI[1706407771]==(SynapseXen_iiillI[3924551813]or(function(...)local SynapseXen_iiIiIii="thats how mafia works"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2639108146,102256292)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1162233170,1162168563)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3924551813]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3488895578,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4105025492,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2272696942,2414383100,3308296043,879350472,1812099961,3024991673,594453474}return SynapseXen_iiillI[3924551813]end)("IiIilillIiiiIi",7520,{},{},2075,51,{}))then SynapseXen_liiIliiIi[SynapseXen_IiIilIiIIliiiil-1]={SynapseXen_iIiiiliIliiilillIiil,SynapseXen_iIliiIIlIll(SynapseXen_iiiIlililllIiI[1776583566],SynapseXen_iiillI[2713915352]or(function()local SynapseXen_iiIiIii="level 1 crook = luraph, level 100 boss = xen"SynapseXen_iiillI[2713915352]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1935848861,996474523),SynapseXen_iIliiIIlIll(3892909633,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1504990821,2776128643,3830817412,1828124642,3074075767,1477313328}return SynapseXen_iiillI[2713915352]end)())}elseif SynapseXen_iiiIlililllIiI[1706407771]==(SynapseXen_iiillI[2170534547]or(function(...)local SynapseXen_iiIiIii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4043407539,3073992627)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1826664715,1826654687)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2170534547]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1631430658,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2265633584,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2814851825,395587855,4014458558,3802777619,302268462,4126599721,1542785523,1082302251,465744772}return SynapseXen_iiillI[2170534547]end)("lillliiii",{},"iIllIIlililIll",94,{}))then SynapseXen_liiIliiIi[SynapseXen_IiIilIiIIliiiil-1]={SynapseXen_IlillliIIilIlI,SynapseXen_iIliiIIlIll(SynapseXen_iiiIlililllIiI[1776583566],SynapseXen_iiillI[3940153553]or(function(...)local SynapseXen_iiIiIii="wally bad bird"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4228023184,3804441800)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2218361632,2218331965)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3940153553]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1000620983,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3398672238,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{743563035,1135684796,2689617097,3226804354,359147017,725344893,3012932395,762316959,2119702824,3421474690}return SynapseXen_iiillI[3940153553]end)("ll",{},"iillI",{},10208,8099,13659,"Iii"))}end;SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end;SynapseXen_lililili[#SynapseXen_lililili+1]=SynapseXen_liiIliiIi end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_iIIlIlliIliI(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3567824177]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[3567824177]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1277607718,3257992792),SynapseXen_iIliiIIlIll(773391848,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2437279454,3833346366,1581102695,4237759077,609008905,1024450383,3627451979}return SynapseXen_iiillI[3567824177]end)(),256),SynapseXen_liIlIlIiiIIii,256)]=SynapseXen_IiIil(SynapseXen_llliIIiiIIlillil,SynapseXen_iiIiiiIiIiiII,SynapseXen_Iiilli)elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4277787799]or(function(...)local SynapseXen_iiIiIii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2216405360,1714962303)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1239454636,1239421640)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[4277787799]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1981100854,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2856397653,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{2547367657,2259056134,788660367,2180830570,934876702,1874791373,488570282}return SynapseXen_iiillI[4277787799]end)(8301,"lIllli",9707,1506))then local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;for SynapseXen_IiIilIiIIliiiil=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[395775834]or(function()local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"SynapseXen_iiillI[395775834]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1508456344,1946128828),SynapseXen_iIliiIIlIll(3989555808,SynapseXen_ilIiII[2]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2114741148,2054019007,490976745}return SynapseXen_iiillI[395775834]end)(),256),SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1633799239]or(function()local SynapseXen_iiIiIii="inb4 posted on exploit reports section"SynapseXen_iiillI[1633799239]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(884215750,2962320272),SynapseXen_iIliiIIlIll(1809297526,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2211341567,3479446821}return SynapseXen_iiillI[1633799239]end)())do SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]=nil end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[476657322]or(function()local SynapseXen_iiIiIii="xen doesn't come with instance caching, sorry superskater"SynapseXen_iiillI[476657322]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1160300920,4123288474),SynapseXen_iIliiIIlIll(2791952977,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{60634352,199508523,2561786432,2079594876,1614968299}return SynapseXen_iiillI[476657322]end)())then SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2001466870]or(function()local SynapseXen_iiIiIii="yed"SynapseXen_iiillI[2001466870]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3224152034,199961765),SynapseXen_iIliiIIlIll(1808335509,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{680976048,327929929,730937889,2376795867}return SynapseXen_iiillI[2001466870]end)())]=SynapseXen_IlillliIIilIlI[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[4290166869]or(function()local SynapseXen_iiIiIii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iiillI[4290166869]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2430942341,3625995298),SynapseXen_iIliiIIlIll(2805083801,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{690202058,4133128207,442959209,1468973982,861222814,3206081299}return SynapseXen_iiillI[4290166869]end)())]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[255310135]or(function()local SynapseXen_iiIiIii="wally bad bird"SynapseXen_iiillI[255310135]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3009130478,2445699609),SynapseXen_iIliiIIlIll(3446382021,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{511639632}return SynapseXen_iiillI[255310135]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3883054613]or(function()local SynapseXen_iiIiIii="luraph better then xen bros :pensive:"SynapseXen_iiillI[3883054613]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1360040860,466354471),SynapseXen_iIliiIIlIll(3939758948,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1922104665,4010753923,4140700392,2564821833,1688896872,2069209079,380720775}return SynapseXen_iiillI[3883054613]end)())~=0;local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3105950297]or(function()local SynapseXen_iiIiIii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."SynapseXen_iiillI[3105950297]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3743309270,2696069433),SynapseXen_iIliiIIlIll(1709373929,SynapseXen_ilIiII[3]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{571311469,3147980283,2891935619,1547756409,3039560557,3584974175,1890147927,1817347078,128486414}return SynapseXen_iiillI[3105950297]end)(),512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[3672927217]or(function()local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"SynapseXen_iiillI[3672927217]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2848372068,958711215),SynapseXen_iIliiIIlIll(820826264,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3213293524,2809453720,2795081808,4224871332,3555250606,1550132417,3039565785,619092059,822365415}return SynapseXen_iiillI[3672927217]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;if SynapseXen_ilillliilIiiiIilIi<=SynapseXen_IIiiiiliiIiiilliiIIl~=SynapseXen_llIilIiliIliIlIlIilI then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2839974550]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[2839974550]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3908718566,79666913),SynapseXen_iIliiIIlIll(3526493215,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{4221417760,4185735936}return SynapseXen_iiillI[2839974550]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3961292529]or(function(...)local SynapseXen_iiIiIii="wait for someone on devforum to say they are gonna deobfuscate this"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1897606377,38902487)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2030596143,2030578314)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3961292529]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(4224209802,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3069531137,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{1056014072,2258755857,2251840010,1871662578,1580312633,3755072942,4204653050,230792271,4209964791,1879195721}return SynapseXen_iiillI[3961292529]end)(2158,3939,"lIIlii"),256)local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3256916275]or(function()local SynapseXen_iiIiIii="epic gamer vision"SynapseXen_iiillI[3256916275]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(366697443,2598462255),SynapseXen_iIliiIIlIll(2974651803,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{4204276695,2055680291,3006686031,2439218911,3397610619,2908790770,4264677530}return SynapseXen_iiillI[3256916275]end)()),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[204090093]or(function(...)local SynapseXen_iiIiIii="this is so sad, alexa play ripull.mp4"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2235143044,340616101)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(809684207,809674932)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[204090093]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(731634151,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(451367243,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3533154596,732578060,1899128746,1887455632,4011989369}return SynapseXen_iiillI[204090093]end)(14246,{},"ililI",{},6786,2246,10682,"IIIi"),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_liIIIli,SynapseXen_IiiilililiIilI;local SynapseXen_IiIiIIiIl,SynapseXen_IlIiIIllIiiiiiIlil;SynapseXen_liIIIli={}if SynapseXen_ilillliilIiiiIilIi~=1 then if SynapseXen_ilillliilIiiiIilIi~=0 then SynapseXen_IiIiIIiIl=SynapseXen_llIilIiliIliIlIlIilI+SynapseXen_ilillliilIiiiIilIi-1 else SynapseXen_IiIiIIiIl=SynapseXen_liillliiliiIlI end;SynapseXen_IlIiIIllIiiiiiIlil=0;for SynapseXen_IiIilIiIIliiiil=SynapseXen_llIilIiliIliIlIlIilI+1,SynapseXen_IiIiIIiIl do SynapseXen_IlIiIIllIiiiiiIlil=SynapseXen_IlIiIIllIiiiiiIlil+1;SynapseXen_liIIIli[SynapseXen_IlIiIIllIiiiiiIlil]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]end;SynapseXen_IiIiIIiIl,SynapseXen_IiiilililiIilI=SynapseXen_IiiiIliIilliIi(SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI](unpack(SynapseXen_liIIIli,1,SynapseXen_IiIiIIiIl-SynapseXen_llIilIiliIliIlIlIilI)))else SynapseXen_IiIiIIiIl,SynapseXen_IiiilililiIilI=SynapseXen_IiiiIliIilliIi(SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]())end;SynapseXen_liillliiliiIlI=SynapseXen_llIilIiliIliIlIlIilI-1;if SynapseXen_IIiiiiliiIiiilliiIIl~=1 then if SynapseXen_IIiiiiliiIiiilliiIIl~=0 then SynapseXen_IiIiIIiIl=SynapseXen_llIilIiliIliIlIlIilI+SynapseXen_IIiiiiliiIiiilliiIIl-2 else SynapseXen_IiIiIIiIl=SynapseXen_IiIiIIiIl+SynapseXen_llIilIiliIliIlIlIilI-1 end;SynapseXen_IlIiIIllIiiiiiIlil=0;for SynapseXen_IiIilIiIIliiiil=SynapseXen_llIilIiliIliIlIlIilI,SynapseXen_IiIiIIiIl do SynapseXen_IlIiIIllIiiiiiIlil=SynapseXen_IlIiIIllIiiiiiIlil+1;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]=SynapseXen_IiiilililiIilI[SynapseXen_IlIiIIllIiiiiiIlil]end end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3221066628]or(function()local SynapseXen_iiIiIii="hi devforum"SynapseXen_iiillI[3221066628]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(180061699,2898377014),SynapseXen_iIliiIIlIll(108466066,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1950764984,3931733683,3690117268,2001809407}return SynapseXen_iiillI[3221066628]end)())then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1782173143]or(function()local SynapseXen_iiIiIii="this is a christian obfuscator, no cursing allowed in our scripts"SynapseXen_iiillI[1782173143]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3060250761,3536921255),SynapseXen_iIliiIIlIll(3300551650,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{509173107,2827625284,441351626,2812348561,3414465214,1861267364,2253881613,1861579331,2964557516,1627693826}return SynapseXen_iiillI[1782173143]end)())]if not not SynapseXen_ilillliilIiiiIilIi==(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2773167614]or(function(...)local SynapseXen_iiIiIii="wow xen is shit buy luraph ok"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1353344602,2373230988)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(604464075,604445734)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2773167614]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1613750726,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2049764617,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{2957851808,3106954207}return SynapseXen_iiillI[2773167614]end)(12606,"li",9422,4251),512)==0)then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 else SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3468169437]or(function()local SynapseXen_iiIiIii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"SynapseXen_iiillI[3468169437]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2019966345,2926160522),SynapseXen_iIliiIIlIll(972933605,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3130520831,2102957394,859191235}return SynapseXen_iiillI[3468169437]end)(),256)]=SynapseXen_ilillliilIiiiIilIi end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4161968624]or(function()local SynapseXen_iiIiIii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iiillI[4161968624]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(986590099,4024993717),SynapseXen_iIliiIIlIll(1965758978,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1473719407,2550826392,1731009665}return SynapseXen_iiillI[4161968624]end)())then local SynapseXen_ilillliilIiiiIilIi,SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[2840184292]or(function(...)local SynapseXen_iiIiIii="xen doesn't come with instance caching, sorry superskater"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(265699309,71398524)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(247907809,247845113)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2840184292]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(814460654,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2602036463,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2768066273,2717797004}return SynapseXen_iiillI[2840184292]end)("lIllili",1447,{},11165,6742,"ilIIiiiiIIlilIliil"),512),SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2146908218]or(function(...)local SynapseXen_iiIiIii="thats how mafia works"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2765691066,2078767209)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1856426522,1856360149)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2146908218]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2408273491,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3208792498,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{4112560539,3837241005,424348023,2133032668}return SynapseXen_iiillI[2146908218]end)("IIIiliIilIil",11268,{},{}),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[169638921]or(function(...)local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4271135191,60379282)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(792333817,792298298)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[169638921]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2615424140,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3337047162,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2802772401,4184310743,4075775832,4135651749,1387203329,168001471,2797411037,306758157}return SynapseXen_iiillI[169638921]end)({},4238,"IiiIlililIIlIiII","I","IilIlllIiiill",2052,8497,{},3550),256)][SynapseXen_ilillliilIiiiIilIi]=SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1121274627]or(function()local SynapseXen_iiIiIii="xen detects custom getfenv"SynapseXen_iiillI[1121274627]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(235987472,458630515),SynapseXen_iIliiIIlIll(63164378,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{1381722638,874635041}return SynapseXen_iiillI[1121274627]end)())then SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1464823881]or(function()local SynapseXen_iiIiIii="yed"SynapseXen_iiillI[1464823881]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3582666019,3604322896),SynapseXen_iIliiIIlIll(3970486532,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2181211181,1967312540,3142308974,3378168063,2332715830,3408327218,2582786413,3961711161,3139335498}return SynapseXen_iiillI[1464823881]end)(),256)]=not SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1959455944]or(function()local SynapseXen_iiIiIii="sometimes it be like that"SynapseXen_iiillI[1959455944]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3005033855,4280200902),SynapseXen_iIliiIIlIll(1920337837,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{60419692}return SynapseXen_iiillI[1959455944]end)(),512)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[432913551]or(function(...)local SynapseXen_iiIiIii="so if you'we nyot awawe of expwoiting by this point, you've pwobabwy been wiving undew a wock that the pionyeews used to wide fow miwes. wobwox is often seen as an expwoit-infested gwound by most fwom the suwface, awthough this isn't the case."local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3070288999,1272491440)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3742497845,3742470585)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[432913551]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3198976258,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3799723767,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1964733585,581283120,1575245185,3899801481,994018583,2404956740,1409123091,2939040294,427832539,3276399352}return SynapseXen_iiillI[432913551]end)("iliIIl",{},{},859))then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_IllillliiIl(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3862346953]or(function()local SynapseXen_iiIiIii="aspect network better obfuscator"SynapseXen_iiillI[3862346953]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3384460748,1326170616),SynapseXen_iIliiIIlIll(2633520058,SynapseXen_ilIiII[3]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{1105786088,1385245514,1243645986,2930830569,1234174707,4218576568}return SynapseXen_iiillI[3862346953]end)(),256),SynapseXen_liIlIlIiiIIii,256)local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[2521598657]or(function()local SynapseXen_iiIiIii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"SynapseXen_iiillI[2521598657]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(418412961,2402193453),SynapseXen_iIliiIIlIll(1342406939,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{3109381377,813059834,1818628130}return SynapseXen_iiillI[2521598657]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_liIIIli,SynapseXen_IiiilililiIilI;local SynapseXen_IiIiIIiIl;local SynapseXen_liIIllIiIiiiiliiIIIi=0;SynapseXen_liIIIli={}if SynapseXen_ilillliilIiiiIilIi~=1 then if SynapseXen_ilillliilIiiiIilIi~=0 then SynapseXen_IiIiIIiIl=SynapseXen_llIilIiliIliIlIlIilI+SynapseXen_ilillliilIiiiIilIi-1 else SynapseXen_IiIiIIiIl=SynapseXen_liillliiliiIlI end;for SynapseXen_IiIilIiIIliiiil=SynapseXen_llIilIiliIliIlIlIilI+1,SynapseXen_IiIiIIiIl do SynapseXen_liIIIli[#SynapseXen_liIIIli+1]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]end;SynapseXen_IiiilililiIilI={SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI](unpack(SynapseXen_liIIIli,1,SynapseXen_IiIiIIiIl-SynapseXen_llIilIiliIliIlIlIilI))}else SynapseXen_IiiilililiIilI={SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]()}end;for SynapseXen_lIilllIIiIiI in next,SynapseXen_IiiilililiIilI do if SynapseXen_lIilllIIiIiI>SynapseXen_liIIllIiIiiiiliiIIIi then SynapseXen_liIIllIiIiiiiliiIIIi=SynapseXen_lIilllIIiIiI end end;return SynapseXen_IiiilililiIilI,SynapseXen_liIIllIiIiiiiliiIIIi elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3827310177]or(function(...)local SynapseXen_iiIiIii="wow xen is shit buy luraph ok"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1414022548,1550970984)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1396866983,1396846365)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3827310177]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(736409621,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(894729479,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{2749063645,1812055426,2008585583,2557522200,433471774,3844801202,3386351200,1460200918,625736402,1413581976}return SynapseXen_iiillI[3827310177]end)("IIliIlIlIilIilI","IiliIl",{},{}))then local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[107400014]or(function(...)local SynapseXen_iiIiIii="hi xen crashes on my axon paste plz help"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1294240106,1250636006)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1970748831,1970685741)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[107400014]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(4203000492,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1562738808,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{1053023844,2575424828,366403642,2185006690,3425286886,722919186,61350507,757870984}return SynapseXen_iiillI[107400014]end)("lliIIlIIiiii","IilIlIiiillliiii",{},"IIliIlililIIiliII"))local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1917607002]or(function(...)local SynapseXen_iiIiIii="this is so sad, alexa play ripull.mp4"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2039723776,3051504458)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2117975548,2117923843)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1917607002]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(98054185,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1772512169,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2859790200,127128362,2853583570,2211506723,1572984241,3397846903,360151647,1249100662}return SynapseXen_iiillI[1917607002]end)("liIIIIillliiliI",{},{},"lIIIIIiI"))]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1709162624]or(function(...)local SynapseXen_iiIiIii="epic gamer vision"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1921832037,2923504699)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3250406875,3250330692)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1709162624]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3842305794,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(596099257,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{2753195081,3496821202}return SynapseXen_iiillI[1709162624]end)("iliIlIl",{},12722,217,{},"IllIiIIIiIIIIlIIlII","lIIlil"),512)][SynapseXen_IIiiiiliiIiiilliiIIl]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2319209329]or(function()local SynapseXen_iiIiIii="xen detects custom getfenv"SynapseXen_iiillI[2319209329]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1992585424,3596131714),SynapseXen_iIliiIIlIll(9665341,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1036574396,1389437886,2751537117,3480999248,3824623427,817654807,478172052}return SynapseXen_iiillI[2319209329]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3505668535]or(function(...)local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2739973019,3146768270)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1664954833,1664892535)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3505668535]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1459388667,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3998017443,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{1876804608,3102283940,2150361585,2949610941,79280661,2726719197,433563604,3343591127}return SynapseXen_iiillI[3505668535]end)("iIIIlIiiIllIlii",10656,"IiilIllIiiiiiili","lliiliIilIIiiI",1544,"lIl",{},12172,"IiIlIlIIiiIIIl","llIiIIlII"),256)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_lIlilli=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2]local SynapseXen_lIilllIIiIiI=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]+SynapseXen_lIlilli;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]=SynapseXen_lIilllIIiIiI;if SynapseXen_lIlilli>0 then if SynapseXen_lIilllIIiIiI<=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1]then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+SynapseXen_lIIiiI[1300471799]SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+3]=SynapseXen_lIilllIIiIiI end else if SynapseXen_lIilllIIiIiI>=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1]then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+SynapseXen_lIIiiI[1300471799]SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+3]=SynapseXen_lIilllIIiIiI end end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[476324068]or(function(...)local SynapseXen_iiIiIii="xen doesn't come with instance caching, sorry superskater"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1413952803,4183025185)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4029561439,4029503057)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[476324068]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(293399219,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2182726241,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{2163760329,3447613348,2250228293,3945444883,2040330979,2539638306}return SynapseXen_iiillI[476324068]end)(14637,1427))then SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3258015173]or(function()local SynapseXen_iiIiIii="hi my 2.5mb script doesn't work with xen please help"SynapseXen_iiillI[3258015173]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(295446311,1765992790),SynapseXen_iIliiIIlIll(3204820319,SynapseXen_ilIiII[2]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2411035141,500677904}return SynapseXen_iiillI[3258015173]end)())]=SynapseXen_iIlIlI[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[2638979619]or(function(...)local SynapseXen_iiIiIii="hi devforum"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2327524895,703856451)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2132278295,2132203758)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2638979619]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(568021252,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1835056739,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{230429152,3218800803,2273929618,3925415197,2433534848,3735784616,2764817081,1680613613}return SynapseXen_iiillI[2638979619]end)("lllliIilI",7274,{},4461,{},{}),262144)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[288666458]or(function(...)local SynapseXen_iiIiIii="hi xen doesn't work on sk8r please help"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3784404078,941129780)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(328699538,328672651)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[288666458]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1666493665,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2694041051,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{3600088272,647909323,299478174,449998124,3235546213,1040558404,2185585141,2875606273,1112137983,4285093770}return SynapseXen_iiillI[288666458]end)("IIliiiiil",5165,6348,"llllIIIlIIii",{},7046,2509))then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[2950049778]or(function(...)local SynapseXen_iiIiIii="print(bytecode)"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1480736606,2313495674)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1066226578,1066160552)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2950049778]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1323328683,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2232086337,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{176315464,3094003308,1711876469,3898505956,1663293687,3841272129}return SynapseXen_iiillI[2950049778]end)("lIilIIIIiIIliiI",1389,{},{},"llIIiiiilliIiilI",7390,7263,13111,"IIlllliIIiiiIiil")),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[97139802]or(function()local SynapseXen_iiIiIii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."SynapseXen_iiillI[97139802]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3347921689,153699562),SynapseXen_iIliiIIlIll(1856686075,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2574120992,945018062,1609563420,2728371941}return SynapseXen_iiillI[97139802]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3783649922]or(function(...)local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1292577450,343330821)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1937252346,1937202227)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3783649922]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1790476307,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2479497903,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1754554853,3147060975,3757354632}return SynapseXen_iiillI[3783649922]end)("liilllIlillIilliII","Ii","Ill","IIliiiIl"),256),SynapseXen_liIlIlIiiIIii,256)]=SynapseXen_ilillliilIiiiIilIi/SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4085934279]or(function()local SynapseXen_iiIiIii="can we have an f in chat for ripull"SynapseXen_iiillI[4085934279]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2342222307,3373025634),SynapseXen_iIliiIIlIll(2236235776,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{4265368250,3220496463,2287199201,2833414719,1008033618,2231894221,21884015,2824050691,2076891438,1367350573}return SynapseXen_iiillI[4085934279]end)())then SynapseXen_iiIiiiIiIiiII[SynapseXen_iIlIlI[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[2560508249]or(function()local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"SynapseXen_iiillI[2560508249]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2836562170,1961771604),SynapseXen_iIliiIIlIll(2113000597,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1573325709,2114474043,1929500055,2396898131,3052615707}return SynapseXen_iiillI[2560508249]end)(),262144)]]=SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3706382537]or(function()local SynapseXen_iiIiIii="SECURE API, IMPOSSIBLE TO BYPASS!"SynapseXen_iiillI[3706382537]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4027020950,1961873316),SynapseXen_iIliiIIlIll(620324372,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3855619408,3575521585,4216342778,2514430269,684133632,4219673887,3215690333,1107339679,2316759633}return SynapseXen_iiillI[3706382537]end)(),256)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4053594355]or(function()local SynapseXen_iiIiIii="hi devforum"SynapseXen_iiillI[4053594355]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1295939316,2372293021),SynapseXen_iIliiIIlIll(1616361974,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{783242166,2479831747,3355000245,3182772699,1171454734,317285285}return SynapseXen_iiillI[4053594355]end)())then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[2366015116]or(function()local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"SynapseXen_iiillI[2366015116]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2008308397,24413616),SynapseXen_iIliiIIlIll(3603934369,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{4201804772,3392535797,2725525154,2246041244,1387505274,1569702690,3840377841}return SynapseXen_iiillI[2366015116]end)(),512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[429438389]or(function(...)local SynapseXen_iiIiIii="this is a christian obfuscator, no cursing allowed in our scripts"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(194804096,1089779253)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1881514986,1881506922)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[429438389]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(520080071,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4120504803,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2508409359,2381915141,1938576837,2311404882,451363330,3935719963,2441579387,1591558545,3267935518,2091667954}return SynapseXen_iiillI[429438389]end)({},{},12445,{},{},"lIiililIlliliIIl","IIIl",{},4438,13593)),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3570070753]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[3570070753]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1620013742,1138973156),SynapseXen_iIliiIIlIll(2204350949,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{4211529636,3707489824,3157260188,3348384438,1461917610}return SynapseXen_iiillI[3570070753]end)(),256)]=SynapseXen_ilillliilIiiiIilIi%SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2773135231]or(function()local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"SynapseXen_iiillI[2773135231]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2371055296,446487537),SynapseXen_iIliiIIlIll(2169026965,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{529428117,808173431,1660741767,4074846874,134924896,1258929092}return SynapseXen_iiillI[2773135231]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_IllillliiIl(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[661910164]or(function()local SynapseXen_iiIiIii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iiillI[661910164]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2029242804,194959035),SynapseXen_iIliiIIlIll(1709785169,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{3612275218,2621320939,3125429826,3981792373,1581263933,745007138,2780674453}return SynapseXen_iiillI[661910164]end)(),256),SynapseXen_liIlIlIiiIIii,256)local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3619910615]or(function()local SynapseXen_iiIiIii="now comes with a free n word pass"SynapseXen_iiillI[3619910615]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2636527193,1367264658),SynapseXen_iIliiIIlIll(3672056400,SynapseXen_ilIiII[5]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{885949748}return SynapseXen_iiillI[3619910615]end)())local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2893823347]or(function()local SynapseXen_iiIiIii="wally bad bird"SynapseXen_iiillI[2893823347]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2188455401,1711412538),SynapseXen_iIliiIIlIll(1149102434,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{390039603,4286260091,161083166,499665687,3998339961,833234176,2845597133,517134186,3949359496}return SynapseXen_iiillI[2893823347]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1]=SynapseXen_ilillliilIiiiIilIi;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]=SynapseXen_ilillliilIiiiIilIi[SynapseXen_IIiiiiliiIiiilliiIIl]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1927636926]or(function(...)local SynapseXen_iiIiIii="hi xen crashes on my axon paste plz help"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2505250474,2396736919)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1746280997,1746227070)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1927636926]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3828600371,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3251393713,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{3709639870,3103158185,2371444395,580522938,166018716,2227764928,2716887120}return SynapseXen_iiillI[1927636926]end)(6098,14040,{},"iIl",225,"II",5657,"ililIIlIiI",{},{}))then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIIlIlliIliI(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1673133796]or(function()local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"SynapseXen_iiillI[1673133796]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2303951998,3940562754),SynapseXen_iIliiIIlIll(3283932128,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{94798182,3018957619,797063715,399067228,3963863945,3685916799,2710942281,2632112872,3651600381,2989897220}return SynapseXen_iiillI[1673133796]end)(),256),SynapseXen_liIlIlIiiIIii,256)local SynapseXen_ilillliilIiiiIilIi=SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1881376197]or(function(...)local SynapseXen_iiIiIii="now with shitty xor string obfuscation"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(181183196,435587216)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(110021503,109953191)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1881376197]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2097034100,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3486026638,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3455162461,3222523214,1880798228,865768533,1593356924,1023038380,3459368799,2749352720,2320918699,3451564208}return SynapseXen_iiillI[1881376197]end)(13743,{},195,{},"IillIii",{},10018,"iliIillll"),512)local SynapseXen_iIiiiliIliiilillIiil,SynapseXen_IIlIiill=SynapseXen_lllIII,SynapseXen_lIIiilIIliIIiI;SynapseXen_liillliiliiIlI=SynapseXen_llIilIiliIliIlIlIilI-1;for SynapseXen_IiIilIiIIliiiil=SynapseXen_llIilIiliIliIlIlIilI,SynapseXen_llIilIiliIliIlIlIilI+(SynapseXen_ilillliilIiiiIilIi>0 and SynapseXen_ilillliilIiiiIilIi-1 or SynapseXen_IIliiIllI)do SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]=SynapseXen_IIlIiill[SynapseXen_IiIilIiIIliiiil-SynapseXen_llIilIiliIliIlIlIilI]end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4188798486]or(function(...)local SynapseXen_iiIiIii="this is so sad, alexa play ripull.mp4"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2155058643,3631583015)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4025055414,4025035459)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[4188798486]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3260803448,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2754275281,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{1826318166}return SynapseXen_iiillI[4188798486]end)({},"lllIllili","iIiillI",11135,{}))then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+SynapseXen_lIIiiI[1300471799]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4044503544]or(function()local SynapseXen_iiIiIii="wow xen is shit buy luraph ok"SynapseXen_iiillI[4044503544]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3039917541,3207871366),SynapseXen_iIliiIIlIll(879920561,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{4071595913,4139668074,506526192,1995122912,3060495312,3674601260,3652583638,3837169988,3490858892}return SynapseXen_iiillI[4044503544]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2545379456]or(function(...)local SynapseXen_iiIiIii="level 1 crook = luraph, level 100 boss = xen"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3678263227,2157058172)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(829837972,829765762)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2545379456]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(293307058,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3938737424,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1327942600,2177670977,3281174567,1187644053,2433063578,1821218727,3949394554,2592941751,1053190118,1008831952}return SynapseXen_iiillI[2545379456]end)(1974,"liIIlIIliliIlIili",{},9953,"liilililllliIlIIi","ilillliiIllIIill",229,9348),256)~=0;local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3234693360]or(function()local SynapseXen_iiIiIii="epic gamer vision"SynapseXen_iiillI[3234693360]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1607820461,4107751066),SynapseXen_iIliiIIlIll(185441145,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3064936283,3637343301,3326515635,3573910845,504533728,4182809876,4018684808,851164920,3650489437,3783961319}return SynapseXen_iiillI[3234693360]end)())local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2017280448]or(function()local SynapseXen_iiIiIii="wally bad bird"SynapseXen_iiillI[2017280448]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(668140525,92487239),SynapseXen_iIliiIIlIll(3450262356,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{1629882875,2410226330,3403391147,2051695236,1210877939,2059341013,2900703692}return SynapseXen_iiillI[2017280448]end)()),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;if SynapseXen_ilillliilIiiiIilIi==SynapseXen_IIiiiiliiIiiilliiIIl~=SynapseXen_llIilIiliIliIlIlIilI then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1779886182]or(function(...)local SynapseXen_iiIiIii="SECURE API, IMPOSSIBLE TO BYPASS!"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3315164,3693935924)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3496361228,3496308822)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1779886182]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(677378854,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(865873423,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{2727879595,1359290799,3016954614,3817603310,3976740282,1535982951,4024980928}return SynapseXen_iiillI[1779886182]end)(12537))then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[702731916]or(function()local SynapseXen_iiIiIii="what are you trying to say? that fucking one dot + dot + dot + many dots is not adding adding 1 dot + dot and then adding all the dots together????"SynapseXen_iiillI[702731916]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(862672956,1583622842),SynapseXen_iIliiIIlIll(2072673108,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{1807215737,3387059338,3855972905,3372491691,2408515406,2013006729,3311722192,3680764908}return SynapseXen_iiillI[702731916]end)(),256)local SynapseXen_ilillliilIiiiIilIi=SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1576999008]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[1576999008]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(584451996,1814956580),SynapseXen_iIliiIIlIll(1888241349,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{3274789641,2101429048,1397109319,1160326423,3409316228,824608587}return SynapseXen_iiillI[1576999008]end)(),512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_IllillliiIl(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[294049613]or(function()local SynapseXen_iiIiIii="thats how mafia works"SynapseXen_iiillI[294049613]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4223087816,1662801531),SynapseXen_iIliiIIlIll(2196223627,SynapseXen_ilIiII[3]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2263099022,394037066,422933820,1441048248,3466223241,1328974097,2652349526,2708806853,3963100897,1062246783}return SynapseXen_iiillI[294049613]end)()),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_IIiiiiliiIiiilliiIIl==0 then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1;SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_ilIIlIIii[SynapseXen_iIIIIIil][103297735]end;local SynapseXen_lilliiIii=(SynapseXen_IIiiiiliiIiiilliiIIl-1)*50;local SynapseXen_IlliliilliiIll=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]if SynapseXen_ilillliilIiiiIilIi==0 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_liillliiliiIlI-SynapseXen_llIilIiliIliIlIlIilI end;for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_ilillliilIiiiIilIi do SynapseXen_IlliliilliiIll[SynapseXen_lilliiIii+SynapseXen_IiIilIiIIliiiil]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+SynapseXen_IiIilIiIIliiiil]end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[4267294943]or(function(...)local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2621449952,2244521439)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2487363844,2487305111)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[4267294943]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2200750040,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(987074406,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{108552673,1091294048,1943879400,3009440132,424898006,2816772858,3579398873,2300250723,1552429868}return SynapseXen_iiillI[4267294943]end)("llliiIIIlllliili",{},"IIilIiiiIlllIil",{},{},"iIlIIil",{},10503,"IiIlii",{}))then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[70359698]or(function(...)local SynapseXen_iiIiIii="now with shitty xor string obfuscation"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4182611046,3315260932)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2029150839,2029137786)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[70359698]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3967919509,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3995130492,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{1969163821,3972876468,1033382713,270292668}return SynapseXen_iiillI[70359698]end)({},{},"IIiIlIili",{},2963,"ilIillIIlIil",{},14101,"IiiIliIlIIiiIiIiii"))local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[34178209]or(function(...)local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1795540782,2863900095)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1562119481,1562084206)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[34178209]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(974939470,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3789823726,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{52450277,493607153,2943977461,1671709562,839697755,387976177,2976243909}return SynapseXen_iiillI[34178209]end)(3230),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[145078663]or(function()local SynapseXen_iiIiIii="yed"SynapseXen_iiillI[145078663]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1646120417,3833506461),SynapseXen_iIliiIIlIll(2431019042,SynapseXen_ilIiII[5]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{1922154015,4166465009,637841536,1849650275,660377455,3923409222,2749649427,3182611336,2484919970,1276376500}return SynapseXen_iiillI[145078663]end)())]=SynapseXen_ilillliilIiiiIilIi-SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3985708334]or(function(...)local SynapseXen_iiIiIii="this is a christian obfuscator, no cursing allowed in our scripts"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2981274664,2209266766)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2940555845,2940493740)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3985708334]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2822198292,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(975560042,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{4105770804,733166521}return SynapseXen_iiillI[3985708334]end)("I","llilll",4715,"illI",13726,{},647,{},"lIilliIil"))then SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1420749403]or(function(...)local SynapseXen_iiIiIii="this is so sad, alexa play ripull.mp4"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(868699023,3724465252)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(720141962,720122178)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1420749403]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2359526913,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3264603352,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3849673945}return SynapseXen_iiillI[1420749403]end)({},{}))]=SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3007678761]or(function()local SynapseXen_iiIiIii="hi xen doesn't work on sk8r please help"SynapseXen_iiillI[3007678761]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(252457413,700512362),SynapseXen_iIliiIIlIll(1015287110,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{4190031040}return SynapseXen_iiillI[3007678761]end)())]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[595945334]or(function(...)local SynapseXen_iiIiIii="xen doesn't come with instance caching, sorry superskater"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1345892434,3170556105)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2071945684,2071938165)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[595945334]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1756073644,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(610783096,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{4189651224,3550658240,349699690,3703376842,3907722195,426339145,591862685,1281038990}return SynapseXen_iiillI[595945334]end)(3638,7169,"iliiIlilIilll",2343,{}))then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[779852038]or(function(...)local SynapseXen_iiIiIii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(676959883,2133570094)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1419772193,1419742614)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[779852038]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(122075997,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1849912236,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{4147236334,3179347518,1830122480,45617090,111844990,2931599705,3139477483,511759839,2760653489}return SynapseXen_iiillI[779852038]end)({},"lillll",{},"IilIiIiIIIl","i"))local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2315318019]or(function(...)local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1739991325,3968790044)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(189385682,189323152)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2315318019]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2445400522,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4111315882,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3572950279,272128594,536579826,2685754759,1867513890}return SynapseXen_iiillI[2315318019]end)({},{},"iiIlilIiIIliIiIIIl","lilliiiiiiliIIl"),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_lilliiIii=SynapseXen_llIilIiliIliIlIlIilI+2;local SynapseXen_lililiIllIlliiliIl={SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI](SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1],SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2])}for SynapseXen_IiIilIiIIliiiil=1,SynapseXen_IIiiiiliiIiiilliiIIl do SynapseXen_lllIII[SynapseXen_lilliiIii+SynapseXen_IiIilIiIIliiiil]=SynapseXen_lililiIllIlliiliIl[SynapseXen_IiIilIiIIliiiil]end;if SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+3]~=nil then SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+3]else SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[200354322]or(function()local SynapseXen_iiIiIii="pain exist is gonna connect the dots of xen"SynapseXen_iiillI[200354322]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4042537495,2541635799),SynapseXen_iIliiIIlIll(3347247999,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{4121326370,3271428500,2034202840,3929932350,2837973859,2570858533,649931946,1338764274,4119422513,2891533942}return SynapseXen_iiillI[200354322]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2588829005]or(function(...)local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(552676273,2542225395)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3221169076,3221114120)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2588829005]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3645463779,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3459351463,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{364894605,2014849185,2476674440,3731882346,330687643,382547188,2849368602,924092738}return SynapseXen_iiillI[2588829005]end)("i",1099))local SynapseXen_iliIIi={}for SynapseXen_IiIilIiIIliiiil=1,#SynapseXen_lililili do local SynapseXen_IiIiiilliiIiIIlii=SynapseXen_lililili[SynapseXen_IiIilIiIIliiiil]for SynapseXen_illIiilIiI=0,#SynapseXen_IiIiiilliiIiIIlii do local SynapseXen_IiiIilIiIIiilIl=SynapseXen_IiIiiilliiIiIIlii[SynapseXen_illIiilIiI]local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_IiiIilIiIIiilIl[1]local SynapseXen_llIIi=SynapseXen_IiiIilIiIIiilIl[2]if SynapseXen_iIiiiliIliiilillIiil==SynapseXen_lllIII and SynapseXen_llIIi>=SynapseXen_llIilIiliIliIlIlIilI then SynapseXen_iliIIi[SynapseXen_llIIi]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIIi]SynapseXen_IiiIilIiIIiilIl[1]=SynapseXen_iliIIi end end end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1508972779]or(function(...)local SynapseXen_iiIiIii="now with shitty xor string obfuscation"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1203064705,62615007)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(789277919,789258405)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1508972779]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1050964778,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1155306162,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{57217000,3630151773,2959534425}return SynapseXen_iiillI[1508972779]end)({}))then if SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[125078388]or(function()local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"SynapseXen_iiillI[125078388]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(891376757,3351614541),SynapseXen_iIliiIIlIll(487788432,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2911201984,3174769667,3442635667,1089031493,1555957185,1947764625}return SynapseXen_iiillI[125078388]end)(),262144)==(SynapseXen_iiillI[3142095985]or(function()local SynapseXen_iiIiIii="yed"SynapseXen_iiillI[3142095985]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1360065615,715948741),SynapseXen_iIliiIIlIll(1832473907,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{3273294446,1277333586,4010735882,1000004426,2670678828,1658722910,3813807969,3261684797,2369434756,2020013034}return SynapseXen_iiillI[3142095985]end)())then SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3066423407]or(function()local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"SynapseXen_iiillI[3066423407]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(13527111,1021619692),SynapseXen_iIliiIIlIll(3554449476,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3297821667,3979487114,1051853076,2181686979,3676995893,4258969706,3198779315,3333470775,1457071923,4224115361}return SynapseXen_iiillI[3066423407]end)(),256)]=SynapseXen_iIiiIlilll else SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3066423407]or(function()local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"SynapseXen_iiillI[3066423407]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(13527111,1021619692),SynapseXen_iIliiIIlIll(3554449476,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3297821667,3979487114,1051853076,2181686979,3676995893,4258969706,3198779315,3333470775,1457071923,4224115361}return SynapseXen_iiillI[3066423407]end)(),256)]=SynapseXen_ilIiII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[125078388]or(function()local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"SynapseXen_iiillI[125078388]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(891376757,3351614541),SynapseXen_iIliiIIlIll(487788432,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2911201984,3174769667,3442635667,1089031493,1555957185,1947764625}return SynapseXen_iiillI[125078388]end)(),262144)]end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2046684063]or(function()local SynapseXen_iiIiIii="can we have an f in chat for ripull"SynapseXen_iiillI[2046684063]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2806243644,2907819350),SynapseXen_iIliiIIlIll(479466653,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{1054864166,1636052813,2682752993,2276696439,749194127,2718216177}return SynapseXen_iiillI[2046684063]end)())then SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1528019777]or(function()local SynapseXen_iiIiIii="baby i just fell for uwu,,,,,, i wanna be with uwu!11!!"SynapseXen_iiillI[1528019777]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2189736637,2693617471),SynapseXen_iIliiIIlIll(2181462738,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{889186768,1792920475,3864330739}return SynapseXen_iiillI[1528019777]end)(),256),SynapseXen_liIlIlIiiIIii,256)]={}elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1911969298]or(function()local SynapseXen_iiIiIii="thats how mafia works"SynapseXen_iiillI[1911969298]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2914196546,394313024),SynapseXen_iIliiIIlIll(440181148,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3574105190,127412007,77025218}return SynapseXen_iiillI[1911969298]end)())then SynapseXen_IlillliIIilIlI[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3557363805]or(function(...)local SynapseXen_iiIiIii="aspect network better obfuscator"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2707715183,334251577)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2436488140,2436474632)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3557363805]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2899965974,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(4054307896,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2920485029,583725736}return SynapseXen_iiillI[3557363805]end)({}))]=SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3344712626]or(function()local SynapseXen_iiIiIii="hi devforum"SynapseXen_iiillI[3344712626]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1446157260,1216834385),SynapseXen_iIliiIIlIll(4047744186,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{1952771960,2064706760,3993933636,812823329}return SynapseXen_iiillI[3344712626]end)(),256)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1499830307]or(function()local SynapseXen_iiIiIii="pain is gonna use the backspace method on xen"SynapseXen_iiillI[1499830307]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1483576139,4292799086),SynapseXen_iIliiIIlIll(2579920103,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{435857347}return SynapseXen_iiillI[1499830307]end)())then SynapseXen_lllIII[SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[191132464]or(function()local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"SynapseXen_iiillI[191132464]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3948859513,2499682894),SynapseXen_iIliiIIlIll(3750761402,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3839466421}return SynapseXen_iiillI[191132464]end)())]=-SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3143936441]or(function()local SynapseXen_iiIiIii="wow xen is shit buy luraph ok"SynapseXen_iiillI[3143936441]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2775343050,2865396948),SynapseXen_iIliiIIlIll(2947132406,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{288630655}return SynapseXen_iiillI[3143936441]end)()),SynapseXen_liIlIlIiiIIii,512)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[493601938]or(function()local SynapseXen_iiIiIii="SECURE API, IMPOSSIBLE TO BYPASS!"SynapseXen_iiillI[493601938]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2552690324,3196626092),SynapseXen_iIliiIIlIll(1021339371,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-#{1501400291,4186650405,2879500413,2593596454,2228687988,3578398677,588495001,3611248976}return SynapseXen_iiillI[493601938]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2137511840]or(function()local SynapseXen_iiIiIii="hi xen crashes on my axon paste plz help"SynapseXen_iiillI[2137511840]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(904312381,2671789681),SynapseXen_iIliiIIlIll(2496743717,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{1075936081,4110731815,2684481574}return SynapseXen_iiillI[2137511840]end)(),256)~=0;local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3106620902]or(function()local SynapseXen_iiIiIii="pain exist is gonna connect the dots of xen"SynapseXen_iiillI[3106620902]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(426741143,4188129102),SynapseXen_iIliiIIlIll(1086536142,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{1333690244,1950337933,2506591915,728565965,238009206}return SynapseXen_iiillI[3106620902]end)(),512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[3592721855]or(function()local SynapseXen_iiIiIii="now with shitty xor string obfuscation"SynapseXen_iiillI[3592721855]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2499601572,145474961),SynapseXen_iIliiIIlIll(1012923967,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{4146400826,3854687138,2676966877,2530588854,2406613134,2123061391,3305326341}return SynapseXen_iiillI[3592721855]end)(),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;if SynapseXen_ilillliilIiiiIilIi<SynapseXen_IIiiiiliiIiiilliiIIl~=SynapseXen_llIilIiliIliIlIlIilI then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[572210195]or(function()local SynapseXen_iiIiIii="i'm intercommunication about the most nonecclesiastical dll exploits for esp. they only characterization objects with a antepatriarchal in the geistesgeschichte for the esp."SynapseXen_iiillI[572210195]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1127576782,3275177658),SynapseXen_iIliiIIlIll(537762627,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2351227915}return SynapseXen_iiillI[572210195]end)())then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3689788436]or(function(...)local SynapseXen_iiIiIii="epic gamer vision"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2954839459,301903867)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2311461645,2311408122)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3689788436]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3742212826,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2435086500,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{216331552,874560061,2697223447}return SynapseXen_iiillI[3689788436]end)({},"I",{}))local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2251940225]or(function()local SynapseXen_iiIiIii="SECURE API, IMPOSSIBLE TO BYPASS!"SynapseXen_iiillI[2251940225]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1770561671,33481242),SynapseXen_iIliiIIlIll(3363364828,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2862599259,3824669256,146014944,1185780959,2488254951,440828172,549300425,2366869154}return SynapseXen_iiillI[2251940225]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[746131507]or(function(...)local SynapseXen_iiIiIii="aspect network better obfuscator"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1884189599,1266611508)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2251909084,2251889363)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[746131507]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2634323794,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1642677004,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{450154503,813078805,2598106582,1008182555,1933447488,1405494959}return SynapseXen_iiillI[746131507]end)(1562,"illlIiilllIi",{}),256)]=SynapseXen_ilillliilIiiiIilIi^SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2726522892]or(function()local SynapseXen_iiIiIii="luraph better then xen bros :pensive:"SynapseXen_iiillI[2726522892]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1990334048,3612979091),SynapseXen_iIliiIIlIll(33040321,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1952494389,777334014,562116356,4223105539,1328102095,2972937099,2379915016,418940042}return SynapseXen_iiillI[2726522892]end)())then SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2014547571]or(function(...)local SynapseXen_iiIiIii="xen best rerubi paste"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4038491022,3475592070)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2477261549,2477239127)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2014547571]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(700246680,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3056354877,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2271444184,1139885176,523180085,1837156197,1950912515,884104112}return SynapseXen_iiillI[2014547571]end)({},{}),256)]=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[2424955693]or(function(...)local SynapseXen_iiIiIii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(419450321,2013473451)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1158178952,1158152928)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2424955693]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(4001005205,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(781060646,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3520506152}return SynapseXen_iiillI[2424955693]end)({},13595,{},{},{},{},{},8089),512)~=0;if SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[321813175]or(function()local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"SynapseXen_iiillI[321813175]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3820949731,135659052),SynapseXen_iIliiIIlIll(73310493,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2694212084,1996379512,72336822,2570554825,1442085555,2578874408,221425916,2089895537,2965287907,2030474248}return SynapseXen_iiillI[321813175]end)(),512)~=0 then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3677113070]or(function()local SynapseXen_iiIiIii="this is a christian obfuscator, no cursing allowed in our scripts"SynapseXen_iiillI[3677113070]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4247008643,2578433725),SynapseXen_iIliiIIlIll(3298602711,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1676528607,2530689385,709915686,3284975103,3628804469,4083897860}return SynapseXen_iiillI[3677113070]end)())then SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[4043003871]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[4043003871]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2770538277,371230755),SynapseXen_iIliiIIlIll(319682090,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2786776896,636664823,3940470032,646488919}return SynapseXen_iiillI[4043003871]end)(),256),SynapseXen_liIlIlIiiIIii,256)]=#SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1087016205]or(function()local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"SynapseXen_iiillI[1087016205]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2332346518,2226255618),SynapseXen_iIliiIIlIll(422618850,SynapseXen_ilIiII[5]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{3625801674,4208605682,668272192,2082801879,424065923}return SynapseXen_iiillI[1087016205]end)(),512),SynapseXen_liIlIlIiiIIii,512)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[52081773]or(function()local SynapseXen_iiIiIii="SYNAPSE XEN [FE BYPASS] [BETTER THEN LURAPH] [AMAZING] OMG OMG OMG !!!!!!"SynapseXen_iiillI[52081773]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2038428918,3292645727),SynapseXen_iIliiIIlIll(2880989053,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{846856141,2857399073,3898330419,710863425,1091877823,3489228181,3467475519,1100985586}return SynapseXen_iiillI[52081773]end)())then SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3129168751]or(function()local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"SynapseXen_iiillI[3129168751]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2173352780,1924010076),SynapseXen_iIliiIIlIll(1395407752,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2997373611,1125168509,1876786103,876193598,1644739340,3798811834}return SynapseXen_iiillI[3129168751]end)(),256)]=SynapseXen_iiIiiiIiIiiII[SynapseXen_iIlIlI[SynapseXen_iIliiIIlIll(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1786993118],SynapseXen_iiillI[3614410741]or(function(...)local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3525846274,1480827496)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2644490434,2644483330)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3614410741]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2422832268,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(3120579753,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3026976677,2935463647,2582161028,1660462004,460758915}return SynapseXen_iiillI[3614410741]end)(451,5070,"IiIIillliiiIiiiliI",9817,{},{},{},"IIiiIIIiIllilllIil",3493),262144),SynapseXen_liIlIlIiiIIii)]]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1522158606]or(function(...)local SynapseXen_iiIiIii="hi my 2.5mb script doesn't work with xen please help"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(227508332,1886720295)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4051828149,4051807322)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1522158606]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1004964202,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1352265646,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{1447606521,198872}return SynapseXen_iiillI[1522158606]end)(6822,{}))then local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[326698105]or(function()local SynapseXen_iiIiIii="thats how mafia works"SynapseXen_iiillI[326698105]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1355193991,510497577),SynapseXen_iIliiIIlIll(2707244567,SynapseXen_ilIiII[4]))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{153997148,2316926977,2731401574,825374106,2274878012}return SynapseXen_iiillI[326698105]end)(),512),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_liIliiIiiliIlll=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]for SynapseXen_IiIilIiIIliiiil=SynapseXen_ilillliilIiiiIilIi+1,SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[3159433972]or(function(...)local SynapseXen_iiIiIii="sometimes it be like that"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2037567718,3275108422)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(788030446,788018091)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3159433972]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3023676165,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2926949142,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{1655997409,2726482294,2532726524,861048326,1517305329}return SynapseXen_iiillI[3159433972]end)("iIIIlillIiii",4224))do SynapseXen_liIliiIiiliIlll=SynapseXen_liIliiIiiliIlll..SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]end;SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[4005480140]or(function(...)local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3680263828,4032609388)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1563532700,1563524724)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[4005480140]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(4249227193,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2008443559,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{1802961511,2004568906,2374768045,3730611120,2727115718,34466491,1995325446,1487658627}return SynapseXen_iiillI[4005480140]end)("iIIllilIll",8319,9449,10938,{},{},{},"IlIiiIlIlll"),256)]=SynapseXen_liIliiIiiliIlll elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1454317037]or(function()local SynapseXen_iiIiIii="sponsored by ironbrew, jk xen is better"SynapseXen_iiillI[1454317037]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3333435388,3529768498),SynapseXen_iIliiIIlIll(3541461255,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{1046098789,1912663687,1802490923,1950500574,1629122017,3908963528,2000820675}return SynapseXen_iiillI[1454317037]end)())then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIliiIIlIll(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[3977026417]or(function()local SynapseXen_iiIiIii="i put more time into this shitty list of dead memes then i did into the obfuscator itself"SynapseXen_iiillI[3977026417]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1211259089,3167973501),SynapseXen_iIliiIIlIll(3798217418,SynapseXen_ilIiII[5]))-string.len(SynapseXen_iiIiIii)-#{2763327793,4247789243,2699654519,3722728021,4205802907,1738701753,4261951331,3571130306,2162818440,1010872510}return SynapseXen_iiillI[3977026417]end)(),512),SynapseXen_liIlIlIiiIIii)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[3316521882]or(function()local SynapseXen_iiIiIii="https://twitter.com/Ripull_RBLX/status/1059334518581145603"SynapseXen_iiillI[3316521882]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2329255125,2450870586),SynapseXen_iIliiIIlIll(3100596982,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3048180011,241702476,3045680135,694541886,2441046559,3296198434,2084356768}return SynapseXen_iiillI[3316521882]end)())local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3482113991]or(function(...)local SynapseXen_iiIiIii="xen best rerubi paste"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(2909297731,2611322732)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3850804683,3850754650)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3482113991]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1397879493,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2144520898,SynapseXen_ilIiII[3]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2797963986,1358636942,3290156967,167403664,2182614080}return SynapseXen_iiillI[3482113991]end)({}),256)]=SynapseXen_ilillliilIiiiIilIi+SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[361201953]or(function()local SynapseXen_iiIiIii="SYNAPSE XEN [FE BYPASS] [BETTER THEN LURAPH] [AMAZING] OMG OMG OMG !!!!!!"SynapseXen_iiillI[361201953]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(4151175021,1055926708),SynapseXen_iIliiIIlIll(1771200138,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3927213083,4005107826,477559146,2540635512,816876653,4293617740,3420010868}return SynapseXen_iiillI[361201953]end)())then local SynapseXen_ilillliilIiiiIilIi=SynapseXen_IllillliiIl(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[937429640]or(function(...)local SynapseXen_iiIiIii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3553753344,1132652334)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(3549598470,3549562169)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[937429640]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1429149619,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1701990560,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{2268473195,819457650,1472424080,1331350921,1972771059,4283822194,3791838441,3635315837,2561967500,3069003495}return SynapseXen_iiillI[937429640]end)("I",2159,{},8210,5091,"IIlliiIilililIIiIll","illliIlIll")),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[3651314211]or(function()local SynapseXen_iiIiIii="SECURE API, IMPOSSIBLE TO BYPASS!"SynapseXen_iiillI[3651314211]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1124727488,2956859664),SynapseXen_iIliiIIlIll(1396220039,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{645303074,1490892074}return SynapseXen_iiillI[3651314211]end)(),512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;if SynapseXen_ilillliilIiiiIilIi>255 then SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIlIlI[SynapseXen_ilillliilIiiiIilIi-256]else SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_ilillliilIiiiIilIi]end;if SynapseXen_IIiiiiliiIiiilliiIIl>255 then SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIlIlI[SynapseXen_IIiiiiliiIiiilliiIIl-256]else SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IIiiiiliiIiiilliiIIl]end;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3949316194]or(function()local SynapseXen_iiIiIii="hi xen doesn't work on sk8r please help"SynapseXen_iiillI[3949316194]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(2224282012,124081677),SynapseXen_iIliiIIlIll(603594519,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{752290749,1930769363,3594916044,3052066607,3639405610,1941279274,1830585680,1520410778,2076408710}return SynapseXen_iiillI[3949316194]end)(),256)]=SynapseXen_ilillliilIiiiIilIi*SynapseXen_IIiiiiliiIiiilliiIIl elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[3578318333]or(function(...)local SynapseXen_iiIiIii="pain exist is gonna connect the dots of xen"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(1847112901,1452959408)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2734898002,2734845288)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3578318333]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(3637107384,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1078241779,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{3002005988,3288421450,626393181,228637325}return SynapseXen_iiillI[3578318333]end)({}))then if not not SynapseXen_lllIII[SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[3668691452]or(function(...)local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3411944615,330286082)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2997365884,2997292612)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[3668691452]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(1659064857,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1442531899,SynapseXen_ilIiII[4]))-string.len(SynapseXen_iiIiIii)-#{1060904194,285315023,845630419,3627125063,4005316203,4217306342,4030779116,1797057377,263183100,4019449779}return SynapseXen_iiillI[3668691452]end)(10773,9136,"iIlIliII",3205,{},4296,"IIllil","lIIII","Ii","iIlil"),256)]==(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[1617625881],SynapseXen_iiillI[2058555850]or(function(...)local SynapseXen_iiIiIii="imagine using some lua minifier tool and thinking you are a badass"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(953797943,3124416205)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(4184456883,4184402309)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2058555850]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2972530438,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(230373556,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{3247576222,3605116323,1625146390}return SynapseXen_iiillI[2058555850]end)({},"lIi",{},{},"iiiiilillilIIiIiiii",{},12896,"i"),512)==0)then SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+1 end elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2266758096]or(function()local SynapseXen_iiIiIii="level 1 crook = luraph, level 100 boss = xen"SynapseXen_iiillI[2266758096]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3171356214,29169658),SynapseXen_iIliiIIlIll(481576588,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{1546919762,4089677477,2921026671,968045002}return SynapseXen_iiillI[2266758096]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[2533947156]or(function()local SynapseXen_iiIiIii="skisploit is the superior obfuscator, clearly."SynapseXen_iiillI[2533947156]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(353598643,781688907),SynapseXen_iIliiIIlIll(2609622349,SynapseXen_iIiiIlilll))-SynapseXen_IiliiIiilIill-string.len(SynapseXen_iiIiIii)-#{2457217483,1197384597,2400070750}return SynapseXen_iiillI[2533947156]end)())local SynapseXen_ilillliilIiiiIilIi=SynapseXen_iIIlIlliIliI(SynapseXen_iIliiIIlIll(SynapseXen_lIIiiI[1776583566],SynapseXen_iiillI[1196552026]or(function(...)local SynapseXen_iiIiIii="can we have an f in chat for ripull"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3907667598,2209005627)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(2983891335,2983861098)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl-SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[1196552026]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2909717427,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1713424947,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2177751202,341398035,451089984,3023224137,1583445933,877891505,2134835394,3091783011}return SynapseXen_iiillI[1196552026]end)("iiilili",10591,3627,{},2957,{},"lI",14055,11114,{})),SynapseXen_liIlIlIiiIIii,512)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;local SynapseXen_IlIiIIllIiiiiiIlil,SynapseXen_Illlli;local SynapseXen_IiIiIIiIl;if SynapseXen_ilillliilIiiiIilIi==1 then return elseif SynapseXen_ilillliilIiiiIilIi==0 then SynapseXen_IiIiIIiIl=SynapseXen_liillliiliiIlI else SynapseXen_IiIiIIiIl=SynapseXen_llIilIiliIliIlIlIilI+SynapseXen_ilillliilIiiiIilIi-2 end;SynapseXen_Illlli={}SynapseXen_IlIiIIllIiiiiiIlil=0;for SynapseXen_IiIilIiIIliiiil=SynapseXen_llIilIiliIliIlIlIilI,SynapseXen_IiIiIIiIl do SynapseXen_IlIiIIllIiiiiiIlil=SynapseXen_IlIiIIllIiiiiiIlil+1;SynapseXen_Illlli[SynapseXen_IlIiIIllIiiiiiIlil]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_IiIilIiIIliiiil]end;return SynapseXen_Illlli,SynapseXen_IlIiIIllIiiiiiIlil elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[2658739086]or(function(...)local SynapseXen_iiIiIii="now comes with a free n word pass"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(3841902991,1452702870)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(1358621950,1358608060)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[2658739086]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(4273606416,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(1923438296,SynapseXen_ilIiII[1]))-string.len(SynapseXen_iiIiIii)-#{1283788638,3281840140,2102245561,3861909759,2698522848,3382043685}return SynapseXen_iiillI[2658739086]end)(5142,{},"iIIiIIlIIiill"))then SynapseXen_liIlIlIiiIIii=SynapseXen_lllIII[SynapseXen_iIIlIlliIliI(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[1116720423]or(function()local SynapseXen_iiIiIii="aspect network better obfuscator"SynapseXen_iiillI[1116720423]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(3605785222,1034960101),SynapseXen_iIliiIIlIll(1263867005,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-#{2719128139}return SynapseXen_iiillI[1116720423]end)(),256)]elseif SynapseXen_IiliiIiilIill==(SynapseXen_iiillI[1834594209]or(function()local SynapseXen_iiIiIii="double-header fair! this rationalization has a overenthusiastically anticheat! you will get nonpermissible for exploiting!"SynapseXen_iiillI[1834594209]=SynapseXen_iIliiIIlIll(SynapseXen_lliiiIiIIIlilI(1992469452,1283120073),SynapseXen_iIliiIIlIll(4251190461,SynapseXen_ilIiII[2]))-string.len(SynapseXen_iiIiIii)-#{1754326017,1178880326,3688562448,544476494,4046115588,3160388761,3446957935,3179214930,66913121,2285028924}return SynapseXen_iiillI[1834594209]end)())then local SynapseXen_llIilIiliIliIlIlIilI=SynapseXen_IllillliiIl(SynapseXen_IllillliiIl(SynapseXen_lIIiiI[909370254],SynapseXen_iiillI[961806922]or(function(...)local SynapseXen_iiIiIii="HELP ME PEOPLE ARE CRASHING MY GAME PLZ HELP"local SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_lliiiIiIIIlilI(4255656167,2886284828)local SynapseXen_iliiII={...}for SynapseXen_iiIII,SynapseXen_Iilll in pairs(SynapseXen_iliiII)do local SynapseXen_IlIIlliiIliI;local SynapseXen_iiliIliiIllIlIiilIii=type(SynapseXen_Iilll)if SynapseXen_iiliIliiIllIlIiilIii=="number"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll elseif SynapseXen_iiliIliiIllIlIiilIii=="string"then SynapseXen_IlIIlliiIliI=SynapseXen_Iilll:len()elseif SynapseXen_iiliIliiIllIlIiilIii=="table"then SynapseXen_IlIIlliiIliI=SynapseXen_lliiiIiIIIlilI(749401577,749384540)end;SynapseXen_iIIliiIIlllIiliIIl=SynapseXen_iIIliiIIlllIiliIIl+SynapseXen_IlIIlliiIliI end;SynapseXen_iiillI[961806922]=SynapseXen_iIliiIIlIll(SynapseXen_iIliiIIlIll(2250902782,SynapseXen_iIIliiIIlllIiliIIl),SynapseXen_iIliiIIlIll(2005603631,SynapseXen_iIiiIlilll))-string.len(SynapseXen_iiIiIii)-SynapseXen_IiliiIiilIill-#{162715288,1540254499}return SynapseXen_iiillI[961806922]end)({},"liiIlIlIIiI","I",{},12322,"IlIilll",{},"i"),256),SynapseXen_liIlIlIiiIIii,256)local SynapseXen_iIiiiliIliiilillIiil=SynapseXen_lllIII;SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]=assert(tonumber(SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]),'`for` initial value must be a number')SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1]=assert(tonumber(SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+1]),'`for` limit must be a number')SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2]=assert(tonumber(SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2]),'`for` step must be a number')SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]=SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI]-SynapseXen_iIiiiliIliiilillIiil[SynapseXen_llIilIiliIliIlIlIilI+2]SynapseXen_iIIIIIil=SynapseXen_iIIIIIil+SynapseXen_lIIiiI[1300471799]end end end;local SynapseXen_liIIIli={...}for SynapseXen_IiIilIiIIliiiil=0,SynapseXen_IIliiIllI do if SynapseXen_IiIilIiIIliiiil>=SynapseXen_IiliIIiIillIlll[1958689028]then SynapseXen_lIIiilIIliIIiI[SynapseXen_IiIilIiIIliiiil-SynapseXen_IiliIIiIillIlll[1958689028]]=SynapseXen_liIIIli[SynapseXen_IiIilIiIIliiiil+1]else SynapseXen_lllIII[SynapseXen_IiIilIiIIliiiil]=SynapseXen_liIIIli[SynapseXen_IiIilIiIIliiiil+1]end end;local SynapseXen_ilillliilIiiiIilIi,SynapseXen_IIiiiiliiIiiilliiIIl=SynapseXen_iilIIlliiIiIlIIIl()if SynapseXen_ilillliilIiiiIilIi and SynapseXen_IIiiiiliiIiiilliiIIl>0 then return unpack(SynapseXen_ilillliilIiiiIilIi,1,SynapseXen_IIiiiiliiIiiilliiIIl)end;return end end;local function SynapseXen_llllIIIlIll(SynapseXen_lIIlIiIii,SynapseXen_iiIiiiIiIiiII)local SynapseXen_liilIiiIiIIilIliIlii=SynapseXen_lillilI(SynapseXen_lIIlIiIii)return SynapseXen_IiIil(SynapseXen_liilIiiIiIIilIliIlii,SynapseXen_iiIiiiIiIiiII or getfenv(0)),SynapseXen_liilIiiIiIIilIliIlii end;return SynapseXen_llllIIIlIll(SynapseXen_iIIliIiIiilllliIilli("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"),getfenv())()("Clicked")
									end)


									section1:addButton("FE Orange Justice Dance", function()
										game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 0

wait(1)
game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16

local CountSCIFIMOVIELOL = 1
function SCIFIMOVIELOL(Part0,Part1,Position,Angle)
	local AlignPos = Instance.new('AlignPosition', Part1); AlignPos.Name = "AliP_"..CountSCIFIMOVIELOL
	AlignPos.ApplyAtCenterOfMass = true;
	AlignPos.MaxForce = 5772000--67752;
	AlignPos.MaxVelocity = math.huge/9e110;
	AlignPos.ReactionForceEnabled = false;
	AlignPos.Responsiveness = 200;
	AlignPos.RigidityEnabled = true;
	local AlignOri = Instance.new('AlignOrientation', Part1); AlignOri.Name = "AliO_"..CountSCIFIMOVIELOL
	AlignOri.MaxAngularVelocity = math.huge/9e110;
	AlignOri.MaxTorque = 5772000
	AlignOri.PrimaryAxisOnly = false;
	AlignOri.ReactionTorqueEnabled = false;
	AlignOri.Responsiveness = 200;
	AlignOri.RigidityEnabled = false;
	local AttachmentA=Instance.new('Attachment',Part1); AttachmentA.Name = "Ath_"..CountSCIFIMOVIELOL
	local AttachmentB=Instance.new('Attachment',Part0); AttachmentB.Name = "Ath_"..CountSCIFIMOVIELOL
	AttachmentA.Orientation = Angle or Vector3.new(0,0,0)
	AttachmentA.Position = Position or Vector3.new(0,0,0)
	AlignPos.Attachment1 = AttachmentA;
	AlignPos.Attachment0 = AttachmentB;
	AlignOri.Attachment1 = AttachmentA;
	AlignOri.Attachment0 = AttachmentB;
	CountSCIFIMOVIELOL = CountSCIFIMOVIELOL + 1
	return {AlignPos,AlignOri,AttachmentA,AttachmentB}
end

if _G.netted ~= true then
	_G.netted = true
	coroutine.wrap(function()
		settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
		settings().Physics.AllowSleep = false
		game:GetService("RunService").RenderStepped:Connect(function()
			game:FindFirstChildOfClass("Players").LocalPlayer.MaximumSimulationRadius=math.pow(math.huge,math.huge)
			sethiddenproperty(game:FindFirstChildOfClass("Players").LocalPlayer,"SimulationRadius",math.huge*math.huge)
		end)
	end)()
end

game:FindFirstChildOfClass("Players").LocalPlayer["Character"].Archivable = true
local hatnameclone = {}
for _,v in next, game:FindFirstChildOfClass("Players").LocalPlayer["Character"]:GetChildren() do
	if v:IsA("Accessory") then
		if hatnameclone[v.Name] then
			if hatnameclone[v.Name] == "s" then
				hatnameclone[v.Name] = {}
			end
			table.insert(hatnameclone[v.Name],v)
		else
			hatnameclone[v.Name] = "s"
		end
	end
end
for _,v in pairs(hatnameclone) do
	if type(v) == "table" then
		local num = 1
		for _,w in pairs(v) do
			w.Name = w.Name..num
			num = num + 1
		end
	end
end
hatnameclone = nil

local DeadChar = game:FindFirstChildOfClass("Players").LocalPlayer.Character

local fldr = Instance.new("Folder",game:FindFirstChildOfClass("Players").LocalPlayer["Character"])
fldr.Name = "DMYF"
local CloneChar = DeadChar:Clone()
local ANIMATIONHERE
if CloneChar:FindFirstChild("Animate") then
	ANIMATIONHERE = CloneChar:FindFirstChild("Animate"):Clone()
	CloneChar:FindFirstChild("Animate"):Destroy()
end
if CloneChar:FindFirstChildOfClass("Folder") then CloneChar:FindFirstChildOfClass("Folder"):Destroy() end
if CloneChar.Torso:FindFirstChild("Neck") then
	local Clonessss = CloneChar.Torso:FindFirstChild("Neck"):Clone()
	Clonessss.Part0 = nil
	Clonessss.Part1 = DeadChar.Head
	Clonessss.Parent = DeadChar.Torso
end
CloneChar.Parent = fldr
CloneChar.HumanoidRootPart.CFrame = DeadChar.HumanoidRootPart.CFrame
CloneChar.Humanoid.BreakJointsOnDeath = false
CloneChar.Name = "non"
CloneChar.Humanoid.DisplayDistanceType = "None"

for _,v in next, DeadChar:GetChildren() do
	if v:IsA("Accessory") then
		local topacc = false
		if v.Handle:FindFirstChildOfClass("Weld") then v.Handle:FindFirstChildOfClass("Weld"):Destroy() end
		v.Handle.Massless = true
		v.Handle.CanCollide = false
		if v.Handle:FindFirstChildOfClass("Attachment") then
			local ath__ = v.Handle:FindFirstChildOfClass("Attachment")
			if ath__.Name == "HatAttachment" or ath__.Name == "HairAttachment" or ath__.Name == "FaceFrontAttachment" or ath__.Name == "FaceCenterAttachment" then
				topacc = ath__.Name
			end
		end
        local bv = Instance.new("BodyVelocity",v.Handle)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			if topacc then
				local allthings = SCIFIMOVIELOL(v.Handle,DeadChar.Torso,Vector3.new(0,1.5,0)+ (DeadChar.Head[topacc].Position + (v.Handle[topacc].Position*-1)),Vector3.new(0,0,0))
				local normaltop = allthings[1].Attachment1
				local alipos = allthings[1]
				local alirot = allthings[2]
				local p0 = v.Handle
				local p1 = DeadChar.Head
				alipos.Parent = CloneChar:FindFirstChild(v.Name).Handle
				alirot.Parent = CloneChar:FindFirstChild(v.Name).Handle
				while true do
					game:GetService("RunService").RenderStepped:wait()
					if HumanDied then break end
					coroutine.wrap(function()
						if alipos.Attachment1 == normaltop then
							p0.CFrame = p0.CFrame:lerp((((DeadChar.Torso.CFrame * CFrame.new(0,1.5,0)) * p1[topacc].CFrame) * p0[topacc].CFrame:inverse()),1)
						else
							v.Handle.CFrame = v.Handle.CFrame:lerp(alipos.Attachment1.Parent.CFrame * CFrame.new(alipos.Attachment1.Position) * CFrame.Angles(math.rad(alipos.Attachment1.Rotation.X),math.rad(alipos.Attachment1.Rotation.Y),math.rad(alipos.Attachment1.Rotation.Z)),1)
						end
					end)()
				end
			else
				SCIFIMOVIELOL(v.Handle,CloneChar[v.Name].Handle,Vector3.new(0,0,0),Vector3.new(0,0,0))
			end
		end)()
    end
end

local a = DeadChar.Torso
local b = DeadChar.HumanoidRootPart
local c = DeadChar.Humanoid
a.Parent = game:FindFirstChildOfClass("Workspace")
c.Parent = game:FindFirstChildOfClass("Workspace")
local told = a:Clone()
local told1 = c:Clone()
b["RootJoint"].Part0 = told
b["RootJoint"].Part1 = DeadChar.Head
a.Name = "torso"
a.Neck:Destroy()
c.Name = "Humanoid"
told.Parent = DeadChar
told1.Parent = DeadChar
DeadChar.PrimaryPart = told
told1.Health = 0
b:Destroy()
a.Parent = DeadChar
c.Parent = DeadChar
told:Destroy()
told1:Destroy()
a.Name = "Torso"

if CloneChar.Head:FindFirstChildOfClass("Decal") then CloneChar.Head:FindFirstChildOfClass("Decal").Transparency = 1 end
if DeadChar:FindFirstChild("Animate") then DeadChar:FindFirstChild("Animate"):Destroy() end

local Collider
function UnCollide()
    if HumanDied then Collider:Disconnect(); return end
    --[[for _,Parts in next, CloneChar:GetChildren() do
        if Parts:IsA("BasePart") then
            Parts.CanCollide = false 
        end 
    end]]
    for _,Parts in next, DeadChar:GetChildren() do
        if Parts:IsA("BasePart") then
        Parts.CanCollide = false
        end 
    end 
end
Collider = game:GetService("RunService").Stepped:Connect(UnCollide)

local resetBindable = Instance.new("BindableEvent")
resetBindable.Event:connect(function()
    game:GetService("StarterGui"):SetCore("ResetButtonCallback", true)
	resetBindable:Clone()
	HumanDied = true
    pcall(function()
		game:FindFirstChildOfClass("Players").LocalPlayer.Character = DeadChar
		DeadChar.Head:Destroy()
		DeadChar:FindFirstChildOfClass("Humanoid"):Destroy()
		game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
		if DeadChar:FindFirstChildOfClass("Folder") then DeadChar:FindFirstChildOfClass("Folder"):Destroy() end
	end)
end)
game:GetService("StarterGui"):SetCore("ResetButtonCallback", resetBindable)

coroutine.wrap(function()
    while true do
        game:GetService("RunService").RenderStepped:wait()
        if not CloneChar or not CloneChar:FindFirstChild("Head") or not CloneChar:FindFirstChildOfClass("Humanoid") or CloneChar:FindFirstChildOfClass("Humanoid").Health <= 0 and not DeadChar or not DeadChar:FindFirstChild("Head") or not DeadChar:FindFirstChildOfClass("Humanoid") or DeadChar:FindFirstChildOfClass("Humanoid").Health <= 0 then 
            HumanDied = true
            pcall(function()
				game:FindFirstChildOfClass("Players").LocalPlayer.Character = DeadChar
				DeadChar.Head:Destroy()
				DeadChar:FindFirstChildOfClass("Humanoid"):Destroy()
				game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
				if DeadChar:FindFirstChildOfClass("Folder") then DeadChar:FindFirstChildOfClass("Folder"):Destroy() end
			end)
            if resetBindable then
                game:GetService("StarterGui"):SetCore("ResetButtonCallback", true)
                resetBindable:Destroy()
            end
            break
        end		
    end
end)()


SCIFIMOVIELOL(DeadChar["Head"],CloneChar["Head"])
SCIFIMOVIELOL(DeadChar["Torso"],CloneChar["Torso"])
SCIFIMOVIELOL(DeadChar["Left Arm"],CloneChar["Left Arm"])
SCIFIMOVIELOL(DeadChar["Right Arm"],CloneChar["Right Arm"])
SCIFIMOVIELOL(DeadChar["Left Leg"],CloneChar["Left Leg"])
SCIFIMOVIELOL(DeadChar["Right Leg"],CloneChar["Right Leg"])

for _,v in pairs(DeadChar:GetChildren()) do
	if v:IsA("BasePart") and v.Name ~= "Head" then
		--[[local bv = Instance.new("BodyVelocity",v)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			while true do
				game:GetService("RunService").RenderStepped:wait()
				if HumanDied then break end
				v.CFrame = CloneChar[v.Name].CFrame
			end
		end)()]]
	elseif v:IsA("BasePart") and v.Name == "Head" then
		local bv = Instance.new("BodyVelocity",v)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			while true do
				game:GetService("RunService").RenderStepped:wait()
				if HumanDied then break end
				v.CFrame = DeadChar.Torso.CFrame * CFrame.new(0,1.5,0)
			end
		end)()
	end
end

for _,BodyParts in next, CloneChar:GetDescendants() do
if BodyParts:IsA("BasePart") or BodyParts:IsA("Part") then
BodyParts.Transparency = 1 end end
game:GetService("RunService").RenderStepped:wait()
game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
game:FindFirstChildOfClass("Workspace"):FindFirstChildOfClass("Camera").CameraSubject = CloneChar.Humanoid

for _,v in next, DeadChar:GetChildren() do
	if v:IsA("Accessory") then
		if v.Handle:FindFirstChildOfClass("Weld") then v.Handle:FindFirstChildOfClass("Weld"):Destroy() end
	end
end

if ANIMATIONHERE then ANIMATIONHERE.Parent = CloneChar end

print("animated by p00tp0t, converted and fixed by 123jl123")
warn("refit function (anti-death) and music added by adam222334II")
ArtificialHB = Instance.new("BindableEvent", script)
ArtificialHB.Name = "Heartbeat"
script:WaitForChild("Heartbeat")

Player = game:GetService("Players").LocalPlayer
PlayerGui = Player.PlayerGui
Cam = workspace.CurrentCamera
Backpack = Player.Backpack
Character = Player.Character
Humanoid = Character.Humanoid
Mouse = Player:GetMouse()
RootPart = Character["HumanoidRootPart"]
Torso = Character["Torso"]
Head = Character["Head"]
RightArm = Character["Right Arm"]
LeftArm = Character["Left Arm"]
RightLeg = Character["Right Leg"]
LeftLeg = Character["Left Leg"]
RootJoint = RootPart["RootJoint"]
Neck = Torso["Neck"]
RightShoulder = Torso["Right Shoulder"]
LeftShoulder = Torso["Left Shoulder"]
RightHip = Torso["Right Hip"]
LeftHip = Torso["Left Hip"]

Character = Player.Character
Humanoid = Character.Humanoid

local BODY = {}
for _, c in pairs(Character:GetDescendants()) do
	if c:IsA("BasePart") and c.Name ~= "Handle" then
		if c ~= RootPart and c ~= Torso and c ~= Head and c ~= RightArm and c ~= LeftArm and c ~= RightLeg and c ~= LeftLeg then
			c.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
		end
		table.insert(BODY,{c,c.Parent,c.Material,c.Color,c.Transparency,c.Size,c.Name})
	elseif c:IsA("JointInstance") then
		table.insert(BODY,{c,c.Parent,nil,nil,nil,nil,nil})
	end
end

function refit()
	Character.Parent = workspace
	for e = 1, #BODY do
		if BODY[e] ~= nil then
			local STUFF = BODY[e]
			local PART = STUFF[1]
			local PARENT = STUFF[2]
			local MATERIAL = STUFF[3]
			local COLOR = STUFF[4]
			local TRANSPARENCY = STUFF[5]
			--local SIZE = STUFF[6]
			local NAME = STUFF[7]
			if PART.ClassName == "Part" and PART ~= RootPart then
				PART.Material = MATERIAL
				PART.Transparency = TRANSPARENCY
				PART.Name = NAME
			end
			if PART.Parent ~= PARENT then
				Humanoid:remove()
				PART.Parent = PARENT
				Humanoid = IT("Humanoid",Character)
			end
		end
	end
end

Humanoid.Died:connect(function()
	           refit()
end)

Player = game:GetService("Players").LocalPlayer
PlayerGui = Player.PlayerGui
Cam = workspace.CurrentCamera
Backpack = Player.Backpack
Character = Player.Character
Humanoid = Character.Humanoid
Mouse = Player:GetMouse()
RootPart = Character["HumanoidRootPart"]
Torso = Character["Torso"]
Head = Character["Head"]
RightArm = Character["Right Arm"]
LeftArm = Character["Left Arm"]
RightLeg = Character["Right Leg"]
LeftLeg = Character["Left Leg"]
RootJoint = RootPart["RootJoint"]
Neck = Torso["Neck"]
RightShoulder = Torso["Right Shoulder"]
LeftShoulder = Torso["Left Shoulder"]
RightHip = Torso["Right Hip"]
LeftHip = Torso["Left Hip"]
local sick = Instance.new("Sound",Torso)

IT = Instance.new
CF = CFrame.new
VT = Vector3.new
RAD = math.rad
C3 = Color3.new
UD2 = UDim2.new
BRICKC = BrickColor.new
ANGLES = CFrame.Angles
EULER = CFrame.fromEulerAnglesXYZ
COS = math.cos
ACOS = math.acos
SIN = math.sin
ASIN = math.asin
ABS = math.abs
MRANDOM = math.random
FLOOR = math.floor

local Speed = 0.1
AnimationSpeed=.8
SmoothTime=2

function swait(num)
if num==0 or num==nil then
--if Stagger.Value==false or Stun.Value<=100 then
--Player.PlayerGui.Pacemaker.Heartbeat.Event:wait()
script.Heartbeat.Event:wait()
--end
else
for i=0,num do
--Player.PlayerGui.Pacemaker.Heartbeat.Event:wait()
script.Heartbeat.Event:wait()
--[[if Stagger.Value==true or Stun.Value>=StunT.Value then
break
end]]
end
end
end
script:WaitForChild("Heartbeat")

frame = 1/60
tf = 0
allowframeloss = false --if set to true will fire every frame it possibly can. This will result in multiple events happening at the same time whenever delta returns frame2 or greater.
tossremainder = false --if set to true t will be set to 0 after Fire()-ing.
lastframe = tick()
script.Heartbeat:Fire() --ayy lmao

game:GetService("RunService").Heartbeat:connect(function(s,p) --herp derp
    tf = tf + s
    if tf >= frame then
        if allowframeloss then
            script.Heartbeat:Fire()
            lastframe=tick()
        else
            ----print("FIRED "..math.floor(t/frame).." FRAME(S)","REMAINDER "..(t - frame(math.floor(t/frame))))
            for i=1, math.floor(tf/frame) do
                script.Heartbeat:Fire()
            end
            lastframe=tick()
        end
        if tossremainder then
            tf = 0
        else
            tf = tf - frame * math.floor(tf/frame)
        end
    end
end)

Anim = {
	Properties = {
		Looping = true,
		Priority = Enum.AnimationPriority.Core
	},
	Keyframes = {
		[0] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.Angles(0, 0, math.rad(-5.672)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.002, 0.045, -0.001) * CFrame.Angles(math.rad(-2.693), math.rad(17.074), math.rad(2.922)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.042, -0.001, 0.004) * CFrame.Angles(math.rad(0.344), math.rad(-6.016), math.rad(-0.917)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.078, -0.239, -0.05) * CFrame.Angles(math.rad(-9.225), math.rad(6.073), math.rad(-106.742)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.002, 0.007, -0.002) * CFrame.Angles(math.rad(-16.215), math.rad(-3.438), math.rad(71.963)),
					},
					["Head"] = {
					},
				},
			},
		},
		[0.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.03, 0.004, -0.016) * CFrame.Angles(math.rad(-1.604), math.rad(-0.573), math.rad(8.594)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.033, 0.044, -0.022) * CFrame.Angles(math.rad(-0.229), math.rad(6.646), math.rad(3.38)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(-0.077, 0.063, -0.003) * CFrame.Angles(math.rad(-2.349), math.rad(-10.714), math.rad(-1.432)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.078, -0.239, -0.05) * CFrame.Angles(math.rad(-22.288), math.rad(-23.549), math.rad(-95.627)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.003, -0.02, -0.032) * CFrame.Angles(math.rad(-5.844), math.rad(44.06), math.rad(64.859)),
					},
				},
			},
		},
		[0.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.087, 0.018, -0.007) * CFrame.Angles(math.rad(-4.641), math.rad(-3.495), math.rad(14.496)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.212, -0.061, -0.023) * CFrame.Angles(math.rad(4.24), math.rad(-8.136), math.rad(8.48)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.09, 0.101, -0.12) * CFrame.Angles(math.rad(-15.871), math.rad(7.792), math.rad(-6.016)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.116, -0.383, -0.139) * CFrame.Angles(math.rad(-118.144), math.rad(-66.062), math.rad(-171.085)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.141, -0.136, -0.408) * CFrame.Angles(math.rad(6.188), math.rad(68.984), math.rad(59.817)),
					},
				},
			},
		},
		[0.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.087, 0.017, 0.005) * CFrame.Angles(math.rad(-4.297), math.rad(-4.24), math.rad(10.714)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.229, -0.107, -0.003) * CFrame.Angles(math.rad(6.131), math.rad(-7.85), math.rad(5.787)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.126, 0.121, -0.157) * CFrame.Angles(math.rad(-17.131), math.rad(12.662), math.rad(-5.386)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.118, -0.401, -0.162) * CFrame.Angles(math.rad(-118.545), math.rad(-64.916), math.rad(-171.429)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.17, -0.174, -0.487) * CFrame.Angles(math.rad(0.974), math.rad(69.958), math.rad(64.687)),
					},
				},
			},
		},
		[0.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.001, 0, 0.006) * CFrame.Angles(math.rad(-0.057), math.rad(-6.016), math.rad(8.652)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.215, -0.123, -0.001) * CFrame.Angles(math.rad(4.526), math.rad(-8.422), math.rad(-0.172)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.156, 0.165, 0) * CFrame.Angles(math.rad(-9.855), math.rad(1.547), math.rad(-6.589)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.2, -0.572, -0.152) * CFrame.Angles(math.rad(-28.19), math.rad(-41.253), math.rad(-124.79)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.285, -0.136, -0.197) * CFrame.Angles(math.rad(-7.563), math.rad(21.486), math.rad(67.265)),
					},
				},
			},
		},
		[0.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.003, 0.005, -0.033) * CFrame.Angles(math.rad(9.053), math.rad(-5.443), math.rad(3.438)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.335, -0.072, -0.015) * CFrame.Angles(math.rad(4.526), math.rad(-8.422), math.rad(-9.511)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.15, 0.169, 0.041) * CFrame.Angles(math.rad(-5.787), math.rad(-7.792), math.rad(8.824)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.083, -0.224, 0.119) * CFrame.Angles(math.rad(-16.501), math.rad(-25.898), math.rad(-105.482)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.102, -0.336, -0.005) * CFrame.Angles(math.rad(-35.18), math.rad(12.834), math.rad(40.508)),
					},
				},
			},
		},
		[0.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.035, 0.015, -0.003) * CFrame.Angles(math.rad(2.75), math.rad(2.922), math.rad(-22.689)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.351, 0.262, -0.153) * CFrame.Angles(math.rad(-21.944), math.rad(-10.084), math.rad(-1.948)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.585, -0.02, -0.022) * CFrame.Angles(math.rad(3.724), math.rad(14.954), math.rad(-0.401)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.024, -0.231, -0.069) * CFrame.Angles(math.rad(-31.742), math.rad(9.053), math.rad(-16.902)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.034, -0.222, -0.102) * CFrame.Angles(math.rad(-37.471), math.rad(-6.303), math.rad(-3.38)),
					},
				},
			},
		},
		[0.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.05, 0.022, 0.004) * CFrame.Angles(math.rad(-1.432), math.rad(5.329), math.rad(-23.778)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.247, 0.234, -0.096) * CFrame.Angles(math.rad(-23.377), math.rad(-10.027), math.rad(-0.115)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.445, -0.087, -0.037) * CFrame.Angles(math.rad(5.443), math.rad(14.954), math.rad(-2.521)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.228, -0.201, 0.068) * CFrame.Angles(math.rad(-36.956), math.rad(-28.762), math.rad(-32.773)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.111, -0.23, 0.086) * CFrame.Angles(math.rad(-37.643), math.rad(44.805), math.rad(37.758)),
					},
				},
			},
		},
		[0.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.045, 0.037, 0.004) * CFrame.Angles(math.rad(0.688), math.rad(1.719), math.rad(-19.079)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.266, 0.165, -0.049) * CFrame.Angles(math.rad(-13.636), math.rad(3.724), math.rad(3.552)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.196, -0.191, 0.094) * CFrame.Angles(math.rad(-21.314), math.rad(-48.128), math.rad(-108.06)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.111, -0.23, 0.086) * CFrame.Angles(math.rad(-30.252), math.rad(29.221), math.rad(117.227)),
					},
				},
			},
		},
		[0.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.042, 0.041, -0.031) * CFrame.Angles(math.rad(6.589), math.rad(-4.354), math.rad(-2.807)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.304, -0.098, 0.035) * CFrame.Angles(math.rad(8.021), math.rad(-2.177), math.rad(-7.85)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.194, 0.099, 0.139) * CFrame.Angles(math.rad(-5.386), math.rad(5.271), math.rad(8.308)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.215, -0.178, 0.13) * CFrame.Angles(math.rad(-11.345), math.rad(-63.197), math.rad(-172.919)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.113, -0.242, 0.009) * CFrame.Angles(math.rad(-8.881), math.rad(1.203), math.rad(173.091)),
					},
				},
			},
		},
		[1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.051, 0.059, -0.015) * CFrame.Angles(math.rad(-3.896), math.rad(-12.204), math.rad(14.381)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.36, -0.26, -0.138) * CFrame.Angles(math.rad(19.366), math.rad(-11.287), math.rad(9.798)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.032, 0.252, 0.162) * CFrame.Angles(math.rad(-27.215), math.rad(22.575), math.rad(4.641)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.303, 0.264, 0.5) * CFrame.Angles(math.rad(105.768), math.rad(18.507), math.rad(-160.485)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.333, 0.352, 0.375) * CFrame.Angles(math.rad(94.653), math.rad(-19.251), math.rad(152.349)),
					},
				},
			},
		},
		[1.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.048, 0.06, 0.001) * CFrame.Angles(math.rad(-5.672), math.rad(-12.49), math.rad(19.824)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.422, -0.266, -0.161) * CFrame.Angles(math.rad(20.856), math.rad(-17.819), math.rad(12.777)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(-0.042, 0.289, 0.177) * CFrame.Angles(math.rad(-27.731), math.rad(22.689), math.rad(2.12)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.336, 0.25, 0.48) * CFrame.Angles(math.rad(85.142), math.rad(19.939), math.rad(-125.592)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.359, 0.32, 0.353) * CFrame.Angles(math.rad(89.668), math.rad(-27.674), math.rad(150.344)),
					},
				},
			},
		},
		[1.15] = {
			["Torso"] = {
				["Right Arm"] = {
					CFrame = CFrame.new(0.321, 0.194, 0.033) * CFrame.Angles(math.rad(40.107), math.rad(-28.934), math.rad(100.554)),
				},
			},
		},
		[1.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.049, 0.061, -0.011) * CFrame.Angles(math.rad(1.49), math.rad(-6.933), math.rad(0.917)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.34, -0.079, -0.013) * CFrame.Angles(math.rad(6.245), math.rad(-1.089), math.rad(-1.604)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.245, 0.143, 0.266) * CFrame.Angles(math.rad(-7.391), math.rad(0.172), math.rad(2.005)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.293, -0.086, -0.005) * CFrame.Angles(math.rad(36.268), math.rad(-12.662), math.rad(-73.109)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.037, 0.197, 0.018) * CFrame.Angles(math.rad(4.698), math.rad(7.735), math.rad(74.427)),
					},
				},
			},
		},
		[1.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.048, 0.063, -0.011) * CFrame.Angles(math.rad(12.433), math.rad(-3.037), math.rad(-5.099)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.497, 0.086, 0.049) * CFrame.Angles(math.rad(-3.667), math.rad(-9.053), math.rad(-8.652)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.411, 0.137, 0.235) * CFrame.Angles(math.rad(-3.037), math.rad(10.943), math.rad(16.558)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.089, -0.186, -0.303) * CFrame.Angles(math.rad(-15.699), math.rad(-37.586), math.rad(-87.032)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.002, 0.176, -0.012) * CFrame.Angles(math.rad(-3.438), math.rad(38.044), math.rad(68.64)),
					},
				},
			},
		},
		[1.35] = {
			["Torso"] = {
				["Left Leg"] = {
					CFrame = CFrame.new(-0.287, 0.032, -0.108) * CFrame.Angles(math.rad(-15.986), math.rad(-19.309), math.rad(-11.287)),
				},
				["Left Arm"] = {
					CFrame = CFrame.new(0.07, 0.097, -0.109) * CFrame.Angles(math.rad(-7.277), math.rad(-12.032), math.rad(-39.248)),
				},
				["Right Arm"] = {
					CFrame = CFrame.new(-0.14, 0.034, 0.05) * CFrame.Angles(math.rad(-15.871), math.rad(8.251), math.rad(46.868)),
				},
			},
		},
		[1.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.048, 0.06, 0.001) * CFrame.Angles(math.rad(7.678), math.rad(2.292), math.rad(-19.366)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.27, 0.114, -0.159) * CFrame.Angles(math.rad(-21.257), math.rad(-18.85), math.rad(-6.073)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.497, -0.039, 0.104) * CFrame.Angles(math.rad(3.094), math.rad(15.183), math.rad(7.678)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.053, -0.03, -0.111) * CFrame.Angles(math.rad(-20.054), math.rad(11.345), math.rad(4.756)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.067, 0.043, -0.051) * CFrame.Angles(math.rad(-16.902), math.rad(-16.1), math.rad(12.261)),
					},
				},
			},
		},
		[1.45] = {
			["Torso"] = {
				["Left Arm"] = {
					CFrame = CFrame.new(0.049, 0.073, -0.158) * CFrame.Angles(math.rad(-24.752), math.rad(11.23), math.rad(2.063)),
				},
			},
		},
		[1.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.048, 0.06, 0.008) * CFrame.Angles(math.rad(-2.063), math.rad(3.896), math.rad(-24.866)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.229, 0.099, -0.266) * CFrame.Angles(math.rad(-23.262), math.rad(-22.689), math.rad(5.042)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.503, -0.062, 0.038) * CFrame.Angles(math.rad(4.526), math.rad(21.543), math.rad(-4.698)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.036, 0, -0.052) * CFrame.Angles(math.rad(-27.559), math.rad(-13.579), math.rad(-37.013)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.094, 0.058, -0.054) * CFrame.Angles(math.rad(-20.97), math.rad(-5.73), math.rad(29.45)),
					},
				},
			},
		},
		[1.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.051, 0.068, -0.087) * CFrame.Angles(math.rad(5.329), math.rad(-2.235), math.rad(-22.632)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.131, 0.157, 0.065) * CFrame.Angles(math.rad(-9.912), math.rad(16.387), math.rad(-10.657)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.45, 0.143, 0.047) * CFrame.Angles(math.rad(-2.005), math.rad(16.673), math.rad(5.787)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.036, 0, -0.052) * CFrame.Angles(math.rad(-27.559), math.rad(-13.579), math.rad(-81.589)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.094, 0.058, -0.054) * CFrame.Angles(math.rad(-5.443), math.rad(-4.412), math.rad(72.479)),
					},
				},
			},
		},
		[1.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.05, 0.073, -0.042) * CFrame.Angles(math.rad(16.215), math.rad(-3.094), math.rad(-1.891)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.303, 0.14, 0.074) * CFrame.Angles(math.rad(-1.375), 0, math.rad(-16.043)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.209, 0.117, 0.033) * CFrame.Angles(math.rad(-4.698), math.rad(-3.839), math.rad(13.235)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.036, 0, -0.052) * CFrame.Angles(math.rad(-23.319), math.rad(-16.272), math.rad(-174.695)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.094, 0.058, -0.054) * CFrame.Angles(math.rad(-19.423), math.rad(-6.245), math.rad(173.492)),
					},
				},
			},
		},
		[1.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.046, 0.059, 0.012) * CFrame.Angles(math.rad(6.245), math.rad(-7.277), math.rad(13.866)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.45, -0.047, 0.018) * CFrame.Angles(math.rad(3.839), math.rad(-16.558), math.rad(-3.094)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.107, 0.253, -0.141) * CFrame.Angles(math.rad(-35.695), math.rad(18.105), math.rad(14.095)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.007, 0.442, -0.369) * CFrame.Angles(math.rad(-15.871), math.rad(-13.808), math.rad(-175.898)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.179, 0.563, -0.401) * CFrame.Angles(math.rad(-35.065), math.rad(-4.24), math.rad(172.059)),
					},
				},
			},
		},
		[1.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.042, 0.057, 0.033) * CFrame.Angles(math.rad(4.927), math.rad(-9.798), math.rad(19.652)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.455, -0.191, -0.002) * CFrame.Angles(math.rad(7.047), math.rad(-20.168), math.rad(-2.005)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.106, 0.26, -0.147) * CFrame.Angles(math.rad(-43.029), math.rad(16.845), math.rad(11.287)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.007, 0.442, -0.369) * CFrame.Angles(math.rad(-34.607), math.rad(-14.897), math.rad(-175.268)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.192, 0.619, -0.464) * CFrame.Angles(math.rad(-56.895), math.rad(-0.974), math.rad(171.028)),
					},
				},
			},
		},
		[2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.042, 0.057, 0.033) * CFrame.Angles(math.rad(8.652), math.rad(-3.61), math.rad(2.005)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.25, 0.008, 0.091) * CFrame.Angles(math.rad(1.49), math.rad(-0.344), math.rad(-6.761)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.363, 0.174, 0.049) * CFrame.Angles(math.rad(-12.261), math.rad(-14.209), math.rad(3.953)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.081, 0.146, -0.061) * CFrame.Angles(math.rad(4.183), math.rad(1.318), math.rad(-177.846)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.01, 0.285, -0.095) * CFrame.Angles(math.rad(-2.406), math.rad(-13.808), math.rad(165.241)),
					},
				},
			},
		},
		[2.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.042, 0.058, 0.032) * CFrame.Angles(math.rad(5.386), math.rad(1.776), math.rad(-10.084)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.195, 0.143, -0.045) * CFrame.Angles(math.rad(-17.876), math.rad(-26.299), math.rad(-3.266)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.404, -0.068, 0.108) * CFrame.Angles(math.rad(2.406), math.rad(9.282), math.rad(6.933)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.011, -0.005, 0.059) * CFrame.Angles(math.rad(-27.158), math.rad(-22.173), math.rad(-62.395)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.033, 0.165, -0.062) * CFrame.Angles(math.rad(-18.564), math.rad(-8.652), math.rad(70.646)),
					},
				},
			},
		},
		[2.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.042, 0.057, 0.028) * CFrame.Angles(math.rad(-2.177), math.rad(5.672), math.rad(-33.518)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.155, 0.117, -0.113) * CFrame.Angles(math.rad(-20.856), math.rad(-28.591), math.rad(8.938)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.494, -0.147, 0.041) * CFrame.Angles(math.rad(11.402), math.rad(28.934), math.rad(-10.371)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.337, 0.024, 0.073) * CFrame.Angles(math.rad(-22.002), math.rad(-45.493), math.rad(-6.818)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.049, -0.182, 0.094) * CFrame.Angles(math.rad(-32.086), math.rad(65.947), math.rad(51.165)),
					},
				},
			},
		},
		[2.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.039, 0.057, -0.012) * CFrame.Angles(math.rad(-0.573), math.rad(4.641), math.rad(-30.138)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.155, 0.117, -0.113) * CFrame.Angles(math.rad(-20.856), math.rad(-28.591), math.rad(4.584)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.505, -0.13, 0.058) * CFrame.Angles(math.rad(11.001), math.rad(29.049), math.rad(-10.199)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.187, -0.034, -0.024) * CFrame.Angles(math.rad(-43.889), math.rad(-20.397), math.rad(-91.329)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.089, -0.11, 0.015) * CFrame.Angles(math.rad(-14.095), math.rad(55.978), math.rad(105.252)),
					},
				},
			},
		},
		[2.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.058, 0.068, -0.114) * CFrame.Angles(math.rad(5.042), 0, math.rad(-9.167)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.336, 0.171, 0.073) * CFrame.Angles(math.rad(-2.979), math.rad(6.417), math.rad(-4.87)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.596, 0.088, 0.136) * CFrame.Angles(math.rad(-0.63), math.rad(8.995), math.rad(7.391)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.139, -0.509, -0.406) * CFrame.Angles(math.rad(-12.032), math.rad(-55.348), math.rad(-146.104)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.455, -0.795, -0.12) * CFrame.Angles(math.rad(-48.873), math.rad(58.785), math.rad(-173.721)),
					},
				},
			},
		},
		[2.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.13, 0.087, -0.184) * CFrame.Angles(math.rad(16.501), math.rad(-4.985), math.rad(2.979)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.354, 0.047, 0.005) * CFrame.Angles(math.rad(14.668), math.rad(21.371), math.rad(-21.257)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.519, 0.389, 0.099) * CFrame.Angles(math.rad(-21.715), math.rad(10.6), math.rad(21.887)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.281, -0.488, -0.292) * CFrame.Angles(math.rad(-54.087), math.rad(-44.003), math.rad(-151.662)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.404, -0.776, -0.036) * CFrame.Angles(math.rad(-69.557), math.rad(16.902), math.rad(145.818)),
					},
				},
			},
		},
		[2.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.139, 0.083, -0.256) * CFrame.Angles(math.rad(-2.922), math.rad(-7.506), math.rad(18.965)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.546, 0.034, -0.047) * CFrame.Angles(math.rad(5.558), math.rad(5.787), math.rad(11.115)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.298, 0.396, 0.089) * CFrame.Angles(math.rad(-30.023), math.rad(22.173), math.rad(-3.037)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.205, -0.546, -0.272) * CFrame.Angles(math.rad(-109.148), math.rad(-51.509), math.rad(-168.106)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.285, -0.428, -0.424) * CFrame.Angles(math.rad(-65.489), math.rad(52.54), math.rad(111.44)),
					},
				},
			},
		},
		[2.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.14, 0.081, -0.253) * CFrame.Angles(math.rad(-5.214), math.rad(-8.422), math.rad(21.257)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.508, 0.044, -0.088) * CFrame.Angles(math.rad(5.558), math.rad(5.787), math.rad(8.021)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.224, 0.406, 0.101) * CFrame.Angles(math.rad(-35.523), math.rad(22.345), math.rad(-0.974)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.21, -0.566, -0.31) * CFrame.Angles(math.rad(-109.148), math.rad(-51.509), math.rad(-168.106)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.297, -0.439, -0.437) * CFrame.Angles(math.rad(-65.489), math.rad(52.54), math.rad(111.44)),
					},
				},
			},
		},
		[2.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.14, 0.081, -0.253) * CFrame.Angles(math.rad(-1.833), math.rad(-5.844), math.rad(6.303)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.543, 0.032, -0.026) * CFrame.Angles(math.rad(5.042), math.rad(-4.698), math.rad(2.979)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.169, 0.407, 0.101) * CFrame.Angles(math.rad(-20.741), math.rad(0.057), math.rad(-1.948)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.019, -0.061, -0.045) * CFrame.Angles(math.rad(12.605), math.rad(-52.082), math.rad(-54.431)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.183, 0.039, 0.188) * CFrame.Angles(math.rad(13.636), math.rad(39.534), math.rad(64.057)),
					},
				},
			},
		},
		[2.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.14, 0.081, -0.253) * CFrame.Angles(math.rad(12.319), math.rad(-0.573), math.rad(-8.365)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.46, 0.29, 0.028) * CFrame.Angles(math.rad(-3.552), math.rad(10.943), math.rad(-17.303)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.581, 0.321, 0.146) * CFrame.Angles(math.rad(-9.969), math.rad(-4.183), math.rad(13.235)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.019, -0.061, -0.045) * CFrame.Angles(math.rad(-25.038), math.rad(-19.824), math.rad(-60.848)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.078, -0.058, -0.086) * CFrame.Angles(math.rad(-13.465), math.rad(3.667), math.rad(44.977)),
					},
				},
			},
		},
		[3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.14, 0.081, -0.253) * CFrame.Angles(math.rad(-1.49), math.rad(3.61), math.rad(-24.924)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.107, 0.306, 0.157) * CFrame.Angles(math.rad(-11.975), math.rad(24.236), math.rad(-2.235)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.596, 0.093, 0.036) * CFrame.Angles(math.rad(3.839), math.rad(-12.376), math.rad(-4.756)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.019, -0.061, -0.045) * CFrame.Angles(math.rad(-14.782), math.rad(18.736), math.rad(-15.183)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.078, -0.058, -0.086) * CFrame.Angles(math.rad(-23.72), math.rad(-5.386), math.rad(2.521)),
					},
				},
			},
		},
		[3.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.14, 0.08, -0.262) * CFrame.Angles(math.rad(-3.266), math.rad(4.412), math.rad(-24.809)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.088, 0.305, 0.149) * CFrame.Angles(math.rad(-11.975), math.rad(24.236), math.rad(1.604)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.479, 0.082, 0.053) * CFrame.Angles(math.rad(3.839), math.rad(-12.376), math.rad(-4.756)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.019, -0.061, -0.045) * CFrame.Angles(math.rad(-3.552), math.rad(13.579), math.rad(-35.638)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.078, -0.058, -0.086) * CFrame.Angles(math.rad(-20.397), math.rad(-0.286), math.rad(14.095)),
					},
				},
			},
		},
		[3.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.15, 0.087, -0.146) * CFrame.Angles(math.rad(-2.578), math.rad(1.375), math.rad(-19.882)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.066, 0.279, 0.031) * CFrame.Angles(math.rad(-5.5), math.rad(13.35), math.rad(7.334)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.479, 0.082, 0.053) * CFrame.Angles(math.rad(1.432), math.rad(-0.573), math.rad(0.516)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.035, -0.127, -0.037) * CFrame.Angles(math.rad(-26.07), math.rad(-54.66), math.rad(-113.331)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.078, -0.058, -0.086) * CFrame.Angles(math.rad(-20.684), math.rad(57.811), math.rad(77.808)),
					},
				},
			},
		},
		[3.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.144, 0.083, 0.087) * CFrame.Angles(math.rad(1.031), math.rad(-1.375), math.rad(8.995)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.338, 0.503, 0.011) * CFrame.Angles(math.rad(2.235), math.rad(8.652), math.rad(-14.668)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.302, 0.094, 0.056) * CFrame.Angles(math.rad(-8.766), math.rad(-13.579), math.rad(-4.068)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.287, 0.073, 0.288) * CFrame.Angles(math.rad(17.246), math.rad(-37.987), math.rad(-127.025)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.014, 0.21, -0.042) * CFrame.Angles(math.rad(2.12), math.rad(-1.031), math.rad(141.463)),
					},
					["Head"] = {
					},
				},
			},
		},
		[3.35] = {
			["Torso"] = {
				["Left Arm"] = {
					CFrame = CFrame.new(0.063, 0.181, 0.113) * CFrame.Angles(math.rad(48.3), math.rad(-20.741), math.rad(-135.619)),
				},
				["Right Arm"] = {
					CFrame = CFrame.new(0.047, 0.27, -0.141) * CFrame.Angles(math.rad(37.242), math.rad(-17.59), math.rad(150.287)),
				},
			},
		},
		[3.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.425, 0.028, 0.049) * CFrame.Angles(math.rad(-0.688), math.rad(-11.23), math.rad(28.075)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.547, 0.515, 0.116) * CFrame.Angles(math.rad(1.662), math.rad(-16.043), math.rad(-5.042)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(-0.106, 0.209, -0.005) * CFrame.Angles(math.rad(-32.601), math.rad(-25.096), math.rad(-19.423)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.309, 0.207, 0.477) * CFrame.Angles(math.rad(110.925), math.rad(11.975), math.rad(-158.537)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.033, 0.243, 0.326) * CFrame.Angles(math.rad(112.701), math.rad(-26.757), math.rad(-173.721)),
					},
				},
			},
		},
		[3.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.589, -0.052, -0.277) * CFrame.Angles(math.rad(-2.578), math.rad(-11.287), math.rad(27.731)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.507, 0.11, 0.115) * CFrame.Angles(math.rad(-2.235), math.rad(-25.153), math.rad(3.782)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(-0.216, 0.499, -0.206) * CFrame.Angles(math.rad(-44.347), math.rad(-35.638), math.rad(-26.07)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.433, 0.139, 0.31) * CFrame.Angles(math.rad(105.94), math.rad(27.445), math.rad(-138.484)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.187, 0.227, 0.306) * CFrame.Angles(math.rad(102.158), math.rad(-33.633), math.rad(164.152)),
					},
				},
			},
		},
		[3.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.573, -0.045, -0.23) * CFrame.Angles(math.rad(0.172), math.rad(-7.334), math.rad(16.329)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.303, 0.101, 0.163) * CFrame.Angles(math.rad(-5.901), math.rad(-12.834), math.rad(-4.011)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.178, 0.792, -0.049) * CFrame.Angles(math.rad(-8.709), math.rad(-26.986), math.rad(-5.329)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.464, 0.038, -0.258) * CFrame.Angles(math.rad(26.643), math.rad(3.094), math.rad(-103.018)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.316, 0.165, -0.017) * CFrame.Angles(math.rad(32.601), math.rad(7.735), math.rad(115.967)),
					},
				},
			},
		},
		[3.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.564, -0.07, -0.032) * CFrame.Angles(math.rad(13.063), math.rad(1.49), math.rad(-0.516)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.121, 0.068, 0.303) * CFrame.Angles(math.rad(-19.824), math.rad(0.745), math.rad(-23.491)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.111, 0.48, 0.192) * CFrame.Angles(math.rad(8.021), math.rad(-29.507), math.rad(14.897)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.485, -0.009, -0.262) * CFrame.Angles(math.rad(-36.211), math.rad(-55.806), math.rad(-136.192)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.316, 0.165, -0.017) * CFrame.Angles(math.rad(16.043), math.rad(76.433), math.rad(98.491)),
					},
				},
			},
		},
		[3.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.547, -0.042, -0.28) * CFrame.Angles(math.rad(10.313), math.rad(2.349), math.rad(-21.371)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.258, 0.489, -0.268) * CFrame.Angles(math.rad(-31.398), math.rad(27.33), math.rad(-0.115)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.436, 0.246, 0.081) * CFrame.Angles(math.rad(1.49), math.rad(-2.922), math.rad(7.85)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.295, -0.116, 0.162) * CFrame.Angles(math.rad(-46.983), math.rad(-48.243), math.rad(-46.238)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.298, -0.085, 0.068) * CFrame.Angles(math.rad(-21.371), math.rad(33.002), math.rad(43.717)),
					},
				},
			},
		},
		[3.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.546, -0.041, -0.288) * CFrame.Angles(math.rad(11.001), math.rad(-0.974), math.rad(-25.21)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.479, 0.543, -0.415) * CFrame.Angles(math.rad(-35.924), math.rad(27.788), math.rad(6.417)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.426, 0.286, 0.076) * CFrame.Angles(math.rad(-0.573), math.rad(-5.901), math.rad(10.657)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.318, -0.029, 0.106) * CFrame.Angles(math.rad(-46.983), math.rad(-48.243), math.rad(-18.85)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.269, -0.021, 0.052) * CFrame.Angles(math.rad(-28.992), math.rad(29.851), math.rad(27.788)),
					},
				},
			},
		},
		[4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.582, -0.041, -0.31) * CFrame.Angles(math.rad(6.933), math.rad(-2.922), math.rad(-19.996)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.389, 0.542, -0.181) * CFrame.Angles(math.rad(-22.173), math.rad(21.601), math.rad(4.183)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.544, 0.455, 0.01) * CFrame.Angles(math.rad(-15.298), math.rad(-5.214), math.rad(1.662)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.338, -0.046, 0.027) * CFrame.Angles(math.rad(-34.034), math.rad(-3.782), math.rad(-26.241)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.269, -0.021, 0.052) * CFrame.Angles(math.rad(-39.706), math.rad(37.758), math.rad(65.317)),
					},
				},
			},
		},
		[4.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.808, -0.041, -0.288) * CFrame.Angles(math.rad(0.573), math.rad(-4.698), math.rad(3.209)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.492, 0.449, 0.124) * CFrame.Angles(math.rad(-13.293), math.rad(10.428), math.rad(0.802)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.214, 0.548, 0.109) * CFrame.Angles(math.rad(-28.304), math.rad(-6.761), math.rad(-2.005)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.346, 0.064, -0.075) * CFrame.Angles(math.rad(-45.436), math.rad(-39.821), math.rad(-93.85)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.269, -0.021, 0.052) * CFrame.Angles(math.rad(-48.931), math.rad(55.176), math.rad(113.503)),
					},
				},
			},
		},
		[4.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(1.006, -0.041, -0.43) * CFrame.Angles(math.rad(-2.12), math.rad(-8.594), math.rad(20.684)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.666, 0.265, 0.106) * CFrame.Angles(math.rad(3.782), math.rad(-14.897), math.rad(8.422)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.12, 0.557, -0.336) * CFrame.Angles(math.rad(-56.551), math.rad(-10.027), math.rad(-18.335)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.4, 0.079, -0.05) * CFrame.Angles(math.rad(-33.174), math.rad(-82.334), math.rad(-179.164)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.351, -0.073, -0.047) * CFrame.Angles(math.rad(-10.313), math.rad(52.598), math.rad(166.33)),
					},
				},
			},
		},
		[4.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(1.022, -0.041, -0.449) * CFrame.Angles(math.rad(-0.057), math.rad(-9.454), math.rad(23.434)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.692, 0.263, 0.09) * CFrame.Angles(math.rad(5.329), math.rad(-18.908), math.rad(3.438)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.091, 0.56, -0.316) * CFrame.Angles(math.rad(-59.416), math.rad(-10.542), math.rad(-18.678)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.45, 0.067, 0.406) * CFrame.Angles(math.rad(-163.006), math.rad(-83.48), math.rad(15.355)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.059, -0.065, 0.199) * CFrame.Angles(math.rad(23.09), math.rad(73.797), math.rad(160.657)),
					},
				},
			},
		},
		[4.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.99, -0.041, -0.386) * CFrame.Angles(math.rad(1.604), math.rad(-4.584), math.rad(13.006)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.324, 0.262, 0.197) * CFrame.Angles(math.rad(-1.203), math.rad(-0.229), math.rad(-2.578)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.002, 0.567, -0.087) * CFrame.Angles(math.rad(-40.451), math.rad(-20.684), math.rad(-7.907)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.103, 0.151, 0.393) * CFrame.Angles(math.rad(-120.493), math.rad(-45.607), math.rad(-4.011)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.53, 0.031, 0.27) * CFrame.Angles(math.rad(-110.524), math.rad(48.759), math.rad(-29.737)),
					},
				},
			},
		},
		[4.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.906, -0.041, -0.256) * CFrame.Angles(math.rad(8.251), math.rad(0.573), math.rad(0.401)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.17, 0.428, -0.121) * CFrame.Angles(math.rad(-20.512), math.rad(15.183), math.rad(-6.589)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.341, 0.377, 0.473) * CFrame.Angles(math.rad(-11.058), math.rad(-5.615), math.rad(11.574)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.007, -0.451, 0.323) * CFrame.Angles(math.rad(-83.136), math.rad(-0.688), math.rad(-2.922)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.067, -0.373, 0.478) * CFrame.Angles(math.rad(-92.132), math.rad(3.094), math.rad(-0.286)),
					},
				},
			},
		},
		[4.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.681, -0.079, -0.449) * CFrame.Angles(math.rad(11.918), math.rad(2.12), math.rad(-11.803)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.323, 0.453, -0.621) * CFrame.Angles(math.rad(-56.895), math.rad(21.028), math.rad(1.719)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.694, 0.592, 0.212) * CFrame.Angles(math.rad(-6.073), math.rad(8.995), math.rad(10.714)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.133, -0.266, -0.019) * CFrame.Angles(math.rad(-23.09), math.rad(-4.125), math.rad(-85.428)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.173, -0.126, -0.269) * CFrame.Angles(math.rad(4.641), math.rad(6.245), math.rad(81.646)),
					},
				},
			},
		},
		[4.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.661, -0.16, -0.464) * CFrame.Angles(math.rad(16.501), math.rad(2.235), math.rad(-19.079)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.323, 0.453, -0.621) * CFrame.Angles(math.rad(-64.057), math.rad(20.684), math.rad(4.24)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.807, 0.559, 0.202) * CFrame.Angles(math.rad(-3.037), math.rad(19.882), math.rad(15.011)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.133, -0.266, -0.019) * CFrame.Angles(math.rad(-18.335), math.rad(-47.957), math.rad(-110.638)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.123, -0.172, -0.016) * CFrame.Angles(math.rad(43.029), math.rad(77.006), math.rad(58.041)),
					},
				},
			},
		},
		[4.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.722, -0.16, -0.427) * CFrame.Angles(math.rad(11.23), math.rad(0.974), math.rad(-11.516)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.192, 0.679, -0.138) * CFrame.Angles(math.rad(-31.226), math.rad(18.678), math.rad(-1.891)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.551, 0.659, -0.146) * CFrame.Angles(math.rad(-15.47), math.rad(1.49), math.rad(9.683)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.082, -0.269, -0.073) * CFrame.Angles(math.rad(-31.971), math.rad(-43.144), math.rad(-110.581)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.137, -0.17, -0.052) * CFrame.Angles(math.rad(-9.053), math.rad(68.182), math.rad(101.356)),
					},
				},
			},
		},
		[4.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.936, -0.175, -0.38) * CFrame.Angles(math.rad(6.36), math.rad(1.261), math.rad(-0.802)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.466, 0.573, 0.174) * CFrame.Angles(math.rad(-9.397), math.rad(1.318), math.rad(-3.38)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.479, 0.647, -0.27) * CFrame.Angles(math.rad(-30.138), math.rad(-1.031), math.rad(-0.344)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.028, -0.242, -0.029) * CFrame.Angles(math.rad(-50.134), math.rad(-71.734), math.rad(-110.581)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.244, -0.229, -0.254) * CFrame.Angles(math.rad(-42.112), math.rad(60.504), math.rad(112.873)),
					},
				},
			},
		},
		[5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(1.008, -0.227, -0.573) * CFrame.Angles(math.rad(1.146), math.rad(-0.573), math.rad(7.448)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.419, 0.519, 0.106) * CFrame.Angles(math.rad(-0.115), math.rad(-9.053), math.rad(0.172)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.435, 0.662, -0.502) * CFrame.Angles(math.rad(-46.925), math.rad(-16.1), math.rad(-8.938)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.034, -0.36, -0.243) * CFrame.Angles(math.rad(-65.374), math.rad(-79.068), math.rad(-125.363)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.31, -0.334, -0.474) * CFrame.Angles(math.rad(-31.57), math.rad(74.771), math.rad(95.97)),
					},
				},
			},
		},
		[5.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(1.03, -0.221, -0.674) * CFrame.Angles(math.rad(2.12), math.rad(-0.344), math.rad(6.933)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.442, 0.624, 0.103) * CFrame.Angles(math.rad(-0.115), math.rad(-9.053), math.rad(-1.662)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.442, 0.768, -0.483) * CFrame.Angles(math.rad(-46.066), math.rad(-16.215), math.rad(-8.709)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.031, -0.354, -0.215) * CFrame.Angles(math.rad(-65.374), math.rad(-79.068), math.rad(-116.941)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.283, -0.297, -0.377) * CFrame.Angles(math.rad(-31.57), math.rad(74.771), math.rad(90.012)),
					},
				},
			},
		},
		[5.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(1.039, -0.199, -0.832) * CFrame.Angles(math.rad(6.761), math.rad(0.401), math.rad(-0.057)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.39, 0.962, -0.14) * CFrame.Angles(math.rad(-10.256), math.rad(-0.172), math.rad(-7.105)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.383, 1.056, -0.386) * CFrame.Angles(math.rad(-36.44), math.rad(-9.454), math.rad(0.63)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.068, -0.458, -0.094) * CFrame.Angles(math.rad(-61.02), math.rad(-40.623), math.rad(-107.601)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.27, -0.176, -0.409) * CFrame.Angles(math.rad(-18.85), math.rad(36.555), math.rad(96.944)),
					},
				},
			},
		},
		[5.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.894, -0.199, -0.832) * CFrame.Angles(math.rad(0.057), math.rad(1.261), math.rad(-7.277)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.47, 0.962, -0.231) * CFrame.Angles(math.rad(-21.715), math.rad(10.542), math.rad(2.865)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.422, 1.006, -0.033) * CFrame.Angles(math.rad(-20.512), math.rad(6.474), math.rad(-1.719)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.068, -0.458, -0.094) * CFrame.Angles(math.rad(-22.517), math.rad(-32.659), math.rad(-125.249)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.27, -0.176, -0.408) * CFrame.Angles(math.rad(1.948), math.rad(52.368), math.rad(129.03)),
					},
				},
			},
		},
		[5.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.73, -0.199, -0.832) * CFrame.Angles(math.rad(-1.089), math.rad(3.724), math.rad(-15.126)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.495, 0.992, -0.307) * CFrame.Angles(math.rad(-36.096), math.rad(9.511), math.rad(12.204)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.471, 0.743, 0.143) * CFrame.Angles(math.rad(1.547), math.rad(9.397), math.rad(-1.203)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.054, -0.096, -0.011) * CFrame.Angles(math.rad(-13.923), math.rad(-53.858), math.rad(-173.205)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.223, -0.131, -0.116) * CFrame.Angles(math.rad(23.491), math.rad(79.355), math.rad(148.167)),
					},
				},
			},
		},
		[5.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.622, -0.199, -0.832) * CFrame.Angles(math.rad(-2.578), math.rad(3.953), math.rad(-20.684)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.499, 0.977, -0.322) * CFrame.Angles(math.rad(-44.691), math.rad(12.49), math.rad(15.928)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.509, 0.714, 0.056) * CFrame.Angles(math.rad(4.526), math.rad(14.152), math.rad(-4.354)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.099, -0.093, 0.062) * CFrame.Angles(math.rad(20.512), math.rad(-56.608), math.rad(-160.715)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.227, -0.153, 0.005) * CFrame.Angles(math.rad(107.029), math.rad(77.464), math.rad(82.792)),
					},
				},
			},
		},
		[5.6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.633, -0.199, -0.808) * CFrame.Angles(math.rad(0.057), math.rad(0.688), math.rad(-14.267)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.574, 0.987, -0.321) * CFrame.Angles(math.rad(-39.419), math.rad(13.866), math.rad(10.772)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.387, 0.779, 0.059) * CFrame.Angles(math.rad(0.286), math.rad(1.261), math.rad(0.688)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.336, -0.125, 0.079) * CFrame.Angles(math.rad(-118.602), math.rad(-81.36), math.rad(21.887)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.041, -0.225, 0.122) * CFrame.Angles(math.rad(-125.249), math.rad(82.563), math.rad(-30.653)),
					},
				},
			},
		},
		[5.7] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.654, -0.199, -0.803) * CFrame.Angles(math.rad(-4.011), math.rad(-2.005), math.rad(-1.031)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.484, 0.902, 0.064) * CFrame.Angles(math.rad(-17.303), math.rad(11.287), math.rad(5.672)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.303, 0.891, -0.083) * CFrame.Angles(math.rad(-11.459), math.rad(-9.11), math.rad(-3.209)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.349, -0.016, 0.436) * CFrame.Angles(math.rad(-105.596), math.rad(-34.32), math.rad(2.406)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.149, -0.045, 0.448) * CFrame.Angles(math.rad(-114.592), math.rad(14.209), math.rad(-16.673)),
					},
				},
			},
		},
		[5.8] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.743, -0.199, -0.751) * CFrame.Angles(math.rad(-5.386), math.rad(-3.896), math.rad(6.646)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.619, 0.713, 0.197) * CFrame.Angles(math.rad(-5.329), math.rad(-7.448), math.rad(5.901)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.305, 0.917, -0.092) * CFrame.Angles(math.rad(-19.939), math.rad(-8.537), math.rad(-4.526)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.223, -0.347, 0.088) * CFrame.Angles(math.rad(-57.697), math.rad(26.7), math.rad(-21.257)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.422, -0.283, 0.271) * CFrame.Angles(math.rad(-63.484), math.rad(-48.873), math.rad(18.564)),
					},
				},
			},
		},
		[5.9] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.819, -0.202, -0.851) * CFrame.Angles(math.rad(-4.469), math.rad(-4.985), math.rad(10.485)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.657, 0.737, 0.192) * CFrame.Angles(math.rad(1.662), math.rad(-12.892), math.rad(5.672)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.38, 1.001, -0.526) * CFrame.Angles(math.rad(-38.331), math.rad(-6.646), math.rad(-6.99)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.015, -0.217, 0.086) * CFrame.Angles(math.rad(-30.94), math.rad(-17.361), math.rad(-62.624)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.346, -0.336, 0.168) * CFrame.Angles(math.rad(-46.238), math.rad(26.07), math.rad(76.719)),
					},
				},
			},
		},
		[6] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.622, -0.209, -0.667) * CFrame.Angles(math.rad(-0.401), math.rad(-2.807), math.rad(2.75)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.455, 0.636, 0.218) * CFrame.Angles(math.rad(-11.173), math.rad(-1.719), math.rad(-0.286)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.24, 0.781, -0.155) * CFrame.Angles(math.rad(-21.429), math.rad(-16.788), math.rad(-9.626)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.015, -0.217, 0.086) * CFrame.Angles(math.rad(-44.404), math.rad(-48.186), math.rad(-75.115)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.32, -0.215, 0.039) * CFrame.Angles(math.rad(-55.348), math.rad(44.576), math.rad(101.7)),
					},
				},
			},
		},
		[6.1] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.341, -0.116, -0.25) * CFrame.Angles(math.rad(-1.547), math.rad(0.344), math.rad(-5.329)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.252, 0.412, 0.336) * CFrame.Angles(math.rad(-16.96), math.rad(6.646), math.rad(6.073)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.181, 0.218, 0.006) * CFrame.Angles(math.rad(-8.652), math.rad(-13.178), math.rad(-3.953)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(-0.016, -0.231, 0.2) * CFrame.Angles(math.rad(-84.569), math.rad(-73.339), math.rad(-91.731)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(0.331, -0.246, 0.186) * CFrame.Angles(math.rad(-104.278), math.rad(80.157), math.rad(116.482)),
					},
				},
			},
		},
		[6.2] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.195, -0.063, -0.043) * CFrame.Angles(math.rad(-2.922), math.rad(4.985), math.rad(-14.61)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.141, 0.297, 0.16) * CFrame.Angles(math.rad(-26.07), math.rad(5.558), math.rad(7.047)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.177, -0.022, -0.095) * CFrame.Angles(math.rad(-0.859), math.rad(-1.604), math.rad(-1.604)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.256, -0.065, 0.245) * CFrame.Angles(math.rad(-30.08), math.rad(-43.717), math.rad(-19.939)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.044, -0.11, 0.124) * CFrame.Angles(math.rad(-31.914), math.rad(36.555), math.rad(31.627)),
					},
				},
			},
		},
		[6.3] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.199, -0.061, -0.011) * CFrame.Angles(math.rad(-2.865), math.rad(5.558), math.rad(-19.137)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.164, 0.233, -0.038) * CFrame.Angles(math.rad(-32.258), math.rad(4.813), math.rad(7.62)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.217, -0.05, -0.099) * CFrame.Angles(math.rad(-0.688), math.rad(5.73), math.rad(-1.604)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.256, -0.065, 0.245) * CFrame.Angles(math.rad(-33.575), math.rad(-42.743), math.rad(-17.647)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.044, -0.11, 0.124) * CFrame.Angles(math.rad(-31.914), math.rad(36.555), math.rad(27.215)),
					},
				},
			},
		},
		[6.4] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(0.067, -0.03, -0.006) * CFrame.Angles(math.rad(-2.406), math.rad(1.375), math.rad(-8.995)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.284, 0.314, -0.055) * CFrame.Angles(math.rad(-19.251), math.rad(8.079), math.rad(5.959)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.222, 0.057, -0.132) * CFrame.Angles(math.rad(-5.329), math.rad(-9.167), math.rad(-2.005)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.243, -0.12, 0.194) * CFrame.Angles(math.rad(-35.409), math.rad(-11.516), math.rad(-86.803)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.058, -0.078, 0.094) * CFrame.Angles(math.rad(-30.023), math.rad(22.173), math.rad(68.411)),
					},
				},
			},
		},
		[6.5] = {
			["HumanoidRootPart"] = {
				["Torso"] = {
					CFrame = CFrame.new(-0.007, -0.014, -0.005) * CFrame.Angles(math.rad(-1.432), math.rad(-0.802), math.rad(-4.985)),
					["Left Leg"] = {
						CFrame = CFrame.new(-0.102, 0.079, -0.034) * CFrame.Angles(math.rad(-6.016), math.rad(10.886), math.rad(3.667)),
					},
					["Right Leg"] = {
						CFrame = CFrame.new(0.099, 0.016, 0.021) * CFrame.Angles(math.rad(-2.235), math.rad(-2.063), math.rad(-1.261)),
					},
					["Left Arm"] = {
						CFrame = CFrame.new(0.072, -0.213, 0.073) * CFrame.Angles(math.rad(-8.824), math.rad(3.323), math.rad(-105.195)),
					},
					["Right Arm"] = {
						CFrame = CFrame.new(-0.04, -0.056, 0.041) * CFrame.Angles(math.rad(-16.616), math.rad(2.636), math.rad(81.99)),
					},
				},
			},
		},
	}
}

local number=0

LastTimeSetTotal=0
local savec0 = {}


GetAnimCF = function(limb,Time)
	
local GA = nil
	coroutine.resume(coroutine.create(function()

if limb == "Torso" then
GA = Anim.Keyframes[Time]["HumanoidRootPart"]["Torso"].CFrame
else
GA = Anim.Keyframes[Time]["HumanoidRootPart"]["Torso"][""..limb].CFrame

end
end))
return GA
end



local model = nil
if owner ~= nil then 
model = owner.Character
else
model = game:GetService("Players").localPlayer.Character
end
function GatherAllInstances(Parent)
	local Instances = {}
	local function GatherInstances(Parent)
		for i, v in pairs(Parent:GetChildren()) do
			GatherInstances(v)
			table.insert(Instances, v)
		end
	end
	GatherInstances(Parent)
	return Instances
end




for i, v in pairs(GatherAllInstances(model)) do
if v:IsA("BasePart") then 
for i, v2 in pairs(GatherAllInstances(model)) do
	if v2:IsA("Motor6D") and  v2.Part1.Name == v.Name then 

local saveCF = v2.C0
table.insert(savec0,{v2.Name,saveCF})

end
end
end end




RunAnim = function(Time)





	
local speed = Time-LastTimeSetTotal

speed = speed*AnimationSpeed
LastTimeSetTotal = Time	
	
	
	
	local doing = true
	
	coroutine.resume(coroutine.create(function()
for i, v in pairs(GatherAllInstances(model)) do
if v:IsA("BasePart") then 
for i, v2 in pairs(GatherAllInstances(model)) do
	if v2:IsA("Motor6D") and  v2.Part1.Name == v.Name then 

--print(v.Name)
local GotAnim = GetAnimCF(v.Name,Time)
--print(GotAnim)
local saveCF = nil
	 for i,v3 in pairs(savec0) do 
		if v2.name == v3[1] then
			saveCF = v3[2]
		end
	end

--print(saveCF)

if GotAnim ~= nil and saveCF ~= nil then
	
	
	coroutine.resume(coroutine.create(function()
while doing == true do
	swait()
	v2.C0 = v2.C0:lerp(saveCF*GotAnim,SmoothTime *speed)
end

end))
--v2.C0 = saveCF*GotAnim 


end

	end

end
end end


end))
wait(speed)
doing = false
end
while true do
RunAnim(0)	
RunAnim(0.1)	
RunAnim(0.2)
RunAnim(0.3)
RunAnim(0.4)
RunAnim(0.5)
RunAnim(0.6)
RunAnim(0.7)
RunAnim(0.8)
RunAnim(0.9)
RunAnim(1)
-----------
RunAnim(1.1)	
RunAnim(1.15)
RunAnim(1.2)	
RunAnim(1.3)
RunAnim(1.35)
RunAnim(1.4)
RunAnim(1.45)
RunAnim(1.5)
RunAnim(1.6)
RunAnim(1.7)
RunAnim(1.8)
RunAnim(1.9)
RunAnim(2)
-----------

RunAnim(2.1)	
RunAnim(2.2)
RunAnim(2.3)
RunAnim(2.4)
RunAnim(2.5)
RunAnim(2.6)
RunAnim(2.7)
RunAnim(2.8)
RunAnim(2.9)
RunAnim(3)
-----------

RunAnim(3.1)	
RunAnim(3.2)
RunAnim(3.3)
RunAnim(3.35)
RunAnim(3.4)
RunAnim(3.5)
RunAnim(3.6)
RunAnim(3.7)
RunAnim(3.8)
RunAnim(3.9)
RunAnim(4)
-----------

RunAnim(4.1)	
RunAnim(4.2)
RunAnim(4.3)
RunAnim(4.4)
RunAnim(4.5)
RunAnim(4.6)
RunAnim(4.7)
RunAnim(4.8)
RunAnim(4.9)
RunAnim(5)
-----------

RunAnim(5.1)	
RunAnim(5.2)
RunAnim(5.3)
RunAnim(5.4)
RunAnim(5.5)
RunAnim(5.6)
RunAnim(5.7)
RunAnim(5.8)
RunAnim(5.9)
RunAnim(6)
-----------
	
RunAnim(6.1)	
RunAnim(6.2)
RunAnim(6.3)
RunAnim(6.4)
RunAnim(6.5)
LastTimeSetTotal = .1
	Humanoid.MaxHealth = "inf"
	Humanoid.Health = "inf"
 	sick.SoundId = "rbxassetid://1881895904"
	sick.Looped = true
	sick.Pitch = 1
	sick.Volume = 10
	sick:Resume()
	sick.Parent = Torso
         refit()
end
--print(Anim.Keyframes[number]["HumanoidRootPart"]["Torso"]["Left Leg"].CFrame)("Clicked")
										end)


										local page = venyx:addPage("FE Script 2", 5012544693)
										local section2 = page:addSection("FE scripts 2")




										section2:addButton("FE Chips", function()
											HumanDied = false
local CountSCIFIMOVIELOL = 1
function SCIFIMOVIELOL(Part0,Part1,Position,Angle)
	local AlignPos = Instance.new('AlignPosition', Part1); AlignPos.Name = "AliP_"..CountSCIFIMOVIELOL
	AlignPos.ApplyAtCenterOfMass = true;
	AlignPos.MaxForce = 5772000--67752;
	AlignPos.MaxVelocity = math.huge/9e110;
	AlignPos.ReactionForceEnabled = false;
	AlignPos.Responsiveness = 200;
	AlignPos.RigidityEnabled = false;
	local AlignOri = Instance.new('AlignOrientation', Part1); AlignOri.Name = "AliO_"..CountSCIFIMOVIELOL
	AlignOri.MaxAngularVelocity = math.huge/9e110;
	AlignOri.MaxTorque = 5772000
	AlignOri.PrimaryAxisOnly = false;
	AlignOri.ReactionTorqueEnabled = false;
	AlignOri.Responsiveness = 200;
	AlignOri.RigidityEnabled = false;
	local AttachmentA=Instance.new('Attachment',Part1); AttachmentA.Name = "Ath_"..CountSCIFIMOVIELOL
	local AttachmentB=Instance.new('Attachment',Part0); AttachmentB.Name = "Ath_"..CountSCIFIMOVIELOL
	AttachmentA.Orientation = Angle or Vector3.new(0,0,0)
	AttachmentA.Position = Position or Vector3.new(0,0,0)
	AlignPos.Attachment1 = AttachmentA;
	AlignPos.Attachment0 = AttachmentB;
	AlignOri.Attachment1 = AttachmentA;
	AlignOri.Attachment0 = AttachmentB;
	CountSCIFIMOVIELOL = CountSCIFIMOVIELOL + 1
	return {AlignPos,AlignOri,AttachmentA,AttachmentB}
end

if _G.netted ~= true then
	_G.netted = true
	coroutine.wrap(function()
		settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
		settings().Physics.AllowSleep = false
		game:GetService("RunService").RenderStepped:Connect(function()
			game:FindFirstChildOfClass("Players").LocalPlayer.MaximumSimulationRadius=math.pow(math.huge,math.huge)
			sethiddenproperty(game:FindFirstChildOfClass("Players").LocalPlayer,"SimulationRadius",math.huge*math.huge)
		end)
	end)()
end

game:FindFirstChildOfClass("Players").LocalPlayer["Character"].Archivable = true
local hatnameclone = {}
for _,v in next, game:FindFirstChildOfClass("Players").LocalPlayer["Character"]:GetChildren() do
	if v:IsA("Accessory") then
		if hatnameclone[v.Name] then
			if hatnameclone[v.Name] == "s" then
				hatnameclone[v.Name] = {}
			end
			table.insert(hatnameclone[v.Name],v)
		else
			hatnameclone[v.Name] = "s"
		end
	end
end
for _,v in pairs(hatnameclone) do
	if type(v) == "table" then
		local num = 1
		for _,w in pairs(v) do
			w.Name = w.Name..num
			num = num + 1
		end
	end
end
hatnameclone = nil

local DeadChar = game:FindFirstChildOfClass("Players").LocalPlayer.Character

local fldr = Instance.new("Folder",game:FindFirstChildOfClass("Players").LocalPlayer["Character"])
fldr.Name = "DMYF"
local CloneChar = DeadChar:Clone()
local ANIMATIONHERE
if CloneChar:FindFirstChild("Animate") then
	ANIMATIONHERE = CloneChar:FindFirstChild("Animate"):Clone()
	CloneChar:FindFirstChild("Animate"):Destroy()
end
if CloneChar:FindFirstChildOfClass("Folder") then CloneChar:FindFirstChildOfClass("Folder"):Destroy() end
if CloneChar.Torso:FindFirstChild("Neck") then
	local Clonessss = CloneChar.Torso:FindFirstChild("Neck"):Clone()
	Clonessss.Part0 = nil
	Clonessss.Part1 = DeadChar.Head
	Clonessss.Parent = DeadChar.Torso
end
CloneChar.Parent = fldr
CloneChar.HumanoidRootPart.CFrame = DeadChar.HumanoidRootPart.CFrame
CloneChar.Humanoid.BreakJointsOnDeath = false
CloneChar.Name = "non"
CloneChar.Humanoid.DisplayDistanceType = "None"

for _,v in next, DeadChar:GetChildren() do
	if v:IsA("Accessory") then
		local topacc = false
		if v.Handle:FindFirstChildOfClass("Weld") then v.Handle:FindFirstChildOfClass("Weld"):Destroy() end
		v.Handle.Massless = true
		v.Handle.CanCollide = false
		if v.Handle:FindFirstChildOfClass("Attachment") then
			local ath__ = v.Handle:FindFirstChildOfClass("Attachment")
			if ath__.Name == "HatAttachment" or ath__.Name == "HairAttachment" or ath__.Name == "FaceFrontAttachment" or ath__.Name == "FaceCenterAttachment" then
				topacc = ath__.Name
			end
		end
        local bv = Instance.new("BodyVelocity",v.Handle)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			if topacc then
				local allthings = SCIFIMOVIELOL(v.Handle,DeadChar.Torso,Vector3.new(0,1.5,0)+ (DeadChar.Head[topacc].Position + (v.Handle[topacc].Position*-1)),Vector3.new(0,0,0))
				local normaltop = allthings[1].Attachment1
				local alipos = allthings[1]
				local alirot = allthings[2]
				local p0 = v.Handle
				local p1 = DeadChar.Head
				alipos.Parent = CloneChar:FindFirstChild(v.Name).Handle
				alirot.Parent = CloneChar:FindFirstChild(v.Name).Handle
				while true do
					game:GetService("RunService").RenderStepped:wait()
					if HumanDied then break end
					coroutine.wrap(function()
						if alipos.Attachment1 == normaltop then
							p0.CFrame = p0.CFrame:lerp((((DeadChar.Torso.CFrame * CFrame.new(0,1.5,0)) * p1[topacc].CFrame) * p0[topacc].CFrame:inverse()),1)
						else
							v.Handle.CFrame = v.Handle.CFrame:lerp(alipos.Attachment1.Parent.CFrame * CFrame.new(alipos.Attachment1.Position) * CFrame.Angles(math.rad(alipos.Attachment1.Rotation.X),math.rad(alipos.Attachment1.Rotation.Y),math.rad(alipos.Attachment1.Rotation.Z)),1)
						end
					end)()
				end
			else
				SCIFIMOVIELOL(v.Handle,CloneChar[v.Name].Handle,Vector3.new(0,0,0),Vector3.new(0,0,0))
			end
		end)()
    end
end

local a = DeadChar.Torso
local b = DeadChar.HumanoidRootPart
local c = DeadChar.Humanoid
a.Parent = game:FindFirstChildOfClass("Workspace")
c.Parent = game:FindFirstChildOfClass("Workspace")
local told = a:Clone()
local told1 = c:Clone()
b["RootJoint"].Part0 = told
b["RootJoint"].Part1 = DeadChar.Head
a.Name = "torso"
a.Neck:Destroy()
c.Name = "Mizt Hub Best"
told.Parent = DeadChar
told1.Parent = DeadChar
DeadChar.PrimaryPart = told
told1.Health = 0
b:Destroy()
a.Parent = DeadChar
c.Parent = DeadChar
told:Destroy()
told1:Destroy()
a.Name = "Torso"

if CloneChar.Head:FindFirstChildOfClass("Decal") then CloneChar.Head:FindFirstChildOfClass("Decal").Transparency = 1 end
if DeadChar:FindFirstChild("Animate") then DeadChar:FindFirstChild("Animate"):Destroy() end

local Collider
function UnCollide()
    if HumanDied then Collider:Disconnect(); return end
    --[[for _,Parts in next, CloneChar:GetChildren() do
        if Parts:IsA("BasePart") then
            Parts.CanCollide = false 
        end 
    end]]
    for _,Parts in next, DeadChar:GetChildren() do
        if Parts:IsA("BasePart") then
        Parts.CanCollide = false
        end 
    end 
end
Collider = game:GetService("RunService").Stepped:Connect(UnCollide)

local resetBindable = Instance.new("BindableEvent")
resetBindable.Event:connect(function()
    game:GetService("StarterGui"):SetCore("ResetButtonCallback", true)
	resetBindable:Destroy()
	HumanDied = true
    pcall(function()
		game:FindFirstChildOfClass("Players").LocalPlayer.Character = DeadChar
		DeadChar.Head:Destroy()
		DeadChar:FindFirstChildOfClass("Humanoid"):Destroy()
		game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
		if DeadChar:FindFirstChildOfClass("Folder") then DeadChar:FindFirstChildOfClass("Folder"):Destroy() end
	end)
end)
game:GetService("StarterGui"):SetCore("ResetButtonCallback", resetBindable)

coroutine.wrap(function()
    while true do
        game:GetService("RunService").RenderStepped:wait()
        if not CloneChar or not CloneChar:FindFirstChild("Head") or not CloneChar:FindFirstChildOfClass("Humanoid") or CloneChar:FindFirstChildOfClass("Humanoid").Health <= 0 and not DeadChar or not DeadChar:FindFirstChild("Head") or not DeadChar:FindFirstChildOfClass("Humanoid") or DeadChar:FindFirstChildOfClass("Humanoid").Health <= 0 then 
            HumanDied = true
            pcall(function()
				game:FindFirstChildOfClass("Players").LocalPlayer.Character = DeadChar
				DeadChar.Head:Destroy()
				DeadChar:FindFirstChildOfClass("Humanoid"):Destroy()
				game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
				if DeadChar:FindFirstChildOfClass("Folder") then DeadChar:FindFirstChildOfClass("Folder"):Destroy() end
			end)
            if resetBindable then
                game:GetService("StarterGui"):SetCore("ResetButtonCallback", true)
                resetBindable:Destroy()
            end
            break
        end		
    end
end)()


SCIFIMOVIELOL(DeadChar["Head"],CloneChar["Head"])
SCIFIMOVIELOL(DeadChar["Torso"],CloneChar["Torso"])
SCIFIMOVIELOL(DeadChar["Left Arm"],CloneChar["Left Arm"])
SCIFIMOVIELOL(DeadChar["Right Arm"],CloneChar["Right Arm"])
SCIFIMOVIELOL(DeadChar["Left Leg"],CloneChar["Left Leg"])
SCIFIMOVIELOL(DeadChar["Right Leg"],CloneChar["Right Leg"])

for _,v in pairs(DeadChar:GetChildren()) do
	if v:IsA("BasePart") and v.Name ~= "Head" then
		--[[local bv = Instance.new("BodyVelocity",v)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			while true do
				game:GetService("RunService").RenderStepped:wait()
				if HumanDied then break end
				v.CFrame = CloneChar[v.Name].CFrame
			end
		end)()]]
	elseif v:IsA("BasePart") and v.Name == "Head" then
		local bv = Instance.new("BodyVelocity",v)
		bv.Velocity = Vector3.new(0,0,0)
		coroutine.wrap(function()
			while true do
				game:GetService("RunService").RenderStepped:wait()
				if HumanDied then break end
				v.CFrame = DeadChar.Torso.CFrame * CFrame.new(0,1.5,0)
			end
		end)()
	end
end

for _,BodyParts in next, CloneChar:GetDescendants() do
if BodyParts:IsA("BasePart") or BodyParts:IsA("Part") then
BodyParts.Transparency = 1 end end
game:GetService("RunService").RenderStepped:wait()
game:FindFirstChildOfClass("Players").LocalPlayer.Character = CloneChar
game:FindFirstChildOfClass("Workspace"):FindFirstChildOfClass("Camera").CameraSubject = CloneChar.Humanoid

for _,v in next, DeadChar:GetChildren() do
	if v:IsA("Accessory") then
		if v.Handle:FindFirstChildOfClass("Weld") then v.Handle:FindFirstChildOfClass("Weld"):Destroy() end
	end
end

if ANIMATIONHERE then ANIMATIONHERE.Parent = CloneChar end

-----------------------
--[[ Name : Chips ]]--
--[[ Description : I think I found my specialty in scripts ]]--
--[[ \ None / ]]--
-------------------------------------------------------
--A script By Creterisk/makhail07
--Discord Creterisk#2958 
-------------------------------------------------------

--Everything is Meaningless.....

wait(1 / 60)

loadstring(game:GetObjects("rbxassetid://5425999987")[1].Source)()

local plr = game.Players.LocalPlayer
local mouse = plr:GetMouse()
local char = plr.Character
local hum = char:FindFirstChildOfClass'Humanoid'
local hed = char.Head
local root = char:FindFirstChild'HumanoidRootPart'
local rootj = root.RootJoint
local tors = char.Torso
local ra = char["Right Arm"]
local la = char["Left Arm"]
local rl = char["Right Leg"]
local ll = char["Left Leg"]
local neck = tors["Neck"]
local RootCF = CFrame.fromEulerAnglesXYZ(-1.57, 0, 3.14)
local RHCF = CFrame.fromEulerAnglesXYZ(0, 1.6, 0)
local LHCF = CFrame.fromEulerAnglesXYZ(0, -1.6, 0)
local maincolor = BrickColor.new("Institutional white")
-------------------------------------------------------
--Start Good Stuff--
-------------------------------------------------------
cam = game.Workspace.CurrentCamera
CF = CFrame.new
angles = CFrame.Angles
attack = false
Euler = CFrame.fromEulerAnglesXYZ
Rad = math.rad
IT = Instance.new
BrickC = BrickColor.new
Cos = math.cos
Acos = math.acos
Sin = math.sin
Asin = math.asin
Abs = math.abs
Mrandom = math.random
Floor = math.floor
-------------------------------------------------------
--End Good Stuff--
-------------------------------------------------------
necko = CF(0, 1, 0, -1, -0, -0, 0, 0, 1, 0, 1, 0)
RSH, LSH = nil, nil 
RW = Instance.new("Weld") 
LW = Instance.new("Weld")
RH = tors["Right Hip"]
LH = tors["Left Hip"]
RSH = tors["Right Shoulder"] 
LSH = tors["Left Shoulder"] 
RSH.Parent = nil 
LSH.Parent = nil 
RW.Name = "RW"
RW.Part0 = tors 
RW.C0 = CF(1.5, 0.5, 0)
RW.C1 = CF(0, 0.5, 0) 
RW.Part1 = ra
RW.Parent = tors 
LW.Name = "LW"
LW.Part0 = tors 
LW.C0 = CF(-1.5, 0.5, 0)
LW.C1 = CF(0, 0.5, 0) 
LW.Part1 = la
LW.Parent = tors
Effects = {}
newWeld = function(wp0, wp1, wc0x, wc0y, wc0z)
    local wld = Instance.new("Weld", wp1)
    wld.Part0 = wp0
    wld.Part1 = wp1
    wld.C0 = CFrame.new(wc0x, wc0y, wc0z)
end
newWeld(tors, ll, -0.5, -1, 0)
ll.Weld.C1 = CFrame.new(0, 1, 0)
newWeld(tors, rl, 0.5, -1, 0)
rl.Weld.C1 = CFrame.new(0, 1, 0)
-------------------------------------------------------
--Start Important Functions--
-------------------------------------------------------
function swait(num)
	if num == 0 or num == nil then
		game:service("RunService").Stepped:wait(0)
	else
		for i = 0, num do
			game:service("RunService").Stepped:wait(0)
		end
	end
end
function thread(f)
	coroutine.resume(coroutine.create(f))
end
function clerp(a, b, t)
	local qa = {
		QuaternionFromCFrame(a)
	}
	local qb = {
		QuaternionFromCFrame(b)
	}
	local ax, ay, az = a.x, a.y, a.z
	local bx, by, bz = b.x, b.y, b.z
	local _t = 1 - t
	return QuaternionToCFrame(_t * ax + t * bx, _t * ay + t * by, _t * az + t * bz, QuaternionSlerp(qa, qb, t))
end
function QuaternionFromCFrame(cf)
	local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components()
	local trace = m00 + m11 + m22
	if trace > 0 then
		local s = math.sqrt(1 + trace)
		local recip = 0.5 / s
		return (m21 - m12) * recip, (m02 - m20) * recip, (m10 - m01) * recip, s * 0.5
	else
		local i = 0
		if m00 < m11 then
			i = 1
		end
		if m22 > (i == 0 and m00 or m11) then
			i = 2
		end
		if i == 0 then
			local s = math.sqrt(m00 - m11 - m22 + 1)
			local recip = 0.5 / s
			return 0.5 * s, (m10 + m01) * recip, (m20 + m02) * recip, (m21 - m12) * recip
		elseif i == 1 then
			local s = math.sqrt(m11 - m22 - m00 + 1)
			local recip = 0.5 / s
			return (m01 + m10) * recip, 0.5 * s, (m21 + m12) * recip, (m02 - m20) * recip
		elseif i == 2 then
			local s = math.sqrt(m22 - m00 - m11 + 1)
			local recip = 0.5 / s
			return (m02 + m20) * recip, (m12 + m21) * recip, 0.5 * s, (m10 - m01) * recip
		end
	end
end
function QuaternionToCFrame(px, py, pz, x, y, z, w)
	local xs, ys, zs = x + x, y + y, z + z
	local wx, wy, wz = w * xs, w * ys, w * zs
	local xx = x * xs
	local xy = x * ys
	local xz = x * zs
	local yy = y * ys
	local yz = y * zs
	local zz = z * zs
	return CFrame.new(px, py, pz, 1 - (yy + zz), xy - wz, xz + wy, xy + wz, 1 - (xx + zz), yz - wx, xz - wy, yz + wx, 1 - (xx + yy))
end
function QuaternionSlerp(a, b, t)
	local cosTheta = a[1] * b[1] + a[2] * b[2] + a[3] * b[3] + a[4] * b[4]
	local startInterp, finishInterp
	if cosTheta >= 1.0E-4 then
		if 1 - cosTheta > 1.0E-4 then
			local theta = math.acos(cosTheta)
			local invSinTheta = 1 / Sin(theta)
			startInterp = Sin((1 - t) * theta) * invSinTheta
			finishInterp = Sin(t * theta) * invSinTheta
		else
			startInterp = 1 - t
			finishInterp = t
		end
	elseif 1 + cosTheta > 1.0E-4 then
		local theta = math.acos(-cosTheta)
		local invSinTheta = 1 / Sin(theta)
		startInterp = Sin((t - 1) * theta) * invSinTheta
		finishInterp = Sin(t * theta) * invSinTheta
	else
		startInterp = t - 1
		finishInterp = t
	end
	return a[1] * startInterp + b[1] * finishInterp, a[2] * startInterp + b[2] * finishInterp, a[3] * startInterp + b[3] * finishInterp, a[4] * startInterp + b[4] * finishInterp
end
function rayCast(Position, Direction, Range, Ignore)
	return game:service("Workspace"):FindPartOnRay(Ray.new(Position, Direction.unit * (Range or 999.999)), Ignore)
end
local RbxUtility = LoadLibrary("RbxUtility")
local Create = RbxUtility.Create

-------------------------------------------------------
--Start Damage Function--
-------------------------------------------------------
function Damage(Part, hit, minim, maxim, knockback, Type, Property, Delay, HitSound, HitPitch)
	return true
end
-------------------------------------------------------
--End Damage Function--
-------------------------------------------------------

-------------------------------------------------------
--Start Damage Function Customization--
-------------------------------------------------------
function ShowDamage(Pos, Text, Time, Color)
	local Rate = (1 / 30)
	local Pos = (Pos or Vector3.new(0, 0, 0))
	local Text = (Text or "")
	local Time = (Time or 2)
	local Color = (Color or Color3.new(1, 0, 1))
	local EffectPart = CFuncs.Part.Create(workspace, "SmoothPlastic", 0, 1, BrickColor.new(Color), "Effect", Vector3.new(0, 0, 0))
	EffectPart.Anchored = true
	local BillboardGui = Create("BillboardGui"){
		Size = UDim2.new(3, 0, 3, 0),
		Adornee = EffectPart,
		Parent = EffectPart,
	}
	local TextLabel = Create("TextLabel"){
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 1, 0),
		Text = Text,
		Font = "Bodoni",
		TextColor3 = Color,
		TextScaled = true,
		TextStrokeColor3 = Color3.fromRGB(0,0,0),
		Parent = BillboardGui,
	}
	game.Debris:AddItem(EffectPart, (Time))
	EffectPart.Parent = game:GetService("Workspace")
	delay(0, function()
		local Frames = (Time / Rate)
		for Frame = 1, Frames do
			wait(Rate)
			local Percent = (Frame / Frames)
			EffectPart.CFrame = CFrame.new(Pos) + Vector3.new(0, Percent, 0)
			TextLabel.TextTransparency = Percent
		end
		if EffectPart and EffectPart.Parent then
			EffectPart:Destroy()
		end
	end)
end
-------------------------------------------------------
--End Damage Function Customization--
-------------------------------------------------------

function MagniDamage(Part, magni, mindam, maxdam, knock, Type)
  for _, c in pairs(workspace:children()) do
    local hum = c:findFirstChild("Humanoid")
    if hum ~= nil then
      local head = c:findFirstChild("Head")
      if head ~= nil then
        local targ = head.Position - Part.Position
        local mag = targ.magnitude
        if magni >= mag and c.Name ~= plr.Name then
          Damage(head, head, mindam, maxdam, knock, Type, root, 0.1, "http://www.roblox.com/asset/?id=0", 1.2)
        end
      end
    end
  end
end


CFuncs = {
	Part = {
		Create = function(Parent, Material, Reflectance, Transparency, BColor, Name, Size)
			local Part = Create("Part")({
				Parent = Parent,
				Reflectance = Reflectance,
				Transparency = Transparency,
				CanCollide = false,
				Locked = true,
				BrickColor = BrickColor.new(tostring(BColor)),
				Name = Name,
				Size = Size,
				Material = Material
			})
			RemoveOutlines(Part)
			return Part
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Weld = {
		Create = function(Parent, Part0, Part1, C0, C1)
			local Weld = Create("Weld")({
				Parent = Parent,
				Part0 = Part0,
				Part1 = Part1,
				C0 = C0,
				C1 = C1
			})
			return Weld
		end
	},
	Sound = {
		Create = function(id, par, vol, pit)
			coroutine.resume(coroutine.create(function()
				local S = Create("Sound")({
					Volume = vol,
					Pitch = pit or 1,
					SoundId = id,
					Parent = par or workspace
				})
				wait()
				S:play()
				game:GetService("Debris"):AddItem(S, 6)
			end))
		end
	},
	ParticleEmitter = {
		Create = function(Parent, Color1, Color2, LightEmission, Size, Texture, Transparency, ZOffset, Accel, Drag, LockedToPart, VelocityInheritance, EmissionDirection, Enabled, LifeTime, Rate, Rotation, RotSpeed, Speed, VelocitySpread)
			local fp = Create("ParticleEmitter")({
				Parent = Parent,
				Color = ColorSequence.new(Color1, Color2),
				LightEmission = LightEmission,
				Size = Size,
				Texture = Texture,
				Transparency = Transparency,
				ZOffset = ZOffset,
				Acceleration = Accel,
				Drag = Drag,
				LockedToPart = LockedToPart,
				VelocityInheritance = VelocityInheritance,
				EmissionDirection = EmissionDirection,
				Enabled = Enabled,
				Lifetime = LifeTime,
				Rate = Rate,
				Rotation = Rotation,
				RotSpeed = RotSpeed,
				Speed = Speed,
				VelocitySpread = VelocitySpread
			})
			return fp
		end
	}
}
function RemoveOutlines(part)
	part.TopSurface, part.BottomSurface, part.LeftSurface, part.RightSurface, part.FrontSurface, part.BackSurface = 10, 10, 10, 10, 10, 10
end
function CreatePart(FormFactor, Parent, Material, Reflectance, Transparency, BColor, Name, Size)
	local Part = Create("Part")({
		formFactor = FormFactor,
		Parent = Parent,
		Reflectance = Reflectance,
		Transparency = Transparency,
		CanCollide = false,
		Locked = true,
		BrickColor = BrickColor.new(tostring(BColor)),
		Name = Name,
		Size = Size,
		Material = Material
	})
	RemoveOutlines(Part)
	return Part
end
function CreateMesh(Mesh, Part, MeshType, MeshId, OffSet, Scale)
	local Msh = Create(Mesh)({
		Parent = Part,
		Offset = OffSet,
		Scale = Scale
	})
	if Mesh == "SpecialMesh" then
		Msh.MeshType = MeshType
		Msh.MeshId = MeshId
	end
	return Msh
end
function CreateWeld(Parent, Part0, Part1, C0, C1)
	local Weld = Create("Weld")({
		Parent = Parent,
		Part0 = Part0,
		Part1 = Part1,
		C0 = C0,
		C1 = C1
	})
	return Weld
end


-------------------------------------------------------
--Start Effect Function--
-------------------------------------------------------
EffectModel = Instance.new("Model", char)
Effects = {
  Block = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay, Type)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("BlockMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      if Type == 1 or Type == nil then
        table.insert(Effects, {
          prt,
          "Block1",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      elseif Type == 2 then
        table.insert(Effects, {
          prt,
          "Block2",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      else
        table.insert(Effects, {
          prt,
          "Block3",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      end
    end
  },
  Sphere = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Cylinder = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("CylinderMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Wave = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://20329976", Vector3.new(0, 0, 0), Vector3.new(x1 / 60, y1 / 60, z1 / 60))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3 / 60,
        y3 / 60,
        z3 / 60,
        msh
      })
    end
  },
  Ring = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://3270017", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Break = {
    Create = function(brickcolor, cframe, x1, y1, z1)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new(0.5, 0.5, 0.5))
      prt.Anchored = true
      prt.CFrame = cframe * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      local num = math.random(10, 50) / 1000
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Shatter",
        num,
        prt.CFrame,
        math.random() - math.random(),
        0,
        math.random(50, 100) / 100
      })
    end
  },
Spiral = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://1051557", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
Push = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://437347603", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  }
}
function part(formfactor ,parent, reflectance, transparency, brickcolor, name, size)
	local fp = IT("Part")
	fp.formFactor = formfactor 
	fp.Parent = parent
	fp.Reflectance = reflectance
	fp.Transparency = transparency
	fp.CanCollide = false 
	fp.Locked = true
	fp.BrickColor = brickcolor
	fp.Name = name
	fp.Size = size
	fp.Position = tors.Position 
	RemoveOutlines(fp)
	fp.Material = "SmoothPlastic"
	fp:BreakJoints()
	return fp 
end 
 
function mesh(Mesh,part,meshtype,meshid,offset,scale)
	local mesh = IT(Mesh) 
	mesh.Parent = part
	if Mesh == "SpecialMesh" then
		mesh.MeshType = meshtype
	if meshid ~= "nil" then
		mesh.MeshId = "http://www.roblox.com/asset/?id="..meshid
		end
	end
	mesh.Offset = offset
	mesh.Scale = scale
	return mesh
end

function Magic(bonuspeed, type, pos, scale, value, color, MType)
	local type = type
	local rng = Instance.new("Part", char)
	rng.Anchored = true
	rng.BrickColor = color
	rng.CanCollide = false
	rng.FormFactor = 3
	rng.Name = "Ring"
	rng.Material = "Neon"
	rng.Size = Vector3.new(1, 1, 1)
	rng.Transparency = 0
	rng.TopSurface = 0
	rng.BottomSurface = 0
	rng.CFrame = pos
	local rngm = Instance.new("SpecialMesh", rng)
	rngm.MeshType = MType
	rngm.Scale = scale
	local scaler2 = 1
	if type == "Add" then
		scaler2 = 1 * value
	elseif type == "Divide" then
		scaler2 = 1 / value
	end
	coroutine.resume(coroutine.create(function()
		for i = 0, 10 / bonuspeed, 0.1 do
			swait()
			if type == "Add" then
				scaler2 = scaler2 - 0.01 * value / bonuspeed
			elseif type == "Divide" then
				scaler2 = scaler2 - 0.01 / value * bonuspeed
			end
			rng.Transparency = rng.Transparency + 0.01 * bonuspeed
			rngm.Scale = rngm.Scale + Vector3.new(scaler2 * bonuspeed, scaler2 * bonuspeed, scaler2 * bonuspeed)
		end
		rng:Destroy()
	end))
end

function Eviscerate(dude)
	if dude.Name ~= char then
		local bgf = IT("BodyGyro", dude.Head)
		bgf.CFrame = bgf.CFrame * CFrame.fromEulerAnglesXYZ(Rad(-90), 0, 0)
		local val = IT("BoolValue", dude)
		val.Name = "IsHit"
		local ds = coroutine.wrap(function()
			dude:WaitForChild("Head"):BreakJoints()
			wait(0.5)
			target = nil
			coroutine.resume(coroutine.create(function()
				for i, v in pairs(dude:GetChildren()) do
					if v:IsA("Accessory") then
						v:Destroy()
					end
					if v:IsA("Humanoid") then
						v:Destroy()
					end
					if v:IsA("CharacterMesh") then
						v:Destroy()
					end
					if v:IsA("Model") then
						v:Destroy()
					end
					if v:IsA("Part") or v:IsA("MeshPart") then
						for x, o in pairs(v:GetChildren()) do
							if o:IsA("Decal") then
								o:Destroy()
							end
						end
						coroutine.resume(coroutine.create(function()
							v.Material = "Neon"
							v.CanCollide = false
							local PartEmmit1 = IT("ParticleEmitter", v)
							PartEmmit1.LightEmission = 1
							PartEmmit1.Texture = "rbxassetid://284205403"
							PartEmmit1.Color = ColorSequence.new(maincolor.Color)
							PartEmmit1.Rate = 150
							PartEmmit1.Lifetime = NumberRange.new(1)
							PartEmmit1.Size = NumberSequence.new({
								NumberSequenceKeypoint.new(0, 0.75, 0),
								NumberSequenceKeypoint.new(1, 0, 0)
							})
							PartEmmit1.Transparency = NumberSequence.new({
								NumberSequenceKeypoint.new(0, 0, 0),
								NumberSequenceKeypoint.new(1, 1, 0)
							})
							PartEmmit1.Speed = NumberRange.new(0, 0)
							PartEmmit1.VelocitySpread = 30000
							PartEmmit1.Rotation = NumberRange.new(-500, 500)
							PartEmmit1.RotSpeed = NumberRange.new(-500, 500)
							local BodPoss = IT("BodyPosition", v)
							BodPoss.P = 3000
							BodPoss.D = 1000
							BodPoss.maxForce = Vector3.new(50000000000, 50000000000, 50000000000)
							BodPoss.position = v.Position + Vector3.new(Mrandom(-15, 15), Mrandom(-15, 15), Mrandom(-15, 15))
							v.Color = maincolor.Color
							coroutine.resume(coroutine.create(function()
								for i = 0, 49 do
									swait(1)
									v.Transparency = v.Transparency + 0.08
								end
								wait(0.5)
								PartEmmit1.Enabled = false
								wait(3)
								v:Destroy()
								dude:Destroy()
							end))
						end))
					end
				end
			end))
		end)
		ds()
	end
end

function FindNearestHead(Position, Distance, SinglePlayer)
	if SinglePlayer then
		return Distance > (SinglePlayer.Torso.CFrame.p - Position).magnitude
	end
	local List = {}
	for i, v in pairs(workspace:GetChildren()) do
		if v:IsA("Model") and v:findFirstChild("Head") and v ~= char and Distance >= (v.Head.Position - Position).magnitude then
			table.insert(List, v)
		end
	end
	return List
end

function Aura(bonuspeed, FastSpeed, type, pos, x1, y1, z1, value, color, outerpos, MType)
	local type = type
	local rng = Instance.new("Part", char)
	rng.Anchored = true
	rng.BrickColor = color
	rng.CanCollide = false
	rng.FormFactor = 3
	rng.Name = "Ring"
	rng.Material = "Neon"
	rng.Size = Vector3.new(1, 1, 1)
	rng.Transparency = 0
	rng.TopSurface = 0
	rng.BottomSurface = 0
	rng.CFrame = pos
	rng.CFrame = rng.CFrame + rng.CFrame.lookVector * outerpos
	local rngm = Instance.new("SpecialMesh", rng)
	rngm.MeshType = MType
	rngm.Scale = Vector3.new(x1, y1, z1)
	local scaler2 = 1
	local speeder = FastSpeed
	if type == "Add" then
		scaler2 = 1 * value
	elseif type == "Divide" then
		scaler2 = 1 / value
	end
	coroutine.resume(coroutine.create(function()
		for i = 0, 10 / bonuspeed, 0.1 do
			swait()
			if type == "Add" then
				scaler2 = scaler2 - 0.01 * value / bonuspeed
			elseif type == "Divide" then
				scaler2 = scaler2 - 0.01 / value * bonuspeed
			end
			speeder = speeder - 0.01 * FastSpeed * bonuspeed
			rng.CFrame = rng.CFrame + rng.CFrame.lookVector * speeder * bonuspeed
			rng.Transparency = rng.Transparency + 0.01 * bonuspeed
			rngm.Scale = rngm.Scale + Vector3.new(scaler2 * bonuspeed, scaler2 * bonuspeed, 0)
		end
		rng:Destroy()
	end))
end

function SoulSteal(dude)
if dude.Name ~= char then
local bgf = IT("BodyGyro", dude.Head)
bgf.CFrame = bgf.CFrame * CFrame.fromEulerAnglesXYZ(Rad(-90), 0, 0)
local val = IT("BoolValue", dude)
val.Name = "IsHit"
local torso = (dude:FindFirstChild'Head' or dude:FindFirstChild'Torso' or dude:FindFirstChild'UpperTorso' or dude:FindFirstChild'LowerTorso' or dude:FindFirstChild'HumanoidRootPart')
local soulst = coroutine.wrap(function()
local soul = Instance.new("Part",dude)
soul.Size = Vector3.new(1,1,1)
soul.CanCollide = false
soul.Anchored = false
soul.Position = torso.Position
soul.Transparency = 1
local PartEmmit1 = IT("ParticleEmitter", soul)
PartEmmit1.LightEmission = 1
PartEmmit1.Texture = "rbxassetid://569507414"
PartEmmit1.Color = ColorSequence.new(maincolor.Color)
PartEmmit1.Rate = 250
PartEmmit1.Lifetime = NumberRange.new(1.6)
PartEmmit1.Size = NumberSequence.new({
	NumberSequenceKeypoint.new(0, 1, 0),
	NumberSequenceKeypoint.new(1, 0, 0)
})
PartEmmit1.Transparency = NumberSequence.new({
	NumberSequenceKeypoint.new(0, 0, 0),
	NumberSequenceKeypoint.new(1, 1, 0)
})
PartEmmit1.Speed = NumberRange.new(0, 0)
PartEmmit1.VelocitySpread = 30000
PartEmmit1.Rotation = NumberRange.new(-360, 360)
PartEmmit1.RotSpeed = NumberRange.new(-360, 360)
local BodPoss = IT("BodyPosition", soul)
BodPoss.P = 3000
BodPoss.D = 1000
BodPoss.maxForce = Vector3.new(50000000000, 50000000000, 50000000000)
BodPoss.position = torso.Position + Vector3.new(Mrandom(-15, 15), Mrandom(-15, 15), Mrandom(-15, 15))
wait(1.6)
soul.Touched:connect(function(hit)
	if hit.Parent == char then
	soul:Destroy()
	end
end)
wait(1.2)
while soul do
	swait()
	PartEmmit1.Color = ColorSequence.new(maincolor.Color)
	BodPoss.Position = tors.Position
end
end)
	soulst()
	end
end
function FaceMouse()
local	Cam = workspace.CurrentCamera
	return {
		CFrame.new(char.Torso.Position, Vector3.new(mouse.Hit.p.x, char.Torso.Position.y, mouse.Hit.p.z)),
		Vector3.new(mouse.Hit.p.x, mouse.Hit.p.y, mouse.Hit.p.z)
	}
end
Effects = {
	Block = function(cf,partsize,meshstart,meshadd,matr,colour,spin,inverse,factor)
	local p = Instance.new("Part",EffectModel)
	p.BrickColor = BrickColor.new(colour)
	p.Size = partsize
	p.Anchored = true
	p.CanCollide = false
	p.Material = matr
	p.CFrame = cf
	if inverse == true then
		p.Transparency = 1
	else
		p.Transparency = 0
	end
	local m = Instance.new("BlockMesh",p)
	m.Scale = meshstart
	coroutine.wrap(function()
		for i = 0, 1, factor do
			swait()
			if inverse == true then
				p.Transparency = 1-i
			else
				p.Transparency = i
			end
			m.Scale = m.Scale + meshadd
			if spin == true then
				p.CFrame = p.CFrame * CFrame.Angles(math.random(-50,50),math.random(-50,50),math.random(-50,50))
			end
		end
		p:Destroy()
	end)()
return p
	end,
	Sphere = function(cf,partsize,meshstart,meshadd,matr,colour,inverse,factor)
	local p = Instance.new("Part",EffectModel)
	p.BrickColor = BrickColor.new(colour)
	p.Size = partsize
	p.Anchored = true
	p.CanCollide = false
	p.Material = matr
	p.CFrame = cf
	if inverse == true then
		p.Transparency = 1
	else
		p.Transparency = 0
	end
	local m = Instance.new("SpecialMesh",p)
	m.MeshType = "Sphere"
	m.Scale = meshstart
	coroutine.wrap(function()
		for i=0,1,factor do
			swait()
			if inverse == true then
				p.Transparency = 1-i
			else
				p.Transparency = i
			end
			m.Scale = m.Scale + meshadd
		end
	p:Destroy()
end)()
return p
	end,

	Cylinder = function(cf,partsize,meshstart,meshadd,matr,colour,inverse,factor)
	local p = Instance.new("Part",EffectModel)
	p.BrickColor = BrickColor.new(colour)
	p.Size = partsize
	p.Anchored = true
	p.CanCollide = false
	p.Material = matr
	p.CFrame = cf
	if inverse == true then
		p.Transparency = 1
	else
		p.Transparency = 0
	end
	local m = Instance.new("CylinderMesh",p)
	m.Scale = meshstart
	coroutine.wrap(function()
		for i=0,1,factor do
			swait()
			if inverse == true then
				p.Transparency = 1-i
			else
				p.Transparency = i
			end
			m.Scale = m.Scale + meshadd
		end
	p:Destroy()
end)()
return p
	end,

Wave = function(cf,meshstart,meshadd,colour,spin,inverse,factor)
local p = Instance.new("Part",EffectModel)
p.BrickColor = BrickColor.new(colour)
p.Size = Vector3.new()
p.Anchored = true
p.CanCollide = false
p.CFrame = cf
if inverse == true then
p.Transparency = 1
else
p.Transparency = 0
end
local m = Instance.new("SpecialMesh",p)
m.MeshId = "rbxassetid://20329976"
m.Scale = meshstart
coroutine.wrap(function()
for i=0,1,factor do
swait()
if inverse == true then
p.Transparency = 1-i
else
p.Transparency = i
end
m.Scale = m.Scale + meshadd
p.CFrame = p.CFrame * CFrame.Angles(0,math.rad(spin),0)
end
p:Destroy()
end)()
return p
end,

Ring = function(cf,meshstart,meshadd,colour,inverse,factor)
local p = Instance.new("Part",EffectModel)
p.BrickColor = BrickColor.new(colour)
p.Size = Vector3.new()
p.Anchored = true
p.CanCollide = false
p.CFrame = cf
if inverse == true then
p.Transparency = 1
else
p.Transparency = 0
end
local m = Instance.new("SpecialMesh",p)
m.MeshId = "rbxassetid://3270017"
m.Scale = meshstart
coroutine.wrap(function()
for i=0,1,factor do
swait()
if inverse == true then
p.Transparency = 1-i
else
p.Transparency = i
end
m.Scale = m.Scale + meshadd
end
p:Destroy()
end)()
return p
end,

Meshed = function(cf,meshstart,meshadd,colour,meshid,textid,spin,inverse,factor)
local p = Instance.new("Part",EffectModel)
p.BrickColor = BrickColor.new(colour)
p.Size = Vector3.new()
p.Anchored = true
p.CanCollide = false
p.CFrame = cf
if inverse == true then
p.Transparency = 1
else
p.Transparency = 0
end
local m = Instance.new("SpecialMesh",p)
m.MeshId = meshid
m.TextureId = textid
m.Scale = meshstart
coroutine.wrap(function()
for i=0,1,factor do
swait()
if inverse == true then
p.Transparency = 1-i
else
p.Transparency = i
end
m.Scale = m.Scale + meshadd
p.CFrame = p.CFrame * CFrame.Angles(0,math.rad(spin),0)
end
p:Destroy()
end)()
return p
end,

Explode = function(cf,partsize,meshstart,meshadd,matr,colour,move,inverse,factor)
local p = Instance.new("Part",EffectModel)
p.BrickColor = BrickColor.new(colour)
p.Size = partsize
p.Anchored = true
p.CanCollide = false
p.Material = matr
p.CFrame = cf * CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360)))
if inverse == true then
p.Transparency = 1
else
p.Transparency = 0
end
local m = Instance.new("SpecialMesh",p)
m.MeshType = "Sphere"
m.Scale = meshstart
coroutine.wrap(function()
for i=0,1,factor do
swait()
if inverse == true then
p.Transparency = 1-i
else
p.Transparency = i
end
m.Scale = m.Scale + meshadd
p.CFrame = p.CFrame * CFrame.new(0,move,0)
end
p:Destroy()
end)()
return p
end,

}
-------------------------------------------------------
--End Effect Function--
-------------------------------------------------------
function Cso(ID, PARENT, VOLUME, PITCH)
	local NSound = nil
	coroutine.resume(coroutine.create(function()
		NSound = IT("Sound", PARENT)
		NSound.Volume = VOLUME
		NSound.Pitch = PITCH
		NSound.SoundId = "http://www.roblox.com/asset/?id="..ID
		swait()
		NSound:play()
		game:GetService("Debris"):AddItem(NSound, 10)
	end))
	return NSound
end
function CamShake(Length, Intensity)
	coroutine.resume(coroutine.create(function()
		local intensity = 1 * Intensity
		local rotM = 0.01 * Intensity
		for i = 0, Length, 0.1 do
			swait()
			intensity = intensity - 0.05 * Intensity / Length
			rotM = rotM - 5.0E-4 * Intensity / Length
			hum.CameraOffset = Vector3.new(Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)))
			cam.CFrame = cam.CFrame * CF(Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity))) * Euler(Rad(Mrandom(-intensity, intensity)) * rotM, Rad(Mrandom(-intensity, intensity)) * rotM, Rad(Mrandom(-intensity, intensity)) * rotM)
		end
		hum.CameraOffset = Vector3.new(0, 0, 0)
	end))
end
NewInstance = function(instance,parent,properties)
	local inst = Instance.new(instance)
	inst.Parent = parent
	if(properties)then
		for i,v in next, properties do
			pcall(function() inst[i] = v end)
		end
	end
	return inst;
end
hum.MaxHealth = 1.0E298
hum.Health = 1.0E298
game:GetService("RunService"):BindToRenderStep("HOT", 0, function()
  if hum.Health > 0.1 and hum.Health < 1.0E298 then
    hum.MaxHealth = 1.0E298
    hum.Health = 1.0E298
  end
end)
-------------------------------------------------------
--End Important Functions--
-------------------------------------------------------


-------------------------------------------------------
--Start Customization--
-------------------------------------------------------
local Player_Size = 1
if Player_Size ~= 1 then
root.Size = root.Size * Player_Size
tors.Size = tors.Size * Player_Size
hed.Size = hed.Size * Player_Size
ra.Size = ra.Size * Player_Size
la.Size = la.Size * Player_Size
rl.Size = rl.Size * Player_Size
ll.Size = ll.Size * Player_Size
----------------------------------------------------------------------------------
rootj.Parent = root
neck.Parent = tors
RW.Parent = tors
LW.Parent = tors
RH.Parent = tors
LH.Parent = tors
----------------------------------------------------------------------------------
rootj.C0 = RootCF * CF(0 * Player_Size, 0 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0))
rootj.C1 = RootCF * CF(0 * Player_Size, 0 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0))
neck.C0 = necko * CF(0 * Player_Size, 0 * Player_Size, 0 + ((1 * Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0))
neck.C1 = CF(0 * Player_Size, -0.5 * Player_Size, 0 * Player_Size) * angles(Rad(-90), Rad(0), Rad(180))
RW.C0 = CF(1.5 * Player_Size, 0.5 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0)) --* RIGHTSHOULDERC0
LW.C0 = CF(-1.5 * Player_Size, 0.5 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0)) --* LEFTSHOULDERC0
----------------------------------------------------------------------------------
RH.C0 = CF(1 * Player_Size, -1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
LH.C0 = CF(-1 * Player_Size, -1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(-90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
RH.C1 = CF(0.5 * Player_Size, 1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
LH.C1 = CF(-0.5 * Player_Size, 1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(-90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
--hat.Parent = Character
end
----------------------------------------------------------------------------------
local SONG = 525565668
local SONG2 = 0
local Music = Instance.new("Sound",tors)
Music.Volume = 2.5
Music.Looped = true
Music.Pitch = 1 --Pitcher
----------------------------------------------------------------------------------
local equipped = false
local idle = 0
local change = 1
local val = 0
local toim = 0
local idleanim = 0.4
local sine = 0
local Sit = 1
local WasAir = false
local InAir = false
local LandTick = 0
local movelegs = false
local FF = Instance.new("ForceField",char)
FF.Visible = false
local Speed = 56
local Chips = "onebearnakedwoman"
----------------------------------------------------------------------------------
hum.JumpPower = 55
hum.Animator.Parent = nil
----------------------------------------------------------------------------------
Chips = IT("Model")
Chips.Parent = char
Chips.Name = "Chips"
RHe = IT("Part")
RHe.Parent = Chips
RHe.BrickColor = BrickColor.new("Really black")
RHe.Locked = true
RHe.CanCollide = false
RHe.Transparency = 0
PMesh = IT("SpecialMesh")
RHe.formFactor =  "Symmetric"
PMesh.MeshType = "FileMesh"
PMesh.MeshId = "rbxassetid://19106014"
PMesh.TextureId = "rbxassetid://342435650"
PMesh.Scale = Vector3.new(1, 1.4, 0.8)
PMesh.Parent = RHe
local RWeld = IT("Weld")
RWeld.Parent = RHe
RWeld.Part0 = RHe
RWeld.Part1 = ra
RWeld.C0 = CF(-1.2, -0.5, 0) * angles(Rad(90), Rad(0), Rad(90))
-------------------------------------------------------
--End Customization--
-------------------------------------------------------


-------------------------------------------------------
--Start Attacks N Stuff--
-------------------------------------------------------
function AttackTemplate()
	attack = true
	for i = 0, 2, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.1)
	end
	attack = false
end
function HitboxFunction(Pose, lifetime, siz1, siz2, siz3, Radie, Min, Max, kb, atype)
  local Hitboxpart = Instance.new("Part", EffectModel)
  RemoveOutlines(Hitboxpart)
  Hitboxpart.Size = Vector3.new(siz1, siz2, siz3)
  Hitboxpart.CanCollide = false
  Hitboxpart.Transparency = 1
  Hitboxpart.Anchored = true
  Hitboxpart.CFrame = Pose
  game:GetService("Debris"):AddItem(Hitboxpart, lifetime)
  MagniDamage(Hitboxpart, Radie, Min, Max, kb, atype)
end
wait2 = false
combo = 1
mouse.Button1Down:connect(function(key)
  if attack == false then
    attack = true
   	Speed = 3.01
    if combo == 1 and wait2 == false then
      wait2 = true
		for i = 0, 1.6, 0.1 do
        	swait()
      		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-45)), 0.2)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(45)), 0.2)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-45), Rad(0)) * angles(Rad(0), Rad(0), Rad(15)), 0.2)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-15)), 0.2)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(180), Rad(0 + 5 * Sin(sine / 20)), Rad(25 + 5 * Sin(sine / 20))), 0.2)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.2)
		end
		Cso("138097048", ra, 1.2, 0.8)
		HitboxFunction(ra.CFrame, 0.01, 1, 1, 1, 7, 6, 9, 3, "Normal")
      	for i = 0, 1.2, 0.1 do
			swait()
        	rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(20), Rad(0), Rad(45)), 0.3)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 5 * Sin(sine / 20)), Rad(0), Rad(-45)), 0.3)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(20), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(15)), 0.3)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(45), Rad(0)) * angles(Rad(0), Rad(0), Rad(-15)), 0.3)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(85), Rad(0 + 5 * Sin(sine / 20)), Rad(45 + 5 * Sin(sine / 20))), 0.3)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-45), Rad(0 - 5 * Sin(sine / 20)), Rad(-25 - 5 * Sin(sine / 20))), 0.3)
      	end
      combo = 1
    end
  	Speed = 56
    wait2 = false
    attack = false
	end
end)
function Taunt()
	attack = true
	Speed = 3
	if Chips == "onebearnakedwoman" then
		local Munch = Cso("1575472350", hed, 5, 1)
		swait(2)
		repeat
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.2 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-20), Rad(0), Rad(0)), 0.3)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-35 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.3)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.3)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.3)
			RW.C0 = clerp(RW.C0, CF(1* Player_Size, 0.1 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(160), Rad(0), Rad(-35)), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.3)
		until Munch.Playing == false
	elseif Chips == "layonme" then
		for i = 0, 6, 0.1 do
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0 - 255.45 * i)), 0.15)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(10), Rad(30 + 5 * Sin(sine / 20)), Rad(45 + 5 * Sin(sine / 20))), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(10), Rad(-30 - 5 * Sin(sine / 20)), Rad(-45 - 5 * Sin(sine / 20))), 0.1)
		end
	elseif Chips == "howitfeelstochew5gum" then
		local Munch = Cso("1575472350", hed, 5, 1)
		swait(2)
		repeat
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.2 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-20), Rad(0), Rad(0)), 0.3)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-35 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.3)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.3)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.3)
			RW.C0 = clerp(RW.C0, CF(1* Player_Size, 0.1 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(160), Rad(0), Rad(-35)), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.3)
		until Munch.Playing == false
		Cso("172324194", hed, 5, 1)
		for i = 0, 5, 0.1 do
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.2 * Player_Size) * angles(Rad(-20), Rad(0), Rad(0)), 0.3)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-35), Rad(0), Rad(0)), 0.3)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 * Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.3)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 * Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.3)
			RW.C0 = clerp(RW.C0, CF(1* Player_Size, 0.1* Player_Size, -0.6* Player_Size) * angles(Rad(160), Rad(0), Rad(-35)), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(-20), Rad(0), Rad(-10)), 0.3)
		end
		local RUN = Cso("957655044", hed, 5, 1)
		swait(2)
		repeat
			swait()
			Speed = 56
			local WALKSPEEDVALUE = 6 / (hum.WalkSpeed / 16)
			root.Velocity = root.CFrame.lookVector * 75
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.3 - 0.65 * Cos(sine / ( WALKSPEEDVALUE / 2 ))) * angles(Rad(-25), Rad(0), Rad(0 - 1.75 * Cos(sine / ( WALKSPEEDVALUE / 2))) + root.RotVelocity.Y / 75), 0.1)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-20 + 5 * Sin(sine / (WALKSPEEDVALUE / 2))), Rad(0), Rad(0) + root.RotVelocity.Y / 13), 0.1)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.8 - 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, 0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size)  * angles(Rad(-15 - 95 * Cos(sine / WALKSPEEDVALUE)) - root.RotVelocity.Y / 75 + -Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 + 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
         	ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.8 + 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, -0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size) * angles(Rad(-15 + 95 * Cos(sine / WALKSPEEDVALUE)) + root.RotVelocity.Y / -75 + Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 - 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / WALKSPEEDVALUE)* Player_Size, 0* Player_Size) * angles(Rad(215), Rad(0), Rad(45)), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / WALKSPEEDVALUE)* Player_Size, 0* Player_Size) * angles(Rad(215), Rad(0), Rad(-45)), 0.1)
		until RUN.Playing == false
	elseif Chips == "5gumdowngrade" then
		Cso("1826625760", hed, 5, 1)
		for i = 0, 5, 0.1 do
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.1)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.1)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.1)
		end
	end
	Speed = 56
	movelegs = false
	attack = false
end
function Gum()
	attack = true
	Speed = 0
	local Senses = Cso("605297168", hed, 6, 1)
	swait(2)
	repeat
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(20)), 0.2)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(-20)), 0.2)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.2)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(-90)), 0.2)
	until Senses.TimePosition > 2.7
	for i = 0, 3, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size) * angles(Rad(-30), Rad(0), Rad(0)), 0.5)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0)), 0.5)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.5)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.5)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(10)), 0.5)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(-90)), 0.5)
	end
	root.Anchored = true
	repeat
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -2.7 + 0.1* Player_Size) * angles(Rad(90), Rad(0), Rad(0)), 0.5)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0)), 0.5)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.5)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.5)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(10)), 0.5)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(-90)), 0.5)
	until Senses.Playing == false
	Speed = 56
	attack = false
	root.Anchored = false
end
function OHHHHHHH()
	attack = true
	Speed = 0
	Cso("663306786", tors, 3, 1)
	for i = 0, 12, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0 + 1 * i * Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-20), Rad(0), Rad(0)), 0.1)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(47), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(65), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(75), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(143), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.1)
	end
	Cso("663307468", tors, 6, 1)
	for i = 0, 6, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 4500 * Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-20), Rad(0), Rad(0)), 0.15)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(47), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(65), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(75), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(156), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.1)
	end
	Speed = 56
	attack = false
end
function WoodyGotWood()
	attack = true
	Speed = 0
	local Woodlenny = Cso("1764642350", hed, 6, 1)
	swait(2)
	repeat
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 5)) * angles(Rad(20), Rad(0), Rad(5)), 0.2)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20), Rad(0), Rad(-5 - 15 * Sin(sine / 20))), 0.2)
		rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 5)* Player_Size, 0* Player_Size) * angles(Rad(20), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.2)
		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 5)* Player_Size, 0* Player_Size) * angles(Rad(20), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 5)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(10)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 5)* Player_Size, 0* Player_Size) * angles(Rad(20), Rad(0), Rad(-10)), 0.2)
	until Woodlenny.TimePosition > 3.6
	root.Anchored = true
	repeat
		swait()
		for i = 0, 2, 0.1 do
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -2.7 + 0.1* Player_Size) * angles(Rad(-90), Rad(0), Rad(0)), 0.5)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0)), 0.5)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.5)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.5)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(90)), 0.5)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(-90)), 0.5)
		end
		for i = 0, 1.6, 0.1 do
			swait()
			rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -2.4 + 0.1* Player_Size) * angles(Rad(-90), Rad(0), Rad(0)), 0.5)
			neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0)), 0.5)
			rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.5)
			ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.5)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(75)), 0.5)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(0), Rad(-75)), 0.5)
		end
	until Woodlenny.Playing == false
	Speed = 56
	attack = false
	root.Anchored = false
end
-------------------------------------------------------
--End Attacks N Stuff--
-------------------------------------------------------
mouse.KeyDown:connect(function(key)
	if attack == false then
		if key == "t" then
			Taunt()
		elseif key == "z" then
			Gum()
		elseif key == "x" then
			OHHHHHHH()
		elseif key == "c" then
			WoodyGotWood() 
                elseif key == "f" then
			SONG = 690663957
			Music.TimePosition = 0
			PMesh.TextureId = "rbxassetid://206977326"
	        	Chips = "cheesexd"

		elseif key == "m" then
			SONG = 525565668
			Music.TimePosition = 0
			PMesh.TextureId = "rbxassetid://342435650"
			Chips = "onebearnakedwoman"
		elseif key == "n" then
			SONG = 937445925
			Music.TimePosition = 0
			PMesh.TextureId = "rbxassetid://342436716"
			Chips = "layonme"
		elseif key == "b" then
			SONG = 1386299751
			Music.TimePosition = 0
			PMesh.TextureId = "rbxassetid://341999291"
			Chips = "howitfeelstochew5gum"
		elseif key == "v" then
			SONG = 554967156
			Music.TimePosition = 0
			PMesh.TextureId = "rbxassetid://341999245"
			Chips = "5gumdowngrade"
		end
	end
end)

 






-------------------------------------------------------
--Start Animations--
-------------------------------------------------------
while true do
	swait()
	sine = sine + change
	local torvel = (root.Velocity * Vector3.new(1, 0, 1)).magnitude
	local velderp = root.Velocity.y
	hitfloor, posfloor = rayCast(root.Position, CFrame.new(root.Position, root.Position - Vector3.new(0, 1, 0)).lookVector, 4* Player_Size, workspace[plr.Name])
	if equipped == true or equipped == false then
		if attack == false then
			idle = idle + 1
		else
			idle = 0
		end
		local Landed = false
		if(hitfloor)then
			WasAir = false
		else
			WasAir = true
		end
		if(WasAir == false)then
			if(InAir == true)then
				LandTick = time()
				Landed = true
			end
		end
		if(time()-LandTick < .3)then
			Landed = true
		end
		if(hitfloor)then
			InAir = false
		else
			InAir = true
		end
		local WALKSPEEDVALUE = 6 / (hum.WalkSpeed / 16)
		local Walking = (math.abs(root.Velocity.x) > 1 or math.abs(root.Velocity.z) > 1)
		local State = (hum.PlatformStand and 'Paralyzed' or hum.Sit and 'Sit' or Landed and 'Land' or not hitfloor and root.Velocity.y < -1 and "Fall" or not hitfloor and root.Velocity.y > 1 and "Jump" or hitfloor and Walking and "Walk" or hitfloor and "Idle")
		if(State == 'Jump')then
			hum.JumpPower = 55
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1 * Cos(sine / 20)* Player_Size) * angles(Rad(-16), Rad(0), Rad(0)), 0.1)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(10 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -.2 - 0.1 * Cos(sine / 20), -.3* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.1)
				ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -.9 - 0.1 * Cos(sine / 20), -.5* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.1)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(25), Rad(-.6), Rad(13 + 4.5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(25), Rad(-.6), Rad(-13 - 4.5 * Sin(sine / 20))), 0.1)
			end
		elseif(State == 'Fall')then
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1 * Cos(sine / 20)* Player_Size) * angles(Rad(25), Rad(0), Rad(0)), 0.1)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(10 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -1 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(25), Rad(0), Rad(0)), 0.1)
				ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -.8 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(25), Rad(0), Rad(0)), 0.1)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(165), Rad(-.6), Rad(45 + 4.5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(165), Rad(-.6), Rad(-45 - 4.5 * Sin(sine / 20))), 0.1)
			end
		elseif(State == 'Land')then
			hum.JumpPower = 0
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -1 + 0.1 * Cos(sine / 20)* Player_Size) * angles(Rad(10), Rad(0), Rad(0)), 0.15)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(35 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, 0.1 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * angles(Rad(0), Rad(-10), Rad(0)) * angles(Rad(-3.5), Rad(0), Rad(5)), 0.15)
				ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, 0.1 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * angles(Rad(0), Rad(10), Rad(0)) * angles(Rad(-3.5), Rad(0), Rad(-5)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(65), Rad(0), Rad(25 + 4.5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(55), Rad(0), Rad(-25 - 4.5 * Sin(sine / 20))), 0.1)
			end
		elseif(State == 'Idle')then
			change = 1
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(-10), Rad(0)) * angles(Rad(0), Rad(0), Rad(5)), 0.1)
				ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(10), Rad(0)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0 + 15 * Sin(sine / 20)), Rad(0 + 5 * Sin(sine / 20)), Rad(10 + 5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0 + 15 * Sin(sine / 20)), Rad(0 - 5 * Sin(sine / 20)), Rad(-10 - 5 * Sin(sine / 20))), 0.1)
			end
		elseif(State == 'Walk')then
			change = 0.55
			hum.JumpPower = 55
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.3 - 0.65 * Cos(sine / ( WALKSPEEDVALUE / 2 ))) * angles(Rad(-25), Rad(0), Rad(0 - 1.75 * Cos(sine / ( WALKSPEEDVALUE / 2))) + root.RotVelocity.Y / 75), 0.1)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-20 + 5 * Sin(sine / (WALKSPEEDVALUE / 2))), Rad(0), Rad(0) + root.RotVelocity.Y / 13), 0.1)
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.8 - 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, 0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size)  * angles(Rad(-15 - 95 * Cos(sine / WALKSPEEDVALUE)) - root.RotVelocity.Y / 75 + -Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 + 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
         		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.8 + 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, -0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size) * angles(Rad(-15 + 95 * Cos(sine / WALKSPEEDVALUE)) + root.RotVelocity.Y / -75 + Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 - 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / WALKSPEEDVALUE)* Player_Size, 0* Player_Size) * angles(Rad(215), Rad(0), Rad(45)), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / WALKSPEEDVALUE)* Player_Size, 0* Player_Size) * angles(Rad(215), Rad(0), Rad(-45)), 0.1)
			elseif attack == true and movelegs == true then
				rl.Weld.C0 = clerp(rl.Weld.C0, CF(0.5* Player_Size, -0.8 - 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, 0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size)  * angles(Rad(-10 - 25 * Cos(sine / WALKSPEEDVALUE)) - root.RotVelocity.Y / 75 + -Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 + 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
         		ll.Weld.C0 = clerp(ll.Weld.C0, CF(-0.5* Player_Size, -0.8 + 0.5 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size, -0.6 * Cos(sine / WALKSPEEDVALUE) / 2* Player_Size) * angles(Rad(-10 + 25 * Cos(sine / WALKSPEEDVALUE)) + root.RotVelocity.Y / -75 + Sin(sine / WALKSPEEDVALUE) / 2.5, Rad(0 - 10 * Cos(sine / WALKSPEEDVALUE)), Rad(0)) * angles(Rad(0 - 2 * Cos(sine / WALKSPEEDVALUE)), Rad(0), Rad(0)), 0.3)
			end
		end
	end
	hum.Name = "HUM"
	hum.WalkSpeed = Speed
	Music.SoundId = "rbxassetid://"..SONG
	Music.Looped = true
	Music.Pitch = 1
	Music.Volume = 1.5
	Music.Parent = tors
	Music.Playing = true
	if 0 < #Effects then
		for e = 1, #Effects do
			if Effects[e] ~= nil then
				local Thing = Effects[e]
				if Thing ~= nil then
					local Part = Thing[1]
					local Mode = Thing[2]
					local Delay = Thing[3]
					local IncX = Thing[4]
					local IncY = Thing[5]
					local IncZ = Thing[6]
					if 1 >= Thing[1].Transparency then
						if Thing[2] == "Block1" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block2" then
							Thing[1].CFrame = Thing[1].CFrame + Vector3.new(0, 0, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block3" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50)) + Vector3.new(0, 0.15, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Cylinder" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Blood" then
							local Mesh = Thing[7]
							Thing[1].CFrame = Thing[1].CFrame * Vector3.new(0, 0.5, 0)
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Elec" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[7], Thing[8], Thing[9])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Disappear" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Shatter" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
							Thing[4] = Thing[4] * CFrame.new(0, Thing[7], 0)
							Thing[1].CFrame = Thing[4] * CFrame.fromEulerAnglesXYZ(Thing[6], 0, 0)
							Thing[6] = Thing[6] + Thing[5]
						end
					else
						Part.Parent = nil
						table.remove(Effects, e)
					end
				end
			end
		end
	end
end
-------------------------------------------------------
--End Animations And Script--
-------------------------------------------------------("Clicked")
											end)


											section2:addButton("FE Elio Blasio", function()
												--// SETTINGS \\ --

local Fling = true --// Recommended: true
local FlingBlockInvisible = true --// Recommended: false (So you can see the flinging block)
local HighlightFlingBlock = false --// Recommended: true
local FlingHighlightColor = Color3.fromRGB(55,55,255) --// First is R, Second is G, Third is B
local GunHatId = 5410674378 -- // (https://www.roblox.com/catalog/5410674378/METAL-x-LIGHTSEER-77)
--// GunHatId is the HatId you will use as the gun for the script, you must have the hat equipped.




-- // MAIN \\ --

local HAT_NAME = game:GetObjects("rbxassetid://"..tostring(GunHatId))[1].Name

-- // Uses Mizt's bypass \\ --

Bypass = "death"
loadstring(game:GetObjects("rbxassetid://5325226148")[1].Source)()

local IsDead = false
local StateMover = true

local playerss = workspace.non
local bbv,bullet
if Bypass == "death" then
	bullet = game.Players.LocalPlayer.Character["HumanoidRootPart"]
	bullet.Transparency = (FlingBlockInvisible ~= true and 0 or 1)
	bullet.Massless = true
	if bullet:FindFirstChildOfClass("Attachment") then
		for _,v in pairs(bullet:GetChildren()) do
			if v:IsA("Attachment") then
				v:Destroy()
			end
		end
	end

	bbv = Instance.new("BodyPosition",bullet)
    bbv.Position = playerss.Torso.CFrame.p
end

if Bypass == "death" then
coroutine.wrap(function()
	while true do
		if not playerss or not playerss:FindFirstChildOfClass("Humanoid") or playerss:FindFirstChildOfClass("Humanoid").Health <= 0 then IsDead = true; return end
		if StateMover then
			bbv.Position = playerss.Torso.CFrame.p
    		bullet.Position = playerss.Torso.CFrame.p
		end
		game:GetService("RunService").RenderStepped:wait()
	end
end)()
end

--[[local force = Instance.new("BodyForce",bullet)
force.Force = Vector3.new(800,800,800)]]--

if HighlightFlingBlock ~= false then
    local Highlight = Instance.new("SelectionBox")
    Highlight.Adornee = bullet
    Highlight.Color3 = (typeof(FlingHighlightColor)=="Color3" and FlingHighlightColor) or (Color3.fromRGB(55,55,255))
    Highlight.Parent = bullet
    Highlight.Name = "HighlightBox"
end

bbav = Instance.new("BodyAngularVelocity",bullet)
    bbav.MaxTorque = Vector3.new(math.huge,math.huge,math.huge)
    bbav.P = 100000000000000000000000000000
    bbav.AngularVelocity = Vector3.new(10000000000000000000000000000000,100000000000000000000000000,100000000000000000)

local CDDF = {}
local DamageFling = function(DmgPer)
    if Fling ~= true then return end
	if IsDead or Bypass ~= "death" or (DmgPer.Name == playerss.Name and DmgPer.Name == "non") or CDDF[DmgPer] or not DmgPer or not DmgPer:FindFirstChildOfClass("Humanoid") or DmgPer:FindFirstChildOfClass("Humanoid").Health <= 0 then return end
	CDDF[DmgPer] = true; StateMover = false
	local PosFling = (DmgPer:FindFirstChild("HumanoidRootPart") and DmgPer:FindFirstChild("HumanoidRootPart") .CFrame.p) or (DmgPer:FindFirstChildOfClass("Part") and DmgPer:FindFirstChildOfClass("Part").CFrame.p)
    bullet.Rotation = playerss.Torso.Rotation
    bbav = Instance.new("BodyAngularVelocity",bullet)
    bbav.MaxTorque = Vector3.new(math.huge,math.huge,math.huge)
    bbav.P = 100000000000000000000000000000
    bbav.AngularVelocity = Vector3.new(10000000000000000000000000000000,100000000000000000000000000,100000000000000000)

	for _=1,15 do
		bbv.Position = PosFling
		bullet.Position = PosFling
		wait(0.03)
	end
	bbav:Destroy()
	bbv.Position = playerss.Torso.CFrame.p
    bullet.Position = playerss.Torso.CFrame.p
	CDDF[DmgPer] = false; StateMover = true
end

wait(.2)


local Aligns = 0
function Align(Part0,Part1,Position,Rotation)
    Aligns = Aligns + 1
	local a0,a1 = Instance.new("Attachment",Part0), Instance.new("Attachment",Part1)
	a1.Position = Position or Vector3.new()
	a1.Rotation = Rotation or Vector3.new()
	local ap = Instance.new("AlignPosition", Part0)
	ap.Attachment0 = a0
	ap.Attachment1 = a1
	local ao = Instance.new("AlignOrientation", Part0)
	ao.Name = "AO-"..Aligns
	ap.Name = "AP-"..Aligns
	ao.Attachment0 = a0
	ao.Attachment1 = a1
	ap.ApplyAtCenterOfMass = true;
	ap.MaxForce = 67752;
	ap.MaxVelocity = math.huge/9e110;
	ap.ReactionForceEnabled = false;
	ap.Responsiveness = 200;
	ap.RigidityEnabled = false;
	ao.MaxAngularVelocity = math.huge/9e110;
	ao.MaxTorque = 67752;
	ao.PrimaryAxisOnly = false;
	ao.ReactionTorqueEnabled = false;
	ao.Responsiveness = 200;
	ao.RigidityEnabled = false;
	return a1
end

local HumanoidIsDead = false

local Player=game.Players.LocalPlayer
local Character=workspace.non
local Gun = Character[HAT_NAME]
local GunHandle = Gun.Handle
GunHandle.AccessoryWeld:Destroy()
GunHandle.SpecialMesh:Destroy()
wait()
GunHandle.Parent=workspace
local hum = Character.Humanoid
local LeftArm=Character["Left Arm"]
local LeftLeg=Character["Left Leg"]
local RightArm=Character["Right Arm"]
local RightLeg=Character["Right Leg"]
local Root=Character["HumanoidRootPart"]
local Head=Character["Head"]
local Torso=Character["Torso"]
local Neck=Torso["Neck"]
local mouse = Player:GetMouse()
local walking = false
local jumping = false
local attacking = false
local firsttime = false
local tauntdebounce = false
local position = nil
local MseGuide = true
local running = false
local settime = 0
local sine = 0
local t = 0
local ws = 18
local change = 1
local combo1 = true
local equip = false
local dgs = 75
local combo2 = false
local switch1 = true
local switch2 = false
local firsttime2 = false
local combo3 = false
local gunallowance = false
local shooting = false
local RunSrv = game:GetService("RunService")
local RenderStepped = game:GetService("RunService").RenderStepped
local removeuseless = game:GetService("Debris")

coroutine.wrap(function()
	while true do
		wait()
		if not Character or not Character:FindFirstChild("Humanoid") or Character:FindFirstChild("Humanoid").Health <= 0 then
			HumanoidIsDead = true
			break
		end
	end
end)()

local HEADLERP = Instance.new("ManualWeld")
HEADLERP.Parent = Head
HEADLERP.Part0 = Head
HEADLERP.Part1 = Head
HEADLERP.C0 = CFrame.new(0, -1.5, -0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local TORSOLERP = Instance.new("ManualWeld")
TORSOLERP.Parent = Root
TORSOLERP.Part0 = Torso
TORSOLERP.C0 = CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local ROOTLERP = Instance.new("ManualWeld")
ROOTLERP.Parent = Root
ROOTLERP.Part0 = Root
ROOTLERP.Part1 = Torso
ROOTLERP.C0 = CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local RIGHTARMLERP = Instance.new("ManualWeld")
RIGHTARMLERP.Parent = RightArm
RIGHTARMLERP.Part0 = RightArm
RIGHTARMLERP.Part1 = Torso
RIGHTARMLERP.C0 = CFrame.new(-1.5, 0, -0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local LEFTARMLERP = Instance.new("ManualWeld")
LEFTARMLERP.Parent = LeftArm
LEFTARMLERP.Part0 = LeftArm
LEFTARMLERP.Part1 = Torso
LEFTARMLERP.C0 = CFrame.new(1.5, 0, -0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local RIGHTLEGLERP = Instance.new("ManualWeld")
RIGHTLEGLERP.Parent = RightLeg
RIGHTLEGLERP.Part0 = RightLeg
RIGHTLEGLERP.Part1 = Torso
RIGHTLEGLERP.C0 = CFrame.new(-0.5, 2, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local LEFTLEGLERP = Instance.new("ManualWeld")
LEFTLEGLERP.Parent = LeftLeg
LEFTLEGLERP.Part0 = LeftLeg
LEFTLEGLERP.Part1 = Torso
LEFTLEGLERP.C0 = CFrame.new(0.5, 2, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))

local function weldBetween(a, b)
    local weld = Instance.new("ManualWeld", a)
    weld.Part0 = a
    weld.Part1 = b
    weld.C0 = a.CFrame:inverse() * b.CFrame
    return weld
end

function MAKETRAIL(PARENT,POSITION1,POSITION2,LIFETIME,COLOR)
A = Instance.new("Attachment", PARENT)
A.Position = POSITION1
A.Name = "A"
B = Instance.new("Attachment", PARENT)
B.Position = POSITION2
B.Name = "B"
tr1 = Instance.new("Trail", PARENT)
tr1.Attachment0 = A
tr1.Attachment1 = B
tr1.Enabled = true
tr1.Lifetime = LIFETIME
tr1.TextureMode = "Static"
tr1.LightInfluence = 0
tr1.Color = COLOR
tr1.Transparency = NumberSequence.new(0, 1)
end

tommygun = Instance.new("Part",Character)
tommygun.Size = Vector3.new(2,2,2)
tommygun.CFrame = RightArm.CFrame
tommygun.CanCollide = false
tommygun.Transparency=1
tommygunweld = Instance.new("Weld",tommygun)
GUN_A1=Align(GunHandle,tommygun,Vector3.new(),Vector3.new(115,-100,90))
tommygunweld.Part0 = tommygun
tommygunweld.Part1 = RightArm
tommygunweld.C0 = tommygun.CFrame:inverse() * RightArm.CFrame * CFrame.new(0,-.80,1.25) * CFrame.Angles(math.rad(98),math.rad(0),0)
mtommygun = Instance.new("SpecialMesh", tommygun)
mtommygun.MeshType = "FileMesh"
mtommygun.Scale = Vector3.new(1, 1, 1)
mtommygun.MeshId,mtommygun.TextureId = 'http://www.roblox.com/asset/?id=116679805','http://www.roblox.com/asset/?id=116679995'
shootbox = Instance.new("Part",Character)
shootbox.Size = Vector3.new(.2,.2,.2)
shootbox.Transparency=1
shootbox.CanCollide = false
shootbox.Transparency = 1
shootbox.CFrame = tommygun.CFrame
shootboxweld = weldBetween(shootbox,tommygun)
shootboxweld.C0 = CFrame.new(0,-.05,2.62)
light = Instance.new("PointLight", shootbox)
light.Color = BrickColor.new("Bright yellow").Color
light.Range = 5
light.Brightness = 11
light.Enabled = false
particlemiter1 = Instance.new("ParticleEmitter", shootbox)
particlemiter1.Enabled = false
particlemiter1.Texture = "rbxassetid://461242617"
particlemiter1.Lifetime = NumberRange.new(.1)
particlemiter1.Size = NumberSequence.new(1,0)
particlemiter1.Rate = 20
particlemiter1.RotSpeed = NumberRange.new(0)
particlemiter1.Speed = NumberRange.new(0)
tommygunammo = Instance.new("Part",Character)
tommygunammo.Size = Vector3.new(2,2,2)
tommygunammo.CFrame = tommygun.CFrame
tommygunammo.CanCollide = false
tommygunammoweld = Instance.new("Weld",tommygunammo)
tommygunammoweld.Part0 = tommygunammo
tommygunammoweld.Part1 = tommygun
tommygunammo.Transparency = 1
tommygunammoweld.C0 = tommygun.CFrame:inverse() * tommygun.CFrame * CFrame.new(0,.4,.25) * CFrame.Angles(math.rad(0),math.rad(0),0)
mtommygunammo = Instance.new("SpecialMesh", tommygunammo)
mtommygunammo.MeshType = "FileMesh"
mtommygunammo.Scale = Vector3.new(1, 1, 1)
mtommygunammo.MeshId,mtommygunammo.TextureId = 'http://www.roblox.com/asset/?id=116740155','http://www.roblox.com/asset/?id=116679995'


coroutine.wrap(function()
for i,v in pairs(Character:GetChildren()) do
if v.Name == "Animate" then v:Remove()
end
end
end)()

function damagealll(Radius,Position)		
	local Returning = {}		
	for _,v in pairs(workspace:GetChildren()) do		
		if v~=Character and v:FindFirstChildOfClass('Humanoid') and v:FindFirstChild('Torso') or v:FindFirstChild('UpperTorso') then
if v:FindFirstChild("Torso") then		
			local Mag = (v.Torso.Position - Position).magnitude		
			if Mag < Radius then		
				table.insert(Returning,v)		
			end
elseif v:FindFirstChild("UpperTorso") then	
			local Mag = (v.UpperTorso.Position - Position).magnitude		
			if Mag < Radius then		
				table.insert(Returning,v)		
			end
end	
		end		
	end		
	return Returning		
end

function swait(num)
	if num == 0 or num == nil then
		game:service("RunService").Stepped:wait(0)
	else
		for i = 0, num do
			game:service("RunService").Stepped:wait(0)
		end
	end
end

doomtheme = Instance.new("Sound", Torso)
doomtheme.Volume = 1
doomtheme.Name = "doomtheme"
doomtheme.Looped = true
doomtheme.SoundId = "rbxassetid://318812395"
doomtheme:Play()

Torso.ChildRemoved:connect(function(removed)
if removed.Name == "doomtheme" then

doomtheme = Instance.new("Sound", Torso)
doomtheme.Volume = 1
doomtheme.Name = "doomtheme"
doomtheme.Looped = true
doomtheme.SoundId = "rbxassetid://318812395"
doomtheme:Play()
end
end)

function SOUND(PARENT,ID,VOL,LOOP,REMOVE)
so = Instance.new("Sound")
so.Parent = PARENT
so.SoundId = "rbxassetid://"..ID
so.Volume = VOL
so.Looped = LOOP
so:Play()
removeuseless:AddItem(so,REMOVE)
end

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='t' then
if tauntdebounce then return end
tauntdebounce = true
local b1 = Instance.new("BillboardGui",Head)
b1.Size = UDim2.new(0,4,0,1.6)
b1.StudsOffset = Vector3.new(0,0,0)
b1.Name = "laff"
b1.AlwaysOnTop = true
b1.StudsOffset = Vector3.new(0,2,0)
b1.Adornee = Head
removeuseless:AddItem(b1,3)
local b2 = Instance.new("TextLabel",b1)
b2.BackgroundTransparency = 1
b2.Text = "HeHeHeHeHeHeHe..."
b2.Font = "Garamond"
b2.TextSize = 30
b2.Name = "lafftext"
b2.TextStrokeTransparency = 0
b2.TextColor3 = BrickColor.new("Grey").Color
b2.TextStrokeColor3 = Color3.new(0,0,0)
b2.Size = UDim2.new(1,0,.5,0)
laff = Instance.new("Sound",Head)
laff.SoundId = "rbxassetid://2126502539"
laff.Volume = 5
laff:Play()
wait(5)
laff:Remove()
tauntdebounce = false
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='e' then
if debounce then return end
if equip then
g1:Remove()
light.Enabled = false
pcall(function()
temmy:Remove()
end)
for i,v in pairs(tommygun:GetDescendants()) do
if v.Name == "temmy" then v:Remove()
end
end
light.Enabled = false
particlemiter1.Enabled = false
hum.CameraOffset = Vector3.new(0,0,0)
attacking = false
equip = false
shooting = false
gunallowance = false
ws = 18
else
g1 = Instance.new("BodyGyro", Root)
g1.D = 175
g1.P = 20000
g1.MaxTorque = Vector3.new(0,9000,0)
g1.CFrame = CFrame.new(Root.Position,mouse.Hit.p)
attacking = true
debounce = true
equip = true
coroutine.wrap(function()
while equip do
g1.CFrame = g1.CFrame:lerp(CFrame.new(Root.Position,mouse.Hit.p),.1)
ws = 10
swait()
if Root.Velocity.y > 1 then
position = "Jump3"
elseif Root.Velocity.y < -1 then
position = "Falling3"
elseif Root.Velocity.Magnitude > 2 and running == false and attacking == true then
position = "Walk3"
elseif Root.Velocity.Magnitude < 2 and running == false and attacking == true then
position = "Idle4"
end
end
end)()
coroutine.wrap(function()
while equip do
swait()
settime = 0.05
sine = sine + change
if position == "Jump3" and attacking and not running then
change = .65
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 2, 0) * CFrame.Angles(math.rad(10), math.rad(0), math.rad(0)), 0.4)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.0, .9) * CFrame.Angles(math.rad(20), math.rad(0), math.rad(0)), 0.4)
elseif position == "Falling3" and attacking and not running then
change = .65
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 2, 0) * CFrame.Angles(math.rad(8), math.rad(4), math.rad(0)), 0.4)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.0, .9) * CFrame.Angles(math.rad(14), math.rad(-4), math.rad(0)), 0.4)
elseif position == "Walk3" and attacking == true and running == false then
change = .65
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0.05*math.sin(sine/4), 0) * CFrame.Angles(math.rad(0), math.rad(-50), math.rad(0)),.2)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 1.92 - 0.35 * math.cos(sine/8)/2.8, 0.2 - math.sin(sine/8)/3.4) * CFrame.Angles(math.rad(10) + -math.sin(sine/8)/2.3, math.rad(0)*math.cos(sine/1), math.rad(0), math.cos(25 * math.cos(sine/8))), 0.1)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.92 + 0.35 * math.cos(sine/8)/2.8, 0.2 + math.sin(sine/8)/3.4) * CFrame.Angles(math.rad(10) - -math.sin(sine/8)/2.3, math.rad(0)*math.cos(sine/1), math.rad(0) , math.cos(25 * math.cos(sine/8))), 0.1)
elseif position == "Idle4" and attacking == true and running == false then
change = .65
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, -.2 + -.1 * math.sin(sine/25), 0) * CFrame.Angles(math.rad(0), math.rad(-50), math.rad(0)),.1)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.3, 2 - .1 * math.sin(sine/25), 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(-10)), 0.1)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.1)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.3, 2.0 - .1 * math.sin(sine/25), 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(10)), 0.1)
end
end
end)()
SOUND(RightArm,898163129,6,false,2)
for i = 1, 30 do
tommygunweld.C0 = tommygunweld.C0:lerp(CFrame.new(0,-.68,1.25) * CFrame.Angles(math.rad(90),math.rad(0),math.rad(-12)),.25)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1, 0.1, 0.4) * CFrame.Angles(math.rad(-90), math.rad(-60), math.rad(0)), 0.25)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1, 1.35, 0.4) * CFrame.Angles(math.rad(-90), math.rad(0), math.rad(0)), 0.25)
swait()
end
gunallowance = true
mouse.Button1Down:connect(function()
if gunallowance then
particlemiter1.Enabled = true
temmy = Instance.new("Sound",tommygun)
temmy.SoundId = "rbxassetid://2204318084"
temmy.Volume = 6
temmy.Name = "temmy"
temmy.Looped = true
temmy:Play()
shooting = true
end
end)
mouse.Button1Up:connect(function()
if gunallowance then
hum.CameraOffset = Vector3.new(0,0,0)
light.Enabled = false
particlemiter1.Enabled = false
pcall(function()
temmy:Remove()
end)
for i,v in pairs(tommygun:GetDescendants()) do
if v.Name == "temmy" then v:Remove()
end
end
shooting = false
end
end)
coroutine.wrap(function()
if firsttime2 then return end
firsttime2 = true
while true do
swait(3)
if shooting then
if switch1 then
switch1 = false
switch2 = true
light.Enabled = true
elseif switch2 then
switch1 = true
switch2 = false
light.Enabled = false
end
pcall(function()
if mouse.Target.Parent:FindFirstChildOfClass("Humanoid") then
DamageFling(mouse.Target.Parent)
end
end)
end
end
end)()
coroutine.wrap(function()
if firsttime then return end
firsttime = true
while true do
if shooting then
    pcall(function()
if mouse.Target.Parent:FindFirstChildOfClass("Humanoid") then
DamageFling(mouse.Target.Parent)
end
end)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1, 1.35, 0.4) * CFrame.Angles(math.rad(-90), math.rad(0 - 10 * math.sin(sine)), math.rad(0)), 0.25)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1, 0.1 + .4 * math.sin(sine), 0.4) * CFrame.Angles(math.rad(-90), math.rad(-60), math.rad(0)), 0.25)
elseif not shooting then
end
swait()
end
end)()
debounce = false
end
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='z' then
print("Music switched to 1")
id = 2199374985
doomtheme.SoundId = "rbxassetid://"..id
doomtheme:Play()
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='v' then
print("Music switched to 4")
id = 2111948183
doomtheme.SoundId = "rbxassetid://"..id
doomtheme:Play()
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='x' then
print("Music switched to 2")
id = 318812395
doomtheme.SoundId = "rbxassetid://"..id
doomtheme:Play()
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='c' then
print("Music switched to 3")
id = 180337897
doomtheme.SoundId = "rbxassetid://"..id
doomtheme:Play()
end
end)

mouse.KeyDown:connect(function(Press)
Press=Press:lower()
if Press=='b' then
print("Music switched to 5")
id = 649148458
doomtheme.SoundId = "rbxassetid://"..id
doomtheme:Play()
end
end)


checks1 = coroutine.wrap(function() -------Checks
while true do
if HumanoidIsDead then break end
if Root.Velocity.y > 1 then
position = "Jump"
elseif Root.Velocity.y < -1 then
position = "Falling"
elseif Root.Velocity.Magnitude < 2 then
position = "Idle"
elseif Root.Velocity.Magnitude < 20 then
position = "Walking"
elseif Root.Velocity.Magnitude > 20 then
position = "Running"
else
end
wait()
end
end)
checks1()

function ray(POSITION, DIRECTION, RANGE, IGNOREDECENDANTS)
	return workspace:FindPartOnRay(Ray.new(POSITION, DIRECTION.unit * RANGE), IGNOREDECENDANTS)
end

function ray2(StartPos, EndPos, Distance, Ignore)
local DIRECTION = CFrame.new(StartPos,EndPos).lookVector
return ray(StartPos, DIRECTION, Distance, Ignore)
end

OrgnC0 = Neck.C0
local movelimbs = coroutine.wrap(function()
while RunSrv.RenderStepped:wait() do
if HumanoidIsDead then break end
TrsoLV = Torso.CFrame.lookVector
Dist = nil
Diff = nil
if not MseGuide then
print("Failed to recognize")
else
local _, Point = Workspace:FindPartOnRay(Ray.new(Head.CFrame.p, mouse.Hit.lookVector), Workspace, false, true)
Dist = (Head.CFrame.p-Point).magnitude
Diff = Head.CFrame.Y-Point.Y
local _, Point2 = Workspace:FindPartOnRay(Ray.new(LeftArm.CFrame.p, mouse.Hit.lookVector), Workspace, false, true)
Dist2 = (LeftArm.CFrame.p-Point).magnitude
Diff2 = LeftArm.CFrame.Y-Point.Y
HEADLERP.C0 = CFrame.new(0, -1.5, -0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0))
Neck.C0 = Neck.C0:lerp(OrgnC0*CFrame.Angles((math.tan(Diff/Dist)*1), 0, (((Head.CFrame.p-Point).Unit):Cross(Torso.CFrame.lookVector)).Y*1), .1)
end
end
end)
movelimbs()
immortal = {}
for i,v in pairs(Character:GetDescendants()) do
	if v:IsA("BasePart") and v.Name ~= "lmagic" and v.Name ~= "rmagic" then
		if v ~= Root and v ~= Torso and v ~= Head and v ~= RightArm and v ~= LeftArm and v ~= RightLeg and v.Name ~= "lmagic" and v.Name ~= "rmagic" and v ~= LeftLeg then
			v.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
		end
		table.insert(immortal,{v,v.Parent,v.Material,v.Color,v.Transparency})
	elseif v:IsA("JointInstance") then
		table.insert(immortal,{v,v.Parent,nil,nil,nil})
	end
end
for e = 1, #immortal do
	if immortal[e] ~= nil then
		local STUFF = immortal[e]
		local PART = STUFF[1]
		local PARENT = STUFF[2]
		local MATERIAL = STUFF[3]
		local COLOR = STUFF[4]
		local TRANSPARENCY = STUFF[5]
if levitate then
		if PART.ClassName == "Part" and PART ~= Root and PART.Name ~= eyo1 and PART.Name ~= eyo2 and PART.Name ~= "lmagic" and PART.Name ~= "rmagic" then
			PART.Material = MATERIAL
			PART.Color = COLOR
			PART.Transparency = TRANSPARENCY
		end
		PART.AncestryChanged:connect(function()
			PART.Parent = PARENT
		end)
else
		if PART.ClassName == "Part" and PART ~= Root and PART.Name ~= "lmagic" and PART.Name ~= "rmagic" then
			PART.Material = MATERIAL
			PART.Color = COLOR
			PART.Transparency = TRANSPARENCY
		end
		PART.AncestryChanged:connect(function()
			PART.Parent = PARENT
		end)
end
	end
end
coroutine.wrap(function()
while true do
if HumanoidIsDead then break end
if hum.Health < .1 then
deadsound = Instance.new("Sound", Torso)
deadsound.Volume = 6
deadsound.SoundId = "rbxassetid://1411352723"
deadsound:Play()
end
wait()
end
end)()

local anims = coroutine.wrap(function()
while true do
if HumanoidIsDead then break end
settime = 0.05
sine = sine + change
if position == "Jump" and attacking == false then
change = 1
tommygunweld.C0 = tommygunweld.C0:lerp(CFrame.new(0,-.80,1.25) * CFrame.Angles(math.rad(98),math.rad(0),0),.25)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.1)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.4)
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(0)), 0.4)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.4,.1,-.2) * CFrame.Angles(math.rad(20),math.rad(-3),math.rad(-4)), 0.4)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 2, 0) * CFrame.Angles(math.rad(10), math.rad(0), math.rad(0)), 0.4)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.0, .9) * CFrame.Angles(math.rad(20), math.rad(0), math.rad(0)), 0.4)
elseif position == "Jump2" and attacking == false then
change = 1
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(-20 - 1 * math.sin(sine/9)), math.rad(0 + 0 * math.cos(sine/8)), math.rad(0) + Root.RotVelocity.Y / 30, math.cos(10 * math.cos(sine/10))), 0.3)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.3)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.3)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.5,.6,-.5) * CFrame.Angles(math.rad(32),math.rad(5 - .1 * math.sin(sine/12)),math.rad(40 - .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.5,.6,-.5) * CFrame.Angles(math.rad(30),math.rad(-5 + .1 * math.sin(sine/12)),math.rad(-40 + .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(.2,1.2,-.3),.3)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.54, 1.4 + .1 * math.sin(sine/9), .4) * CFrame.Angles(math.rad(9 + 2 * math.cos(sine/9)), math.rad(0), math.rad(0)), 0.3)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.54, 2.0 + .02 * math.sin(sine/9), 0.2 + .1 * math.sin(sine/9)) * CFrame.Angles(math.rad(25 + 5 * math.sin(sine/9)), math.rad(20), math.rad(0)), 0.3)
elseif position == "Falling" and attacking == false then
change = 1
tommygunweld.C0 = tommygunweld.C0:lerp(CFrame.new(0,-.80,1.25) * CFrame.Angles(math.rad(98),math.rad(0),0),.25)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.1)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.4)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 2, 0) * CFrame.Angles(math.rad(8), math.rad(4), math.rad(0)), 0.2)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.0, .9) * CFrame.Angles(math.rad(14), math.rad(-4), math.rad(0)), 0.2)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.6, 0.5, 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(20)), 0.2)
elseif position == "Falling2" and attacking == false then
change = 1
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(-20 - 1 * math.sin(sine/9)), math.rad(0 + 0 * math.cos(sine/8)), math.rad(0) + Root.RotVelocity.Y / 30, math.cos(10 * math.cos(sine/10))), 0.3)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.3)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.3)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.5,.6,-.5) * CFrame.Angles(math.rad(32),math.rad(5 - .1 * math.sin(sine/12)),math.rad(40 - .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.5,.6,-.5) * CFrame.Angles(math.rad(30),math.rad(-5 + .1 * math.sin(sine/12)),math.rad(-40 + .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(.2,1.2,-.3),.3)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.54, 1.4 + .1 * math.sin(sine/9), .4) * CFrame.Angles(math.rad(9 + 2 * math.cos(sine/9)), math.rad(0), math.rad(0)), 0.3)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.54, 2.0 + .02 * math.sin(sine/9), 0.2 + .1 * math.sin(sine/9)) * CFrame.Angles(math.rad(25 + 5 * math.sin(sine/9)), math.rad(20), math.rad(0)), 0.3)
elseif position == "Walking" and attacking == false and running == false then
change = 1.2
walking = true
tommygunweld.C0 = tommygunweld.C0:lerp(CFrame.new(0,-.80,1.25) * CFrame.Angles(math.rad(98),math.rad(0),0),.25)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.3,1,0) * CFrame.Angles(math.rad(180),math.rad(1),math.rad(10)), 0.1)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.1)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.5 + Root.RotVelocity.Y / 85,.35,.5*math.sin(sine/8)) * CFrame.Angles(math.rad(-35*math.sin(sine/8)),math.rad(0*math.sin(sine/8)),math.rad(10 + Root.RotVelocity.Y / 10, math.sin(20 * math.sin(sine/4)))),.3)
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0.05*math.sin(sine/4), 0) * CFrame.Angles(math.rad(-10), math.rad(5 * math.cos(sine/7)), math.rad(0) + Root.RotVelocity.Y / 30, math.cos(25 * math.cos(sine/10))), 0.1)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 1.92 - 0.35 * math.cos(sine/8)/2.8, 0.2 - math.sin(sine/8)/3.4) * CFrame.Angles(math.rad(10) + -math.sin(sine/8)/2.3, math.rad(0)*math.cos(sine/1), math.rad(0), math.cos(25 * math.cos(sine/8))), 0.3)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.92 + 0.35 * math.cos(sine/8)/2.8, 0.2 + math.sin(sine/8)/3.4) * CFrame.Angles(math.rad(10) - -math.sin(sine/8)/2.3, math.rad(0)*math.cos(sine/1), math.rad(0) , math.cos(25 * math.cos(sine/8))), 0.3)
elseif position == "Idle" and attacking == false and running == false then
change = .5
tommygunweld.C0 = tommygunweld.C0:lerp(CFrame.new(0,-.80,1.25) * CFrame.Angles(math.rad(98),math.rad(0),0),.25)
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, -.2 + -.1 * math.sin(sine/12), 0) * CFrame.Angles(math.rad(0),math.rad(25),math.rad(0)),.1)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.3 + .1 * math.sin(sine/12),1 + .1 * math.sin(sine/12),0) * CFrame.Angles(math.rad(180),math.rad(1),math.rad(8 + 5 * math.sin(sine/12))), 0.1)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.59 - .05 * math.sin(sine/12), 0.1 -.1 * math.sin(sine/12), 0) * CFrame.Angles(math.rad(-2), math.rad(2), math.rad(8  - 6 * math.sin(sine/12))), .2)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.3, 2 - .1 * math.sin(sine/12), 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(-10)), 0.1)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.1)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.3, 2.0 - .1 * math.sin(sine/12), 0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(10)), 0.1)
elseif position == "Idle2" and attacking == false and running == false then
change = .75
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0 - 3 * math.sin(sine/9)),0,0),.1)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.1)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(-.2,.2,0) * CFrame.Angles(0,0,0),.1)
LEFTARMLERP.C1 = CFrame.new(0,0,0) * CFrame.Angles(0,0,0)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.6, 0.8 - .1 * math.sin(sine/9), 0) * CFrame.Angles(math.rad(0), math.rad(0 + 3 * math.sin(sine/9)), math.rad(35 - 5 * math.sin(sine/9))), 0.4)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.6, 0.8 - .1 * math.sin(sine/9), 0) * CFrame.Angles(math.rad(0), math.rad(0 - 3 * math.sin(sine/9)), math.rad(-35 + 5 * math.sin(sine/9))), 0.4)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.54, 1.4 + .1 * math.sin(sine/9), .4) * CFrame.Angles(math.rad(9 + 2 * math.cos(sine/9)), math.rad(0), math.rad(0)), 0.4)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 2.0,0) * CFrame.Angles(math.rad(0), math.rad(0), math.rad(-10 + 2 * math.sin(sine/9))), 0.4)
elseif position == "Walking2" and attacking == false and running == false then
ws = 50
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(-20 - 1 * math.sin(sine/9)), math.rad(0 + 0 * math.cos(sine/8)), math.rad(0) + Root.RotVelocity.Y / 30, math.cos(10 * math.cos(sine/10))), 0.3)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(0,0,0),.3)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,0) * CFrame.Angles(math.rad(0),0,0),.3)
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(-1.5,.6,-.5) * CFrame.Angles(math.rad(32),math.rad(5 - .1 * math.sin(sine/12)),math.rad(40 - .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(1.5,.6,-.5) * CFrame.Angles(math.rad(30),math.rad(-5 + .1 * math.sin(sine/12)),math.rad(-40 + .5 * math.sin(sine/12))), 0.3)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(.2,1.2,-.3),.3)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.54, 1.4 + .1 * math.sin(sine/9), .4) * CFrame.Angles(math.rad(9 + 2 * math.cos(sine/9)), math.rad(0), math.rad(0)), 0.3)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.54, 2.0 + .02 * math.sin(sine/9), 0.2 + .1 * math.sin(sine/9)) * CFrame.Angles(math.rad(25 + 5 * math.sin(sine/9)), math.rad(20), math.rad(0)), 0.3)
elseif position == "Running" and attacking == false then
change = 1
RIGHTARMLERP.C0 = RIGHTARMLERP.C0:lerp(CFrame.new(0, .5, 0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0)), 0.3)
LEFTARMLERP.C1 = LEFTARMLERP.C1:lerp(CFrame.new(-1.24+.6*math.sin(sine/4)/1.4, 0.54, 0-0.8*math.sin(sine/4))*CFrame.Angles(math.rad(6+140*math.sin(sine/4)/1.2), math.rad(0), math.rad(20+70*math.sin(sine/4))), 0.3)
LEFTARMLERP.C0 = LEFTARMLERP.C0:lerp(CFrame.new(0,.5,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0)),.3)
ROOTLERP.C0 = ROOTLERP.C0:lerp(CFrame.new(0, -.2, 0) * CFrame.Angles(math.rad(-20 - 0 * math.sin(sine/4)), math.rad(0 + 6 * math.sin(sine/4)), math.rad(0) + Root.RotVelocity.Y / 30, math.sin(10 * math.sin(sine/4))), 0.3)
RIGHTLEGLERP.C1 = RIGHTLEGLERP.C1:lerp(CFrame.new(0,0,-.2 + .5*-math.sin(sine/4)),.3)
RIGHTLEGLERP.C0 = RIGHTLEGLERP.C0:lerp(CFrame.new(-0.5, 1.6+0.1*math.sin(sine/4),.7*-math.sin(sine/4)) * CFrame.Angles(math.rad(15+ -50 * math.sin(sine/4)),0,0),.3)
LEFTLEGLERP.C1 = LEFTLEGLERP.C1:lerp(CFrame.new(0,0,-.2 + .5*math.sin(sine/4)),.3)
LEFTLEGLERP.C0 = LEFTLEGLERP.C0:lerp(CFrame.new(0.5, 1.6-0.1*math.sin(sine/4),.7*math.sin(sine/4)) * CFrame.Angles(math.rad(15 + 50 * math.sin(sine/4)),0,0),.3)
end
swait()
end
end)
anims()("Clicked")
end)


section2:addButton("FE Car", function()
loadstring(game:HttpGet('https://raw.githubusercontent.com/MonkoTubeYT/carscript/master/!carscript.lua',true))()("Clicked")
end)


section2:addButton("FE Spider", function()
	setsimulationradius(math.huge, math.huge)

local mouse = game.Players.LocalPlayer:GetMouse()

game.Players.LocalPlayer.Character.Archivable = true
game.Players.LocalPlayer.Character.Animate.Disabled = true
local clonec =  game.Players.LocalPlayer.Character:Clone()
clonec.Parent = workspace
clonec.Name = "POOCLONE"
clonec.Humanoid.HipHeight = -1.2 -- change this to look taller.
game.Players.LocalPlayer.Character = clonec
clonec.Animate.Disabled = false

workspace.Camera.CameraSubject = clonec.Humanoid
game.Players.LocalPlayer.Character = workspace[game.Players.LocalPlayer.Name]
game.Players.LocalPlayer.Character.Animate.Disabled = true
---game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = true
game.Players.LocalPlayer.Character.Humanoid.Animator:Destroy()

spawn(function()


while true do
if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
clonec.Humanoid.Jump = game.Players.LocalPlayer.Character.Humanoid.Jump

local veco = workspace.Camera.CFrame:VectorToObjectSpace(game.Players.LocalPlayer.Character.Humanoid.MoveDirection)
clonec.Humanoid:Move(veco, true)

end
wait()
end

end)

for i,v in pairs(clonec:GetDescendants())do 
    
    if v:IsA("Part") then 
    v.Transparency = 1
    end 
end 





local bodyvelocity = Instance.new("BodyVelocity",game.Players.LocalPlayer.Character["HumanoidRootPart"])
bodyvelocity.MaxForce = Vector3.new(9.9999999805064e+18, 9.999999869911e+14, 9.999999869911e+14)
bodyvelocity.Velocity = Vector3.new(0, 0, 0)
game:GetService("RunService").Stepped:connect(function()
    
    game.Players.LocalPlayer.Character.Torso.CanCollide = false 
    game.Players.LocalPlayer.Character.Head.CanCollide = false 
        game.Players.LocalPlayer.Character.HumanoidRootPart.CanCollide = false 
       game.Players.LocalPlayer.Character.Humanoid.PlatformStand = true  
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = (clonec.HumanoidRootPart.CFrame * CFrame.Angles(math.rad(-90),0,0)) * CFrame.new(0,-0,-1)
           game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
 game.Players.LocalPlayer.Character.HumanoidRootPart.RotVelocity = Vector3.new(0,0,0)
 
end)




local segments = Instance.new("Folder")
local part = Instance.new("Part")
local part_2 = Instance.new("Part")

segments.Name = "segments"
segments.Parent = workspace
part.Anchored = true
part.CanCollide = false
part.Transparency = 1
part.Size = Vector3.new(1, 1, 2)
part.BottomSurface = Enum.SurfaceType.Smooth
part.BrickColor = BrickColor.new("Alder")
part.TopSurface = Enum.SurfaceType.Smooth
part.Color = Color3.new(0.666667, 0.333333, 1)
part.Parent = segments
part.Name = "seg1"
part.CFrame = CFrame.new(-4.1, 2.1, -37.5)
part_2.Anchored = true
part_2.CanCollide = false
part_2.Size = Vector3.new(1, 1, 2)
part_2.BottomSurface = Enum.SurfaceType.Smooth
part_2.BrickColor = BrickColor.new("Cool yellow")
part_2.TopSurface = Enum.SurfaceType.Smooth
part_2.Color = Color3.new(0.992157, 0.917647, 0.552941)
part_2.Parent = segments
part_2.CFrame = CFrame.new(-4.1, 2.1, -37.5)
part_2.Name = "seg2"
part_2.Transparency = 1

local segments2 = Instance.new("Folder")
local part = Instance.new("Part")
local part_2 = Instance.new("Part")

segments2.Name = "segments2"
segments2.Parent = workspace
part.Anchored = true
part.CanCollide = false
part.Size = Vector3.new(1, 1, 2)
part.BottomSurface = Enum.SurfaceType.Smooth
part.BrickColor = BrickColor.new("Alder")
part.TopSurface = Enum.SurfaceType.Smooth
part.Name = "seg1"
part.Color = Color3.new(0.666667, 0.333333, 1)
part.Parent = segments2
part.CFrame = CFrame.new(-4.1, 2.1, -37.5)
part_2.Anchored = true
part_2.CanCollide = false
part_2.Size = Vector3.new(1, 1, 2)
part_2.BottomSurface = Enum.SurfaceType.Smooth
part_2.BrickColor = BrickColor.new("Alder")
part_2.TopSurface = Enum.SurfaceType.Smooth
part_2.Color = Color3.new(0.666667, 0.333333, 1)
part_2.Parent = segments2
part_2.CFrame = CFrame.new(-4.1, 2.1, -37.5)
part_2.Name = "seg2"
part_2.Transparency = 1
part.Transparency = 1



local leg1 = Instance.new("Part")
leg1.Anchored = true
leg1.Size = Vector3.new(0.5, 0.2, 0.5)
leg1.BottomSurface = Enum.SurfaceType.Smooth
leg1.Color = Color3.new(0, 1, 0)
leg1.BrickColor = BrickColor.new("New Yeller")
leg1.TopSurface = Enum.SurfaceType.Smooth
leg1.Name = "leg1"
leg1.Parent = workspace
leg1.CFrame = CFrame.new(-31.15, 0.1, 8.65)
leg1.CanCollide = false
leg1.Transparency = 1





local leg1 =workspace.leg1:Clone()
leg1.Parent = workspace

local leg2= workspace.leg1:Clone()
leg2.Parent = workspace

local lp = game.Players.LocalPlayer
local head = game.Players.LocalPlayer.Character.Head

function coffset(x,y,z)
	return (head.CFrame * CFrame.new(x,y,z)).Position
end




mouse.KeyDown:connect(function(k)
	
	if k == "z" then
		
		leg1.Position = mouse.Hit.Position
	elseif k == "x" then
		
		
		leg2.Position = mouse.Hit.Position
	end
	
end)

	

		
spawn(function()
--
while true do
	
	
if game.Players.LocalPlayer.Character.Humanoid.MoveDirection.Magnitude >0.1 then
		wait(1.6/lp.Character.Humanoid.WalkSpeed)
		
	local ray1 =Ray.new(coffset(3,-0,0),Vector3.new(0,-10,0) )
	local hit,pos = workspace:FindPartOnRayWithIgnoreList(ray1,{leg1,leg2,lp.Character})
	if pos then
		leg1.Position = pos
		end
		

	
		wait(1.6/lp.Character.Humanoid.WalkSpeed)
	local ray2 =Ray.new(coffset(-3,-0,0),Vector3.new(0,-10,0) )
	local hit,pos = workspace:FindPartOnRayWithIgnoreList(ray2,{leg1,leg2,lp.Character})
	if pos then
	leg2.Position = pos	
		end
	
	end
	game:GetService("RunService").RenderStepped:wait()
end

end)


  

spawn(function()

local mouse = game.Players.LocalPlayer:GetMouse()



local len  = 2

local offset = Vector3.new(1,-3,0)
	
	local offset = Vector3.new(1,-1,0)
	
local segs = {}

local posn =  game.Players.LocalPlayer.Character.Head.Position + Vector3.new(0,-2.5,0)






for i,v in pairs(workspace.segments:GetChildren()) do
	
	
	
	table.insert(segs,v)
	
	
end






function vectorabsy(vec)
	local v = Vector3.new(vec.X,math.abs(vec.Y),vec.Z)
	return v
end


local count = #segs


while true do
	
	for i = 1,5 do
		
	for i = 1,count do
		
		if i == 1 then
			
			local seg = segs[i]
			
			local pos1 = 	segs[i].Position - (segs[i].CFrame.LookVector* (len/2) )  -- Calculating position that is on back of the part
			local pos2 =leg1.Position
			local vec = (pos2 - pos1).Unit 
			
			local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) 
			
			seg.CFrame = cframe
			
		else
				local seg = segs[i]
			local pos1 = 	segs[i].Position - (segs[i].CFrame.LookVector* (len/2) )
			local pos2 = 	segs[i-1].Position - (segs[i-1].CFrame.LookVector* (len/2) )
			local vec = (pos2 - pos1).Unit
				local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) 
			
			seg.CFrame = cframe
		end

	end	
	
	--Back

	
		for i = 1,count do
		
		local i = ( count - i ) + 1
		if i == count then
			
			local seg = segs[i]
			
			local pos1 = 	segs[i].Position + (segs[i].CFrame.LookVector* (len/2) )  -- Calculating position that is on back of the part
			local pos2 =(game.Players.LocalPlayer.Character.Head.CFrame * CFrame.new(offset)).Position  
			local vec =(pos2 - pos1).Unit 
			if vec.Y > 0 then
				
			vec = Vector3.new(vec.X, vec.Y-0.01 ,vec.Z)	
				
			end
			
			local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) * CFrame.Angles(0,math.rad(-180),0) 
			
			seg.CFrame =cframe
			
		else
				local seg = segs[i]
			local pos1 = 	segs[i].Position + (segs[i].CFrame.LookVector* (len/2) )
			
			local pos2 = 	segs[i+1].Position + (segs[i+1].CFrame.LookVector* (len/2) )
			local vec = (pos2 - pos1).Unit 
			
				local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2)  * CFrame.Angles(0,math.rad(-180),0)
			
			seg.CFrame = cframe
		end

		end	
		
	end
	game:GetService("RunService").Heartbeat:wait()
end
	
end)




spawn(function()

local mouse = game.Players.LocalPlayer:GetMouse()



local len  = 2

local offset = Vector3.new(-1,-1,0)

local segs = {}

local posn =  game.Players.LocalPlayer.Character.Head.Position + Vector3.new(0,-2.5,0)






for i,v in pairs(workspace.segments2:GetChildren()) do
	
	

	table.insert(segs,v)
	
	
end





function vectorabsy(vec)
	local v = Vector3.new(vec.X,math.abs(vec.Y),vec.Z)
	return v
end


local count = #segs


while true do

	for i = 1,5 do
		
	for i = 1,count do
		
		if i == 1 then
			
			local seg = segs[i]
			
			local pos1 = 	segs[i].Position - (segs[i].CFrame.LookVector* (len/2) )  -- Calculating position that is on back of the part
			local pos2 =leg2.Position
			local vec = (pos2 - pos1).Unit 
		
			local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) 
			
			seg.CFrame = cframe
			
		else
				local seg = segs[i]
			local pos1 = 	segs[i].Position - (segs[i].CFrame.LookVector* (len/2) )
			local pos2 = 	segs[i-1].Position - (segs[i-1].CFrame.LookVector* (len/2) )
			local vec = (pos2 - pos1).Unit
				local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) 
			
			seg.CFrame = cframe
		end

	end	
	
	--Back

	
		for i = 1,count do
		
		local i = ( count - i ) + 1
		if i == count then
			
			local seg = segs[i]
			
			local pos1 = 	segs[i].Position + (segs[i].CFrame.LookVector* (len/2) )  -- Calculating position that is on back of the part
			local pos2 =(game.Players.LocalPlayer.Character.Head.CFrame * CFrame.new(offset)).Position  
			local vec =(pos2 - pos1).Unit 
			if vec.Y > 0 then
				
			vec = Vector3.new(vec.X, vec.Y-0.01 ,vec.Z)	
				
			end
			
			local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2) * CFrame.Angles(0,math.rad(-180),0) 
			
			seg.CFrame =cframe
			
		else
				local seg = segs[i]
			local pos1 = 	segs[i].Position + (segs[i].CFrame.LookVector* (len/2) )
			
			local pos2 = 	segs[i+1].Position + (segs[i+1].CFrame.LookVector* (len/2) )
			local vec = (pos2 - pos1).Unit 
			
				local cframe = CFrame.new(pos2 - (vec*(len/2) ),pos2)  * CFrame.Angles(0,math.rad(-180),0)
			
			seg.CFrame = cframe
		end

		end	
		
	end
	game:GetService("RunService").Heartbeat:wait()
end
	
end)


game.Players.LocalPlayer.Character.Torso["Right Shoulder"]:Destroy()
game.Players.LocalPlayer.Character.Torso["Left Shoulder"]:Destroy()
game.Players.LocalPlayer.Character.Torso["Right Hip"]:Destroy()
game.Players.LocalPlayer.Character.Torso["Left Hip"]:Destroy()


	
local bodyvelocity = Instance.new("BodyVelocity",game.Players.LocalPlayer.Character["Right Arm"])
bodyvelocity.MaxForce = Vector3.new(9.9999999805064e+18, 9.999999869911e+14, 9.999999869911e+14)
bodyvelocity.Velocity = Vector3.new(0, 200, 0)

local bodyvelocity = Instance.new("BodyVelocity",game.Players.LocalPlayer.Character["Left Arm"])
bodyvelocity.MaxForce = Vector3.new(9.9999999805064e+18, 9.999999869911e+14, 9.999999869911e+14)
bodyvelocity.Velocity = Vector3.new(0, 200, 0)

local bodyvelocity = Instance.new("BodyVelocity",game.Players.LocalPlayer.Character["Left Leg"])
bodyvelocity.MaxForce = Vector3.new(9.9999999805064e+18, 9.999999869911e+14, 9.999999869911e+14)
bodyvelocity.Velocity = Vector3.new(0, 200, 0)

local bodyvelocity = Instance.new("BodyVelocity",game.Players.LocalPlayer.Character["Right Leg"])
bodyvelocity.MaxForce = Vector3.new(9.9999999805064e+18, 9.999999869911e+14, 9.999999869911e+14)
bodyvelocity.Velocity = Vector3.new(0, 200, 0)

spawn(function()
	

	game.Players.LocalPlayer.Character.Humanoid.Died:connect(function()
		
		segments:Destroy()
		segments2:Destroy()
		
	end)
	
	game:GetService("RunService").Stepped:connect(function()
	  	game.Players.LocalPlayer.Character["Right Arm"].CanCollide = false
	game.Players.LocalPlayer.Character["Left Arm"].CanCollide = false
		
	 game.Players.LocalPlayer.Character["Right Leg"].CanCollide = false
	game.Players.LocalPlayer.Character["Left Leg"].CanCollide = false

	   end)
	
	repeat game:GetService("RunService").Heartbeat:wait()
		
	game.Players.LocalPlayer.Character["Right Arm"].CFrame = 	segments.seg1 .CFrame * CFrame.Angles(math.rad(90),0,0 )
	game.Players.LocalPlayer.Character["Left Arm"].CFrame = 	segments.seg2.CFrame * CFrame.Angles(math.rad(90),0,0 )
		
	 game.Players.LocalPlayer.Character["Right Leg"].CFrame = 	segments2.seg1.CFrame * CFrame.Angles(math.rad(90),0,0 )
	game.Players.LocalPlayer.Character["Left Leg"].CFrame = 	segments2.seg2.CFrame * CFrame.Angles(math.rad(90),0,0 )
	
	until game.Players.LocalPlayer.Character.Humanoid.Health  < 1

		
		
		
	
	
end)

--thisisascript("Clicked")
	end)

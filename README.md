local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local decalId = "rbxassetid://123713551750774"
local musicId = "rbxassetid://1847661821"
local volume = 10
local pitch = 1

print("S34lify script v2 loaded! Applying...")

-- 1. SKYBOX: Create massive invisible box around server
local skybox = Instance.new("Part")
skybox.Name = "S34Skybox"
skybox.Size = Vector3.new(50000, 50000, 50000)  -- Huge to enclose everything
skybox.Position = Workspace:GetRealPhysicsFPS() * Vector3.new(0, 0, 0)  -- Center
skybox.Anchored = true
skybox.CanCollide = false
skybox.Transparency = 1  -- Invisible
skybox.Parent = Workspace

local faces = {"Front", "Back", "Left", "Right", "Top", "Bottom"}
for _, face in pairs(faces) do
    local decal = Instance.new("Decal")
    decal.Texture = decalId
    decal.Face = Enum.NormalId[face]
    decal.Parent = skybox
end

-- Override Lighting sky too for safety
if Lighting:FindFirstChild("Sky") then
    Lighting.Sky:Destroy()
end
local newSky = Instance.new("Sky")
newSky.SkyboxBk = decalId
newSky.SkyboxDn = decalId
newSky.SkyboxFt = decalId
newSky.SkyboxLf = decalId
newSky.SkyboxRt = decalId
newSky.SkyboxUp = decalId
newSky.Parent = Lighting

-- 2. DECALS: Smart spam - only large/visible parts (no lag)
local function applyDecal(part)
    if part:IsA("BasePart") and part.Size.Magnitude > 4 and not part:IsDescendantOf(player.Character) and part.Parent ~= skybox then
        for _, faceName in pairs(faces) do
            local face = Enum.NormalId[faceName]
            if part:FindFirstChildOfClass("Decal") == nil or math.random(1, 3) == 1 then  -- Sparse to avoid overload
                local decal = Instance.new("Decal")
                decal.Texture = decalId
                decal.Face = face
                decal.Transparency = 0.2  -- Slight tint
                decal.Parent = part
            end
        end
    end
end

for _, obj in pairs(Workspace:GetDescendants()) do
    applyDecal(obj)
end

-- Ongoing: New parts
RunService.Heartbeat:Connect(function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and #obj:GetChildren() == 0 then  -- Fresh parts
            applyDecal(obj)
        end
    end
end)

-- 3. MUSIC: Override all sounds
for _, obj in pairs(Workspace:GetDescendants()) do
    if obj:IsA("Sound") then
        obj.SoundId = musicId
        obj.Volume = volume
        obj.Pitch = pitch  -- "normal" = 1, but settable
        obj.Looped = true
        obj:Play()
    end
end
for _, obj in pairs(Lighting:GetDescendants()) do
    if obj:IsA("Sound") then
        obj.SoundId = musicId
        obj.Volume = volume
        obj.Pitch = pitch
        obj.Looped = true
        obj:Play()
    end
end

-- New sounds
RunService.Heartbeat:Connect(function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Sound") and obj.SoundId ~= musicId then
            obj.SoundId = musicId
            obj.Volume = volume
            obj.Pitch = pitch
            obj.Looped = true
            obj:Play()
        end
    end
end)

print("S34lify v2 complete!"
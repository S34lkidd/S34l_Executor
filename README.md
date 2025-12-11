-- Delta / Mobile Exploit Fix (2025)
ScreenGui.Parent = game:GetService("CoreGui")

-- Fallback for exploits that block CoreGui (Delta, Hydrogen, Arceus X, etc.)
spawn(function()
    wait(0.1)
    if not ScreenGui.Parent or not ScreenGui:FindFirstChild("Frame") then
        ScreenGui.Parent = player:WaitForChild("PlayerGui") -- Fallback to PlayerGui
        notify("Fallback Mode", "CoreGui blocked • Using PlayerGui", false)
    end
end)
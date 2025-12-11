loadstring(function()
    local UIS = game:GetService("UserInputService")
    local TS = game:GetService("TweenService")
    local Players = game:GetService("Players")
    local player = Players.LocalPlayer

    local isMobile = UIS.TouchEnabled and not UIS.MouseEnabled

    local function rainbow()
        return Color3.fromHSV(tick() % 5 / 5, 1, 1)
    end

    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "S34lkiddExecutor"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = game:GetService("CoreGui")

    local Frame = Instance.new("Frame")
    Frame.Size = isMobile and UDim2.new(0, 560, 0, 480) or UDim2.new(0, 540, 0, 400)
    Frame.Position = UDim2.new(0.5, 0, 0.5, 0)
    Frame.AnchorPoint = Vector2.new(0.5, 0.5)
    Frame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
    Frame.BorderSizePixel = 0
    Frame.Parent = ScreenGui

    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 18)

    local Border = Instance.new("UIStroke")
    Border.Thickness = 5
    Border.Color = Color3.fromRGB(255, 0, 255)
    Border.Parent = Frame

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, -40, 0, 50)
    Title.Position = UDim2.new(0, 20, 0, 10)
    Title.BackgroundTransparency = 1
    Title.Text = "S34lkidd's Executor"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = isMobile and 34 or 28
    Title.Font = Enum.Font.GothamBlack
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Frame

    local ScriptBox = Instance.new("TextBox")
    ScriptBox.Size = UDim2.new(1, -40, 1, isMobile and -160 or -130)
    ScriptBox.Position = UDim2.new(0, 20, 0, isMobile and 70 or 65)
    ScriptBox.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
    ScriptBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    ScriptBox.PlaceholderText = "Enter your script here..."
    ScriptBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
    ScriptBox.Text = ""
    ScriptBox.TextSize = isMobile and 18 or 14
    ScriptBox.Font = Enum.Font.Code
    ScriptBox.MultiLine = true
    ScriptBox.ClearTextOnFocus = false
    ScriptBox.TextWrapped = true
    ScriptBox.TextXAlignment = Enum.TextXAlignment.Left
    ScriptBox.TextYAlignment = Enum.TextYAlignment.Top
    ScriptBox.Parent = Frame

    Instance.new("UICorner", ScriptBox).CornerRadius = UDim.new(0, 14)

    local buttons = {"Execute", "Clear", "Paste"}
    local callbacks = {
        function()
            if ScriptBox.Text == "" then notify("Empty", "Nothing to execute!", false) return end
            local func, err = loadstring(ScriptBox.Text)
            if func then
                pcall(func)
                notify("Success", "Script executed!", true)
            else
                notify("Error", err or "Loadstring failed", false)
            end
        end,
        function() ScriptBox.Text = "" notify("Cleared", "Script box is empty", true) end,
        function()
            local clip = getclipboard and getclipboard() or (syn and syn.request({Url="http://localhost:8080/",Method="GET"}).Body) or ""
            if clip ~= "" then ScriptBox.Text = clip notify("Pasted", "Clipboard → Script Box", true)
            else notify("Failed", "Clipboard is empty", false) end
        end
    }

    local btns = {}
    for i, name in ipairs(buttons) do
        local btn = Instance.new("TextButton")
        btn.Size = isMobile and UDim2.new(0.9, 0, 0, 58) or UDim2.new(0, 160, 0, 48)
        btn.Position = isMobile and UDim2.new(0.5, 0, 1, -200 + (i-1)*70) or UDim2.new(0, 20 + (i-1)*180, 1, -70)
        btn.AnchorPoint = Vector2.new(0.5, 0)
        btn.BackgroundColor3 = Color3.fromRGB(35, 35,35)
        btn.Text = name
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.TextSize = isMobile and 26 or 20
        btn.Font = Enum.Font.GothamBold
        btn.Parent = Frame

        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 14)
        local stroke = Instance.new("UIStroke", btn)
        stroke.Thickness = 4
        stroke.Color = Color3.fromRGB(255, 0, 255)

        btn.MouseButton1Click:Connect(callbacks[i])
        table.insert(btns, {btn = btn, stroke = stroke})
    end

    -- Rainbow Effect
    spawn(function()
        while task.wait(0.03) do
            local c = rainbow()
            Border.Color = c
            Title.TextColor3 = c
            ScriptBox.TextColor3 = c
            for _, b in btns do
                b.btn.TextColor3 = c
                b.stroke.Color = c
            end
        end
    end)

    -- Notification
    function notify(title, text, success)
        local n = Instance.new("Frame")
        n.Size = UDim2.new(0, 320, 0, 90)
        n.Position = UDim2.new(0, 20, 0, -100)
        n.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        n.Parent = ScreenGui

        Instance.new("UICorner", n).CornerRadius = UDim.new(0, 14)
        local s = Instance.new("UIStroke", n)
        s.Thickness = 3
        s.Color = success and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,50,50)

        local t1 = Instance.new("TextLabel", n)
        t1.Size = UDim2.new(1, -20, 0.5, 0)
        t1.Position = UDim2.new(0, 10, 0, 8)
        t1.BackgroundTransparency = 1
        t1.Text = title
        t1.TextColor3 = success and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,80,80)
        t1.TextSize = 24
        t1.Font = Enum.Font.GothamBold

        local t2 = Instance.new("TextLabel", n)
        t2.Size = UDim2.new(1, -20, 0.5, 0)
        t2.Position = UDim2.new(0, 10, 0.5, -5)
        t2.BackgroundTransparency = 1
        t2.Text = text
        t2.TextColor3 = Color3.fromRGB(200,200,200)
        t2.TextSize = 17

        TS:Create(n, TweenInfo.new(0.4), {Position = UDim2.new(0, 20, 0, 20)}):Play()
        task.wait(3)
        TS:Create(n, TweenInfo.new(0.4), {Position = UDim2.new(0, 20, 0, -100)}):Play()
        task.wait(0.5)
        n:Destroy()
    end

    -- Draggable (works on mobile too)
    local dragging, dragStart, startPos
    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = Frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)

    Frame.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    -- Delta / Mobile CoreGui fallback
    task.wait(0.2)
    if not Frame.Parent then
        ScreenGui.Parent = player:WaitForChild("PlayerGui")
        notify("Fallback", "CoreGui blocked → PlayerGui", false)
    end

    notify("Loaded!", "S34lkidd's Executor • Rainbow ON", true)
end)()
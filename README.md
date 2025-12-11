-- S34lkidd's Executor - Fully Standalone (No External UI Lib) - Mobile + PC
-- Rainbow • Touch-Friendly • Clipboard • 2025

loadstring(function()
    local UIS = game:GetService("UserInputService")
    local TS = game:GetService("TweenService")
    local Players = game:GetService("Players")
    local player = Players.LocalPlayer
    local mouse = player:GetMouse()

    local isMobile = UIS.TouchEnabled and not UIS.MouseEnabled

    -- Rainbow Function
    local function rainbow()
        return Color3.fromHSV(tick() % 5 / 5, 1, 1)
    end

    -- ScreenGui
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "S34lkiddExecutor"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = game.CoreGui

    -- Main Frame
    local Frame = Instance.new("Frame")
    Frame.Size = isMobile and UDim2.new(0, 560, 0, 480) or UDim2.new(0, 540, 0, 400)
    Frame.Position = UDim2.new(0.5, 0, 0.5, 0)
    Frame.AnchorPoint = Vector2.new(0.5, 0.5)
    Frame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
    Frame.BorderSizePixel = 0
    Frame.Parent = ScreenGui

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 18)
    Corner.Parent = Frame

    local Border = Instance.new("UIStroke")
    Border.Thickness = 5
    Border.Color = Color3.fromRGB(255, 0, 255)
    Border.Parent = Frame

    -- Title
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

    -- Script Box
    local ScriptBox = Instance.new("TextBox")
    ScriptBox.Size = UDim2.new(1, -40, 1, isMobile and -160 or -130)
    ScriptBox.Position = UDim2.new(0, 20, 0, isMobile and 70 or 65)
    ScriptBox.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
    ScriptBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    ScriptBox.PlaceholderText = "Enter loadstring(script) here..."
    ScriptBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
    ScriptBox.Text = ""
    ScriptBox.TextSize = isMobile and 18 or 14
    ScriptBox.Font = Enum.Font.Code
    ScriptBox.TextXAlignment = Enum.TextXAlignment.Left
    ScriptBox.TextYAlignment = Enum.TextYAlignment.Top
    ScriptBox.MultiLine = true
    ScriptBox.ClearTextOnFocus = false
    ScriptBox.TextWrapped = true
    ScriptBox.Parent = Frame

    local BoxCorner = Instance.new("UICorner")
    BoxCorner.CornerRadius = UDim.new(0, 14)
    BoxCorner.Parent = ScriptBox

    -- Buttons (Mobile = Vertical, PC = Horizontal)
    local btnSize = isMobile and UDim2.new(0, 460, 0, 58) or UDim2.new(0, 140, 0, 48)
    local btnY = isMobile and {-200, -130, -60} or {-70, -70, -70}
    local btnX = isMobile and {0.5, 0} or {1, -170, 0, 30, 0.5, -70}

    local buttons = {"Execute", "Clear", "Paste from Clipboard"}
    local callbacks = {
        function()
            if ScriptBox.Text ~= "" then
                local s, e = pcall(loadstring(ScriptBox.Text))
                notify(s and "Success!" or "Error", s and "Script executed" or e, s)
            else
                notify("Empty", "No script to execute!", false)
            end
        end,
        function() ScriptBox.Text = "" notify("Cleared", "Script box is now empty", true) end,
        function()
            local clip = (syn and syn.request and syn.request({Url="http://localhost:8080/",Method="GET"}).Body) or (getclipboard and getclipboard()) or ""
            if clip ~= "" then ScriptBox.Text = clip notify("Pasted!", "Clipboard loaded", true) else notify("Failed", "Clipboard empty", false) end
        end
    }

    local btns = {}
    for i, name in ipairs(buttons) do
        local btn = Instance.new("TextButton")
        btn.Size = btnSize
        btn.Position = isMobile and UDim2.new(0.5, 0, 1, btnY[i]) or UDim2.new(btnX[i*2-1], btnX[i*2], 1, btnY[1])
        btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
        btn.Text = name
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.TextSize = isMobile and 26 or 20
        btn.Font = Enum.Font.GothamBold
        btn.Parent = Frame

        local c = Instance.new("UICorner", btn)
        c.CornerRadius = UDim.new(0, 14)

        local stroke = Instance.new("UIStroke", btn)
        stroke.Thickness = 4
        stroke.Color = Color3.fromRGB(255, 0, 255)

        btn.MouseButton1Click:Connect(callbacks[i])
        table.insert(btns, {btn = btn, stroke = stroke})
    end

    -- Rainbow Effects
    spawn(function()
        while wait(0.03) do
            local color = rainbow()
            Border.Color = color
            Title.TextColor3 = color
            ScriptBox.TextColor3 = color

            for _, b in btns do
                b.btn.TextColor3 = color
                b.stroke.Color = color
            end
        end
    end)

    -- Simple Notification
    function notify(title, text, success)
        local notif = Instance.new("Frame")
        notif.Size = UDim2.new(0, 300, 0, 80)
        notif.Position = UDim2.new(0, 20, 0, -100)
        notif.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        notif.Parent = ScreenGui

        local nc = Instance.new("UICorner", notif)
        nc.CornerRadius = UDim.new(0, 12)

        local ns = Instance.new("UIStroke", notif)
        ns.Thickness = 3

        local t1 = Instance.new("TextLabel", notif)
        t1.Size = UDim2.new(1, -20, 0.5, 0)
        t1.Position = UDim2.new(0, 10, 0, 5)
        t1.BackgroundTransparency = 1
        t1.Text = title
        t1.TextColor3 = success and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,50,50)
        t1.TextSize = 22
        t1.Font = Enum.Font.GothamBold

        local t2 = Instance.new("TextLabel", notif)
        t2.Size = UDim2.new(1, -20, 0.5, 0)
        t2.Position = UDim2.new(0, 10, 0.5, 0)
        t2.BackgroundTransparency = 1
        t2.Text = text
        t2.TextColor3 = Color3.fromRGB(200,200,200)
        t2.TextSize = 16

        TS:Create(notif, TweenInfo.new(0.5), {Position = UDim2.new(0, 20, 0, 20)}):Play()
        wait(3)
        TS:Create(notif, TweenInfo.new(0.5), {Position = UDim2.new(0, 20, 0, -100)}):Play()
        wait(0.6)
        notif:Destroy()
    end

    -- Draggable
    local dragging = false
    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            local conn
            conn = UIS.InputChanged:Connect(function(i)
                if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
                    local delta = i.Position - mouse.X < 10000 and i.Position or mouse.X + Vector2.new(100,100)
                    Frame.Position = UDim2.new(0, Frame.Position.X.Offset + delta.X, 0, Frame.Position.Y.Offset + delta.Y)
                end
            end)
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                    if conn then conn:Disconnect() end
                end
            end)
        end
    end)

    notify("S34lkidd's Executor", "Loaded • Mobile & PC • Rainbow Mode ON", true)
end)()
local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local TARGET_CUBE = workspace.Robberys.Objects:GetChildren()[15]
local PROMPT = TARGET_CUBE:WaitForChild("Cube.096"):WaitForChild("ProximityPrompt")

local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
ScreenGui.Name = "AutoFarmGui"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 330, 0, 320)
Frame.Position = UDim2.new(0.78, 0, 0.1, 0)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
Title.Text = "🔧 SATU HUB V. VIP"
Title.TextColor3 = Color3.fromRGB(255, 255, 100)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

local Toggle = Instance.new("TextButton", Frame)
Toggle.Size = UDim2.new(0.8, 0, 0, 40)
Toggle.Position = UDim2.new(0.1, 0, 0.12, 0)
Toggle.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
Toggle.Text = "START (OFF)"
Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
Toggle.Font = Enum.Font.Gotham
Toggle.TextSize = 16

local StatusLabel = Instance.new("TextLabel", Frame)
StatusLabel.Size = UDim2.new(0.8, 0, 0, 30)
StatusLabel.Position = UDim2.new(0.1, 0, 0.29, 0)
StatusLabel.Text = "🟢 พร้อมทำงาน"
StatusLabel.BackgroundTransparency = 1
StatusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 14

local LogBox = Instance.new("ScrollingFrame", Frame)
LogBox.Size = UDim2.new(0.9, 0, 0.45, 0)
LogBox.Position = UDim2.new(0.05, 0, 0.45, 0)
LogBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
LogBox.BorderSizePixel = 1
LogBox.CanvasSize = UDim2.new(0, 0, 1, 0)
LogBox.AutomaticCanvasSize = Enum.AutomaticSize.Y
LogBox.ScrollBarThickness = 6

local LogLayout = Instance.new("UIListLayout", LogBox)
LogLayout.SortOrder = Enum.SortOrder.LayoutOrder
LogLayout.Padding = UDim.new(0, 4)

local ClearButton = Instance.new("TextButton", Frame)
ClearButton.Size = UDim2.new(0.4, 0, 0, 25)
ClearButton.Position = UDim2.new(0.05, 0, 0.91, 0)
ClearButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
ClearButton.Text = "🧹 ล้าง Log"
ClearButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ClearButton.Font = Enum.Font.Gotham
ClearButton.TextSize = 13

ClearButton.MouseButton1Click:Connect(function()
    for _, child in ipairs(LogBox:GetChildren()) do
        if child:IsA("TextLabel") then
            child:Destroy()
        end
    end
end)

local Credit = Instance.new("TextLabel", Frame)
Credit.Size = UDim2.new(0.5, 0, 0, 25)
Credit.Position = UDim2.new(0.55, 0, 0.91, 0)
Credit.Text = "SATU HUB"
Credit.TextColor3 = Color3.fromRGB(200, 200, 200)
Credit.BackgroundTransparency = 1
Credit.Font = Enum.Font.Gotham
Credit.TextSize = 15

local function addLog(msg, color)
    local label = Instance.new("TextLabel")
    label.Text = os.date("[%H:%M:%S] ") .. msg
    label.TextColor3 = color or Color3.fromRGB(255, 255, 255)
    label.Size = UDim2.new(1, -10, 0, 20)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Code
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = LogBox
end

local function MultiPressPrompt()
    for i = 1, 30 do
        StatusLabel.Text = "⏳ กดรอบที่ " .. i
        addLog("กดรอบที่ " .. i, Color3.fromRGB(255, 255, 255))
        fireproximityprompt(PROMPT)
        task.wait(0.5)  -- เปลี่ยนจาก 1 วินาทีเป็น 0.5 วินาที
    end
    return true
end

local Status = false

Toggle.MouseButton1Click:Connect(function()
    Status = not Status
    Toggle.Text = Status and "STOP (ON)" or "START (OFF)"
    Toggle.BackgroundColor3 = Status and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(170, 0, 0)
    StatusLabel.Text = Status and "🔴 กำลังทำงาน..." or "🟢 พร้อมทำงาน"

    if Status then  
        addLog("เริ่มทำงานแล้ว", Color3.fromRGB(0, 255, 0))  
    else  
        addLog("หยุดการทำงาน", Color3.fromRGB(255, 0, 0))  
    end
end)

coroutine.wrap(function()
    while task.wait() do
        if Status then
            local targetPart = TARGET_CUBE:FindFirstChild("Cube.096")
            if not targetPart then
                StatusLabel.Text = "⚠️ ไม่พบ Cube.096!"
                addLog("ไม่พบ Cube.096", Color3.fromRGB(255, 100, 100))
                task.wait(5)
                continue
            end

            HumanoidRootPart.CFrame = targetPart.CFrame * CFrame.new(0, 3, 0)  
            StatusLabel.Text = "✈️ วาร์ปแล้ว"  
            addLog("วาร์ปไปยัง Cube.096", Color3.fromRGB(100, 255, 100))  
            task.wait(0.5)  

            if MultiPressPrompt() then  
                StatusLabel.Text = "⏳ รอ 3 วิ..."  
                addLog("รอ 3 วิ...", Color3.fromRGB(255, 200, 0))  
                task.wait(3)  

                local newPart = Instance.new("Part")  
                newPart.Size = Vector3.new(6, 1, 6)  
                newPart.Position = Vector3.new(0, -100, 0)  
                newPart.Anchored = true  
                newPart.BrickColor = BrickColor.new("Bright green")  
                newPart.Name = "UndergroundPoint"  
                newPart.Parent = workspace  

                HumanoidRootPart.CFrame = CFrame.new(newPart.Position + Vector3.new(0, 3, 0))  
                StatusLabel.Text = "⬇️ ลงใต้ดิน"  
                addLog("รอ 80 วิ...", Color3.fromRGB(255, 255, 0))  

                task.wait(80 + math.random(-5, 5))  -- เปลี่ยนจาก 120 เป็น 80
            end  
        end
    end
end)()


local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- ตรวจสอบ PlaceId ปัจจุบัน
local placeId = game.PlaceId
print("📌 PlaceId ของแมพนี้:", placeId)

-- กำหนดโค้ดสำหรับแต่ละแมพ
local MapConfig = {
    [116495829188952] = function()
        print("✅ โหลดระบบสำหรับ Dead Rails Alpha")
        -- ใส่โค้ดของแมพ Dead Rails Alpha ตรงนี้
    end,

    [18668065416] = function()
        print("✅ โหลดระบบสำหรับ Blue Lock Rivals")
        -- ใส่โค้ดของแมพ Blue Lock Rivals ตรงนี้
    end
}

-- ถ้าแมพมีการตั้งค่า ให้รันโค้ดเฉพาะของมัน
if MapConfig[placeId] then
    print("🚀 กำลังโหลดโค้ดของแมพ:", placeId)
    MapConfig[placeId]() -- รันโค้ดของแมพนั้น ๆ
else
    print("⏹️ ไม่มีการตั้งค่าสำหรับแมพนี้ สคริปต์จะไม่โหลดอะไรเลย")
    return -- จบการทำงานของสคริปต์ตรงนี้
end



local Window = Rayfield:CreateWindow({
   Name = "FOR GOT     (Dead rails)",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "FOR GOT",
   LoadingSubtitle = "by FOR GOT",
   Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local Tab = Window:CreateTab("หน้าหลัก", 4483362458) -- Title, Image

local Divider = Tab:CreateDivider()

local running = false  -- ตัวแปรเก็บสถานะ Toggle

local Toggle = Tab:CreateToggle({
   Name = "ดรอปของ ออโต้",
   CurrentValue = false,
   Flag = "Toggle1",
   Callback = function(Value)
       running = Value  -- อัปเดตสถานะ Toggle

       if running then
           task.spawn(function()
               while running do
                   game.ReplicatedStorage.Remotes.DropItem:FireServer()
                   wait(0.01)
               end
           end)
       end
   end,
})
local Button = Tab:CreateButton({
   Name = "เก็บของออโต้",
   Interact = 'Click',
   Callback = function()
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local toggleEnabled = true  -- ตัวแปรเพื่อบันทึกสถานะของการเปิด/ปิด

-- สร้าง GUI สำหรับปุ่ม toggle
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 150, 0, 40)  -- ลดขนาดลง
toggleButton.Position = UDim2.new(0, 20, 0.5, -20)  -- ย้ายไปด้านซ้ายของจอ
toggleButton.Text = "เก็บของออโต้"
toggleButton.Parent = screenGui
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)  -- สีพื้นหลัง
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)  -- สีตัวอักษร
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 18
toggleButton.BorderSizePixel = 2
toggleButton.Draggable = true  -- ทำให้สามารถลากได้
toggleButton.Active = true
toggleButton.Selectable = true

-- ฟังก์ชันที่ใช้ในการ toggle การทำงาน
local function toggleItems()
    toggleEnabled = not toggleEnabled
    if toggleEnabled then
        toggleButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)  -- สีฟ้า (เปิด)
        toggleButton.Text = "เก็บของออโต้"
    else
        toggleButton.BackgroundColor3 = Color3.fromRGB(170, 0, 0)  -- สีแดง (ปิด)
        toggleButton.Text = "ปิดออโต้"
    end
end

-- เพิ่มฟังก์ชันให้ปุ่ม
toggleButton.MouseButton1Click:Connect(toggleItems)

while true do
    local args = {}

    -- ถ้า toggle เปิดอยู่ จะทำงานตามปกติ
    if toggleEnabled then
        -- เก็บทุกไอเทมใน workspace.RuntimeItems
        for _, item in pairs(workspace.RuntimeItems:GetChildren()) do
            -- ตรวจสอบว่าโมเดลมี PrimaryPart
            if item.PrimaryPart then
                -- คำนวณระยะห่างระหว่างผู้เล่นและโมเดล
                local distance = (item.PrimaryPart.Position - character.HumanoidRootPart.Position).Magnitude

                -- ถ้าระยะห่างมากกว่า 10 เมตร เปลี่ยนสีเป็นแดงนีออน
                if distance > 10 then
                    -- เปลี่ยนสีของโมเดลทั้งหมดที่มี PrimaryPart
                    for _, part in pairs(item:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.BrickColor = BrickColor.new("Bright red")  -- สีแดงนีออน
                            part.Material = Enum.Material.Neon  -- ทำให้เป็นเนออน
                        end
                    end
                elseif distance <= 30 then
                    -- ถ้าผู้เล่นเข้าใกล้ไอเทมให้สีกลับเป็นปกติ และเก็บไอเทมทันที
                    for _, part in pairs(item:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.BrickColor = BrickColor.new("Bright blue")  -- สีปกติ
                            part.Material = Enum.Material.SmoothPlastic  -- เปลี่ยนเป็นวัสดุปกติ
                        end
                    end
                    -- ส่งคำสั่งเก็บไอเทมทันที
                    game:GetService("ReplicatedStorage").Remotes.StoreItem:FireServer(item)
                end

                table.insert(args, item)
            end
        end
    end

    -- รอเวลา 0.01 วินาทีก่อนวนลูปใหม่
    wait(0.01)
end
   -- The function that takes place when the button is pressed
   end,
})


local Button = Tab:CreateButton({
   Name = "กลางวัน",
   Interact = 'Click',
   Callback = function()

local Lighting = game:GetService("Lighting")

-- ตั้งเวลาให้เป็นเที่ยงวันและป้องกันการเปลี่ยนแปลง
Lighting.ClockTime = 12
Lighting.TimeOfDay = "12:00:00"
Lighting.Brightness = 2 -- ปรับความสว่างให้เหมาะสม
Lighting.GlobalShadows = true -- ทำให้แสงเงาดูสมจริง
Lighting.Changed:Connect(function()
    Lighting.ClockTime = 12 -- ล็อคเวลาไว้ที่เที่ยงวัน
end)

   -- The function that takes place when the button is pressed
   end,
})

local Button = Tab:CreateButton({
   Name = "ลบหมอก",
   Interact = 'Click',
   Callback = function()

-- ลบหมอกและความมืดในเกม
local lighting = game:GetService("Lighting")

-- ลบหมอก
lighting.FogEnd = 0

-- ปรับความสว่าง
lighting.Brightness = 2  -- คุณสามารถปรับค่านี้ตามต้องการ

-- ปรับแสงสว่างของท้องฟ้า
lighting.Ambient = Color3.fromRGB(255, 255, 255)  -- ปรับแสงให้สว่างขึ้น

-- ปรับแสงของแหล่งแสงในเกม
lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)  -- แสงภายนอกให้สว่าง

   -- The function that takes place when the button is pressed
   end,
})




local Tab = Window:CreateTab("เพิ่มเติม", 4483362458) -- Title, Image

local Divider = Tab:CreateDivider()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local noclipEnabled = false -- สถานะของ Noclip
local minY = workspace.FallenPartsDestroyHeight + 10 -- ป้องกันตกแมพ

-- ฟังก์ชันเปิด/ปิด Noclip
local function toggleNoclip(Value)
    noclipEnabled = Value
end

-- สร้าง Toggle
local Toggle = Tab:CreateToggle({
   Name = "Noclip",
   CurrentValue = false,
   Flag = "NoclipToggle",
   Callback = function(Value)
       toggleNoclip(Value)
   end,
})

-- วนลูปตรวจสอบ Noclip
RunService.Stepped:Connect(function()
    if noclipEnabled then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false -- ทำให้ทะลุสิ่งปลูกสร้าง
            end
        end
    else
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true -- ปิด Noclip
            end
        end
    end

    -- ป้องกันตกแมพ
    if humanoidRootPart.Position.Y < minY then
        humanoidRootPart.CFrame = humanoidRootPart.CFrame + Vector3.new(0, 10, 0)
    end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local ESPEnabled = false -- ค่าปกติของ ESP (ปิดอยู่)

-- ฟังก์ชันสร้าง ESP
local function createESP(player)
    if player == LocalPlayer then return end -- ข้ามตัวเอง

    local character = player.Character
    if not character then return end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoidRootPart or not humanoid then return end

    -- สร้าง BillboardGui
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP"
    billboard.Size = UDim2.new(4, 0, 1, 0)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.Adornee = humanoidRootPart
    billboard.AlwaysOnTop = true

    -- สร้าง TextLabel สำหรับชื่อ, เลือด, ระยะ
    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(6, 0, 6, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextScaled = true
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextColor3 = Color3.fromRGB(0, 255, 0) -- สีเขียว
    textLabel.Parent = billboard

    -- สร้าง Box ESP (คลุมตัว)
    local highlight = Instance.new("Highlight")
    highlight.FillColor = Color3.fromRGB(0, 255, 0) -- สีเขียว
    highlight.OutlineColor = Color3.fromRGB(0, 255, 0)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    highlight.Parent = character

    -- อัปเดตข้อมูล Text ESP แบบเรียลไทม์
    local function updateESP()
        while billboard.Parent and ESPEnabled do
            if character.Parent and humanoidRootPart.Parent then
                local distance = (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and (LocalPlayer.Character.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude) or 0
                textLabel.Text = string.format("[%s] HP: %d | %.1f m", player.Name, humanoid.Health, distance)
            else
                billboard:Destroy()
                highlight:Destroy()
                break
            end
            task.wait(0.1) -- อัปเดตทุก 0.1 วินาที
        end
    end

    billboard.Parent = character
    task.spawn(updateESP)
end

-- โหลด ESP สำหรับผู้เล่นที่มีอยู่แล้ว
local function toggleESP(state)
    ESPEnabled = state
    if ESPEnabled then
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                createESP(player)
            end
        end
    else
        for _, player in ipairs(Players:GetPlayers()) do
            if player.Character then
                if player.Character:FindFirstChild("ESP") then
                    player.Character.ESP:Destroy()
                end
                if player.Character:FindFirstChildOfClass("Highlight") then
                    player.Character:FindFirstChildOfClass("Highlight"):Destroy()
                end
            end
        end
    end
end

-- โหลด ESP เมื่อมีผู้เล่นใหม่เข้ามา
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        task.wait(1) -- รอให้ Character โหลดเสร็จ
        if ESPEnabled then
            createESP(player)
        end
    end)
end)

-- ⚡ เชื่อมกับ Toggle GUI
local Toggle = Tab:CreateToggle({
    Name = "ESP ",
    CurrentValue = false,
    Flag = "ESP_Toggle",
    Callback = function(Value)
        toggleESP(Value)
    end
})


local Players = game:GetService("Players")
local player = Players.LocalPlayer
local runService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")
local camera = workspace.CurrentCamera

local npcLock = false
local lastTarget = nil
local toggleLoop

-- ฟังก์ชันเพิ่ม Highlight ให้ Player
local function addPlayerHighlight()
    if player.Character then
        local highlight = player.Character:FindFirstChild("PlayerHighlightESP")
        if not highlight then
            highlight = Instance.new("Highlight")
            highlight.Name = "PlayerHighlightESP"
            highlight.FillColor = Color3.new(1, 1, 1)
            highlight.OutlineColor = Color3.new(1, 1, 1)
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Parent = player.Character
        end
    end
end

-- ฟังก์ชันลบ Highlight ออกจาก Player
local function removePlayerHighlight()
    if player.Character and player.Character:FindFirstChild("PlayerHighlightESP") then
        player.Character.PlayerHighlightESP:Destroy()
    end
end

-- ฟังก์ชันค้นหา NPC ที่ใกล้ที่สุด
local function getClosestNPC()
    local closestNPC = nil
    local closestDistance = math.huge

    for _, object in ipairs(workspace:GetDescendants()) do
        if object:IsA("Model") then
            local humanoid = object:FindFirstChild("Humanoid") or object:FindFirstChildWhichIsA("Humanoid")
            local hrp = object:FindFirstChild("HumanoidRootPart") or object.PrimaryPart
            if humanoid and hrp and humanoid.Health > 0 and object.Name ~= "Horse" then
                local isPlayer = false
                for _, pl in ipairs(Players:GetPlayers()) do
                    if pl.Character == object then
                        isPlayer = true
                        break
                    end
                end
                if not isPlayer then
                    local distance = (hrp.Position - player.Character.HumanoidRootPart.Position).Magnitude
                    if distance < closestDistance then
                        closestDistance = distance
                        closestNPC = object
                    end
                end
            end
        end
    end

    return closestNPC
end

-- Toggle UI
local Toggle = Tab:CreateToggle({
   Name = "Aim bot",
   CurrentValue = false,
   Flag = "NPC_Lock_Toggle",
   Callback = function(Value)
       npcLock = Value

       if npcLock then
           -- เริ่มระบบล็อก NPC
           toggleLoop = runService.RenderStepped:Connect(function()
               local npc = getClosestNPC()
               if npc and npc:FindFirstChild("Humanoid") then
                   local npcHumanoid = npc:FindFirstChild("Humanoid")
                   if npcHumanoid.Health > 0 then
                       camera.CameraSubject = npcHumanoid
                       lastTarget = npc
                       addPlayerHighlight()
                   else
                       StarterGui:SetCore("SendNotification", {
                           Title = "Killed NPC",
                           Text = npc.Name,
                           Duration = 0.4
                       })
                       lastTarget = nil
                       removePlayerHighlight()
                       if player.Character and player.Character:FindFirstChild("Humanoid") then
                           camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
                       end
                   end
               else
                   if player.Character and player.Character:FindFirstChild("Humanoid") then
                       camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
                   end
                   lastTarget = nil
                   removePlayerHighlight()
               end
           end)
       else
           -- ปิดระบบล็อก NPC
           if toggleLoop then toggleLoop:Disconnect() end
           removePlayerHighlight()
           if player.Character and player.Character:FindFirstChild("Humanoid") then
               camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
           end
       end
   end,
})

local Tab = Window:CreateTab("Drop Down", 4483362458) -- Title, Image

local Divider = Tab:CreateDivider()


local Players = game:GetService("Players")
local Camera = game.Workspace.CurrentCamera
local player = Players.LocalPlayer
local targetPlayer = nil -- เก็บเป้าหมายที่ต้องการให้กล้องติดตาม

-- UI Library Dropdown (แก้ให้ตรงกับ UI Library ที่มึงใช้)
local Dropdown = Tab:CreateDropdown({
    Name = "เลือกผู้เล่น",
    Options = {"None"}, -- เริ่มต้นมีแค่ "None"
    CurrentOption = {"None"},
    MultipleOptions = false,
    Flag = "Dropdown1",
    Callback = function(Options)
        local selectedPlayerName = Options[1]
        if selectedPlayerName == "None" then
            -- ✅ กลับไปเป็นมุมมองปกติของตัวเอง
            Camera.CameraType = Enum.CameraType.Custom
            Camera.CameraSubject = player.Character and player.Character:FindFirstChild("Humanoid")
            targetPlayer = nil
        else
            -- ✅ ตั้งกล้องให้ติดตามผู้เล่นที่เลือก
            targetPlayer = Players:FindFirstChild(selectedPlayerName)
            if targetPlayer and targetPlayer.Character then
                local targetHumanoid = targetPlayer.Character:FindFirstChild("Humanoid")
                if targetHumanoid then
                    Camera.CameraType = Enum.CameraType.Custom
                    Camera.CameraSubject = targetHumanoid -- ✅ ให้กล้องติดตามตัวละครนี้
                end
            end
        end
    end
})

-- ฟังก์ชันอัปเดตรายชื่อผู้เล่นใน Dropdown
local function updatePlayerList()
    local playerNames = {"None"} -- ใส่ "None" ไว้ในรายการเสมอ
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player then -- ไม่ใส่ตัวเอง
            table.insert(playerNames, plr.Name)
        end
    end
    Dropdown:Refresh(playerNames, true) -- รีเฟรช Dropdown
end

-- อัปเดตรายชื่อผู้เล่นเมื่อมีคนเข้า-ออก
Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)

-- โหลดรายชื่อครั้งแรก
updatePlayerList()

























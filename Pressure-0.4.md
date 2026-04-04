local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Pressure 0.4",
    Icon = 0,
    LoadingTitle = "Pressure 0.4",
    LoadingSubtitle = "by Normalplayer",
    Theme = {
        TextColor = Color3.fromRGB(200, 225, 255),
        Background = Color3.fromRGB(10, 18, 35),
        Topbar = Color3.fromRGB(15, 25, 50),
        Shadow = Color3.fromRGB(5, 10, 20),
        NotificationBackground = Color3.fromRGB(12, 22, 45),
        NotificationActionsBackground = Color3.fromRGB(30, 60, 120),
        TabBackground = Color3.fromRGB(20, 35, 70),
        TabStroke = Color3.fromRGB(30, 55, 110),
        TabBackgroundSelected = Color3.fromRGB(40, 90, 180),
        TabTextColor = Color3.fromRGB(150, 190, 255),
        SelectedTabTextColor = Color3.fromRGB(220, 235, 255),
        ElementBackground = Color3.fromRGB(15, 28, 58),
        ElementBackgroundHover = Color3.fromRGB(20, 38, 78),
        SecondaryElementBackground = Color3.fromRGB(10, 18, 40),
        ElementStroke = Color3.fromRGB(35, 65, 130),
        SecondaryElementStroke = Color3.fromRGB(25, 48, 100),
        SliderBackground = Color3.fromRGB(30, 80, 170),
        SliderProgress = Color3.fromRGB(50, 120, 220),
        SliderStroke = Color3.fromRGB(70, 150, 255),
        ToggleBackground = Color3.fromRGB(15, 28, 58),
        ToggleEnabled = Color3.fromRGB(40, 110, 220),
        ToggleDisabled = Color3.fromRGB(40, 55, 90),
        ToggleEnabledStroke = Color3.fromRGB(60, 140, 255),
        ToggleDisabledStroke = Color3.fromRGB(50, 70, 120),
        ToggleEnabledOuterStroke = Color3.fromRGB(35, 90, 180),
        ToggleDisabledOuterStroke = Color3.fromRGB(25, 40, 80),
        DropdownSelected = Color3.fromRGB(20, 40, 85),
        DropdownUnselected = Color3.fromRGB(15, 28, 58),
        InputBackground = Color3.fromRGB(15, 28, 58),
        InputStroke = Color3.fromRGB(40, 75, 150),
        PlaceholderColor = Color3.fromRGB(100, 140, 200),
    },
    ToggleUIKeybind = "K",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = { Enabled = true, FolderName = "Pressure", FileName = "Pressure" },
    KeySystem = false
})

task.wait(2)
Rayfield:Notify({
    Title = "Pressure 0.4",
    Content = "Please report any bugs in the comments of ScriptBlox, or leave a suggestion!",
    Duration = 8,
    Image = "message-circle",
})

local Updates = Window:CreateTab("Updates", "file-text")
Updates:CreateParagraph({
    Title = "Version 0.4 mini update",
    Content = "- Monster Dodge now lifts 80 studs above (monsters can no longer reach)\n- Monster Dodge timeout increased to 40 seconds\n- Fixed Monster Dodge not stopping when toggle is disabled\n- Added NeoStyk ESP\n- Added Anti WallDweller"
})
Updates:CreateDivider()
Updates:CreateParagraph({
      Title = "Version 0.3 Fix update",
    Content = "- Fixed Monster Dodge (now lifts 50 studs above)\n- Fixed Generator ESP\n- Fixed toggles not properly disabling\n- Added Anti-Statue\n- Added Generator ESP\n- Improved performance\n- Added Changelog\n- Fixed Pandemonium notification spam\n- Fixed Anti toggles not working in every room"
})

local Esp    = Window:CreateTab("Visuals", "eye")
local Auto   = Window:CreateTab("Automations", "zap")
local Anti   = Window:CreateTab("Antis", "shield")

local WHITE      = Color3.fromRGB(255, 255, 255)
local RED        = Color3.fromRGB(255, 0, 0)
local Rooms      = workspace.GameplayFolder.Rooms
local Player     = game:GetService("Players").LocalPlayer
local RunService = game:GetService("RunService")

-- ================================================
-- CONFIGS
-- ================================================

local KEYCARDS = {
    NormalKeyCard = { label = "Keycard",       fill = Color3.fromRGB(255, 50,  50),  outline = Color3.fromRGB(255, 150, 150) },
    InnerKeyCard  = { label = "Inner Keycard", fill = Color3.fromRGB(0,   120, 255), outline = Color3.fromRGB(100, 180, 255) },
    RidgeKeyCard  = { label = "Ridge Keycard", fill = Color3.fromRGB(255, 150, 0),   outline = Color3.fromRGB(255, 200, 100) },
    PasswordPaper = { label = "Password",      fill = Color3.fromRGB(0,   200, 80),  outline = Color3.fromRGB(100, 255, 150) },
}

local ITEM_PATTERNS = {
    { pattern = "Lantern",      label = "Lantern",       fill = Color3.fromRGB(255, 200, 0),   outline = Color3.fromRGB(255, 230, 100) },
    { pattern = "Flashlight",   label = "Flashlight",    fill = Color3.fromRGB(200, 200, 255), outline = Color3.fromRGB(255, 255, 255) },
    { pattern = "Blacklight",   label = "Blacklight",    fill = Color3.fromRGB(150, 0,   255), outline = Color3.fromRGB(200, 100, 255) },
    { pattern = "Medkit",       label = "Medkit",        fill = Color3.fromRGB(255, 50,  50),  outline = Color3.fromRGB(255, 150, 150) },
    { pattern = "HealthBoost",  label = "Health Boost",  fill = Color3.fromRGB(255, 100, 100), outline = Color3.fromRGB(255, 180, 180) },
    { pattern = "Defib",        label = "Defib",         fill = Color3.fromRGB(255, 0,   100), outline = Color3.fromRGB(255, 100, 180) },
    { pattern = "FlashBeacon",  label = "Flash Beacon",  fill = Color3.fromRGB(255, 255, 50),  outline = Color3.fromRGB(255, 255, 150) },
    { pattern = "Scanner",      label = "Scanner",       fill = Color3.fromRGB(0,   255, 200), outline = Color3.fromRGB(100, 255, 230) },
    { pattern = "^Book$",       label = "Book",          fill = Color3.fromRGB(180, 120, 40),  outline = Color3.fromRGB(220, 170, 100) },
    { pattern = "CodeBreacher", label = "Code Breacher", fill = Color3.fromRGB(0,   200, 255), outline = Color3.fromRGB(100, 230, 255) },
    { pattern = "Gummylight",   label = "Gummylight",    fill = Color3.fromRGB(255, 100, 200), outline = Color3.fromRGB(255, 180, 230) },
    { pattern = "WindupLight",  label = "Windup Light",  fill = Color3.fromRGB(255, 180, 50),  outline = Color3.fromRGB(255, 210, 120) },
    { pattern = "SPRINT",       label = "SPRINT",        fill = Color3.fromRGB(50,  255, 100), outline = Color3.fromRGB(150, 255, 180) },
    { pattern = "ToyRemote",    label = "Toy Remote",    fill = Color3.fromRGB(100, 100, 255), outline = Color3.fromRGB(180, 180, 255) },
    { pattern = "Battery",      label = "Battery",       fill = Color3.fromRGB(255, 230, 0),   outline = Color3.fromRGB(255, 245, 100) },
    { pattern = "Neostyk",      label = "NeoStyk",       fill = Color3.fromRGB(0,   255, 150), outline = Color3.fromRGB(100, 255, 200) },
    { pattern = "NeoStyk",      label = "NeoStyk",       fill = Color3.fromRGB(0,   255, 150), outline = Color3.fromRGB(100, 255, 200) },
}

local MONSTERS = {
    A60 = true, AbstractArt = true, Angler = true,
    Bottomfeeder = true, Bouncers = true, Candlebearers = true, Candlebrutes = true,
    Eyefestation = true, GuardianAngel = true, Harbinger = true,
    ImaginaryFriend = true, Lopee = true, Pandemonium = true,
    Parasite = true, Pipsqueak = true, Rebarb = true, Redeemer = true,
    Skelepede = true, Stan = true, TheDiVine = true, TheEducator = true,
    TheMindscape = true, ThePainter = true, TheSaboteur = true,
    Trenchbleeder = true, WallDwellers = true, WitchingHour = true,
    Blitz = true, Squiddles = true, NaviAI = true, Void = true,
    RottenCoral = true, Searchlights = true, DefenseSystem = true,
    Froger = true, Chainsmoker = true, Pinkie = true,
    -- Wall Dweller variants
    WallDweller = true, MeatWallDweller = true, RottenWallDweller = true,
}

local CURRENCY_PATTERNS = {
    { pattern = "^UCurrency5%-",   label = "~5$",      fill = Color3.fromRGB(100, 220, 255), outline = Color3.fromRGB(180, 240, 255) },
    { pattern = "^UCurrency10%-",  label = "~10$",     fill = Color3.fromRGB(50,  180, 255), outline = Color3.fromRGB(150, 220, 255) },
    { pattern = "^UCurrency15%-",  label = "~15$",     fill = Color3.fromRGB(0,   150, 255), outline = Color3.fromRGB(100, 200, 255) },
    { pattern = "^UCurrency25%-",  label = "~25$",     fill = Color3.fromRGB(0,   100, 220), outline = Color3.fromRGB(80,  170, 255) },
    { pattern = "^UCurrency50%-",  label = "~50$",     fill = Color3.fromRGB(0,   80,  200), outline = Color3.fromRGB(60,  150, 240) },
    { pattern = "^UCurrency100%-", label = "~100$",    fill = Color3.fromRGB(0,   50,  180), outline = Color3.fromRGB(50,  120, 220) },
    { pattern = "^UCurrency200%-", label = "~200$",    fill = Color3.fromRGB(0,   30,  150), outline = Color3.fromRGB(30,  100, 200) },
    { pattern = "^Currency5%-",    label = "5$",       fill = Color3.fromRGB(180, 255, 100), outline = Color3.fromRGB(220, 255, 170) },
    { pattern = "^Currency10%-",   label = "10$",      fill = Color3.fromRGB(100, 220, 50),  outline = Color3.fromRGB(170, 240, 120) },
    { pattern = "^Currency15%-",   label = "15$",      fill = Color3.fromRGB(50,  200, 50),  outline = Color3.fromRGB(130, 230, 130) },
    { pattern = "^Currency25%-",   label = "25$",      fill = Color3.fromRGB(0,   180, 80),  outline = Color3.fromRGB(80,  220, 150) },
    { pattern = "^Currency50%-",   label = "50$",      fill = Color3.fromRGB(0,   150, 255), outline = Color3.fromRGB(80,  200, 255) },
    { pattern = "^Currency100%-",  label = "100$",     fill = Color3.fromRGB(255, 200, 0),   outline = Color3.fromRGB(255, 230, 100) },
    { pattern = "^Currency200%-",  label = "200$",     fill = Color3.fromRGB(255, 100, 0),   outline = Color3.fromRGB(255, 180, 80)  },
    { pattern = "^RareCurrency",   label = "RARE$",    fill = Color3.fromRGB(200, 0,   255), outline = Color3.fromRGB(255, 100, 255) },
    { pattern = "^Blueprint$",     label = "Blueprint",fill = Color3.fromRGB(0,   180, 255), outline = Color3.fromRGB(100, 220, 255) },
}

-- ================================================
-- HELPERS
-- ================================================

local function GetItemConfig(name)
    for _, e in ipairs(ITEM_PATTERNS) do
        if string.match(name, e.pattern) then
            return { label = e.label, fill = e.fill, outline = e.outline }
        end
    end
    return nil
end

local function GetCurrencyConfig(name)
    for _, e in ipairs(CURRENCY_PATTERNS) do
        if string.match(name, e.pattern) then
            return { label = e.label, fill = e.fill, outline = e.outline or WHITE }
        end
    end
    return nil
end

local function GetRootPart()
    local char = Player.Character
    return char and char:FindFirstChild("HumanoidRootPart")
end

local function GetHumanoid()
    local char = Player.Character
    return char and char:FindFirstChildOfClass("Humanoid")
end

local lastNotify = {}
local function NotifyOnce(key, title, content, duration, image)
    if lastNotify[key] and (tick() - lastNotify[key]) < 8 then return end
    lastNotify[key] = tick()
    Rayfield:Notify({ Title = title, Content = content, Duration = duration, Image = image })
end

-- ================================================
-- ESP FUNCTIONS
-- ================================================

local function AddESP(part, config)
    if not part or not part.Parent then return end
    if part:FindFirstChildOfClass("Highlight") then return end

    local h = Instance.new("Highlight")
    h.Adornee = part
    h.FillColor = config.fill
    h.OutlineColor = config.outline or WHITE
    h.FillTransparency = config.fillTransparency or 0.4
    h.OutlineTransparency = 0
    h.Parent = part

    local bb = Instance.new("BillboardGui")
    bb.Name = "ESP_Label"
    bb.Adornee = part
    bb.AlwaysOnTop = true
    bb.Size = UDim2.new(0, 140, 0, 40)
    bb.StudsOffset = Vector3.new(0, 3, 0)
    bb.Parent = part

    local lbl = Instance.new("TextLabel")
    lbl.Text = config.label
    lbl.Size = UDim2.new(1, 0, 1, 0)
    lbl.BackgroundTransparency = 1
    lbl.TextColor3 = config.textColor or WHITE
    lbl.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    lbl.TextStrokeTransparency = 0.3
    lbl.TextScaled = true
    lbl.Font = Enum.Font.GothamBold
    lbl.Parent = bb

    local stroke = Instance.new("UIStroke")
    stroke.Color = config.outline or WHITE
    stroke.Thickness = 1.5
    stroke.Parent = lbl
end

local function RemoveESP(part)
    if not part then return end
    local h = part:FindFirstChildOfClass("Highlight")
    if h then h:Destroy() end
    local b = part:FindFirstChild("ESP_Label")
    if b then b:Destroy() end
end

local function ScanRooms(enabled, matchFn, onFound)
    local connections = {}
    local scanned = {}

    local function processDesc(d)
        if scanned[d] then return end
        scanned[d] = true
        if not d.Parent then return end
        local config = matchFn(d.Name)
        if config then
            local target = d:FindFirstChild("ProxyPart") or d
            onFound(target, config)
        end
    end

    local function scanRoom(room)
        for _, d in ipairs(room:GetDescendants()) do
            if not enabled() then break end
            processDesc(d)
        end
    end

    for _, room in ipairs(Rooms:GetChildren()) do
        if not enabled() then break end
        task.defer(function() scanRoom(room) end)
        table.insert(connections, room.DescendantAdded:Connect(function(d)
            task.wait(0.1)
            if enabled() then processDesc(d) end
        end))
    end

    table.insert(connections, Rooms.ChildAdded:Connect(function(room)
        task.wait(0.3)
        if not enabled() then return end
        scanRoom(room)
        table.insert(connections, room.DescendantAdded:Connect(function(d)
            task.wait(0.1)
            if enabled() then processDesc(d) end
        end))
    end))

    return connections
end

local function CreateESPToggle(tab, name, flag, matchFn)
    local connections = {}
    local active = false
    local targets = {}

    tab:CreateToggle({
        Name = name, CurrentValue = false, Flag = flag,
        Callback = function(Value)
            active = Value
            if Value then
                targets = {}
                connections = ScanRooms(
                    function() return active end,
                    matchFn,
                    function(target, config)
                        AddESP(target, config)
                        table.insert(targets, target)
                    end
                )
            else
                for _, c in ipairs(connections) do c:Disconnect() end
                connections = {}
                for _, target in ipairs(targets) do
                    pcall(RemoveESP, target)
                end
                targets = {}
            end
        end
    })
end

local function CreateAntiToggle(tab, name, flag, matchFn)
    local conns = {}
    local active = false

    tab:CreateToggle({
        Name = name, CurrentValue = false, Flag = flag,
        Callback = function(Value)
            active = Value
            if Value then
                local function destroyIfMatch(d)
                    if not active then return end
                    if matchFn(d.Name) then
                        pcall(function() d:Destroy() end)
                    end
                end
                local function setupRoom(room)
                    task.defer(function()
                        if not active then return end
                        for _, d in ipairs(room:GetDescendants()) do
                            destroyIfMatch(d)
                        end
                    end)
                    table.insert(conns, room.DescendantAdded:Connect(function(d)
                        task.wait(0.05)
                        if active then destroyIfMatch(d) end
                    end))
                end
                for _, room in ipairs(Rooms:GetChildren()) do
                    setupRoom(room)
                end
                table.insert(conns, Rooms.ChildAdded:Connect(function(room)
                    task.wait(0.3)
                    if active then setupRoom(room) end
                end))
            else
                for _, c in ipairs(conns) do c:Disconnect() end
                conns = {}
            end
        end
    })
end

-- ================================================
-- VISUALS TAB
-- ================================================

Esp:CreateSection("Items")

CreateESPToggle(Esp, "Keycard ESP",              "EspKeycard",  function(n) return KEYCARDS[n] end)
CreateESPToggle(Esp, "Currency & Blueprint ESP", "EspCurrency", function(n) return GetCurrencyConfig(n) end)
CreateESPToggle(Esp, "Item ESP",                 "EspItems",    function(n) return GetItemConfig(n) end)

Esp:CreateSection("Monsters")

-- Monster Locker ESP
local lockerConns, lockerActive = {}, false
Esp:CreateToggle({
    Name = "Monster Locker ESP", CurrentValue = false, Flag = "EspMonsterLocker",
    Callback = function(Value)
        lockerActive = Value
        local cfg = { label = "⚠ MONSTER LOCKER", fill = Color3.fromRGB(200, 0, 0), outline = Color3.fromRGB(255, 80, 80), textColor = Color3.fromRGB(255, 80, 80), fillTransparency = 0.3 }
        if Value then
            lockerConns = ScanRooms(
                function() return lockerActive end,
                function(n) return n == "MonsterLocker" and cfg or nil end,
                function(target, config) AddESP(target, config) end
            )
        else
            for _, c in ipairs(lockerConns) do c:Disconnect() end
            lockerConns = {}
            for _, room in ipairs(Rooms:GetChildren()) do
                for _, d in ipairs(room:GetDescendants()) do
                    if d.Name == "MonsterLocker" then pcall(RemoveESP, d) end
                end
            end
        end
    end
})

-- Fake Door ESP
local fakeDoorConns, fakeDoorActive = {}, false
Esp:CreateToggle({
    Name = "Fake Door ESP", CurrentValue = false, Flag = "EspFakeDoor",
    Callback = function(Value)
        fakeDoorActive = Value
        local cfg = { label = "✖ FAKE DOOR", fill = Color3.fromRGB(180, 0, 0), outline = RED, textColor = RED, fillTransparency = 0.3 }
        if Value then
            fakeDoorConns = ScanRooms(
                function() return fakeDoorActive end,
                function(n) return n == "Door" and cfg or nil end,
                function(target, config)
                    if target.Parent and target.Parent.Name == "TricksterDoor" then
                        AddESP(target, config)
                    end
                end
            )
        else
            for _, c in ipairs(fakeDoorConns) do c:Disconnect() end
            fakeDoorConns = {}
            for _, room in ipairs(Rooms:GetChildren()) do
                for _, d in ipairs(room:GetDescendants()) do
                    if d.Name == "Door" and d.Parent and d.Parent.Name == "TricksterDoor" then
                        pcall(RemoveESP, d)
                    end
                end
            end
        end
    end
})

-- Generator ESP
local generatorConns, generatorActive = {}, false
Esp:CreateToggle({
    Name = "Generator ESP", CurrentValue = false, Flag = "EspGenerator",
    Callback = function(Value)
        generatorActive = Value
        local trackedGenerators = {}

        local function getGeneratorCfg(fixedVal)
            local pct = tonumber(fixedVal) or 0
            if pct >= 100 then return nil end
            local r = math.floor(255 * (1 - pct / 100))
            local g = math.floor(255 * (pct / 100))
            return {
                label = "⚙ Generator " .. math.floor(pct) .. "%",
                fill = Color3.fromRGB(r, g, 0),
                outline = Color3.fromRGB(255, 200, 0),
                textColor = WHITE,
                fillTransparency = 0.3,
            }
        end

        local function setupGenerator(gen)
            if not generatorActive then return end
            if trackedGenerators[gen] then return end
            trackedGenerators[gen] = true
            local fixed = gen:FindFirstChild("Fixed")
            if not fixed then return end
            local proxy = gen:FindFirstChild("ProxyPart") or gen
            local cfg = getGeneratorCfg(fixed.Value)
            if cfg then pcall(AddESP, proxy, cfg) end
            table.insert(generatorConns, fixed:GetPropertyChangedSignal("Value"):Connect(function()
                if not generatorActive then return end
                pcall(RemoveESP, proxy)
                local newCfg = getGeneratorCfg(fixed.Value)
                if newCfg then pcall(AddESP, proxy, newCfg) end
            end))
        end

        local function scanRoom(room)
            for _, d in ipairs(room:GetDescendants()) do
                if not generatorActive then break end
                if d.Name == "PresetGenerator" or d.Name == "Generator" then
                    pcall(setupGenerator, d)
                end
                if d.Name == "Fixed" and d.Parent then
                    pcall(setupGenerator, d.Parent)
                end
            end
        end

        if Value then
            for _, room in ipairs(Rooms:GetChildren()) do
                task.defer(function() scanRoom(room) end)
                table.insert(generatorConns, room.DescendantAdded:Connect(function(d)
                    task.wait(0.2)
                    if not generatorActive then return end
                    if d.Name == "PresetGenerator" or d.Name == "Generator" or d.Name == "Fixed" then
                        pcall(setupGenerator, d.Name == "Fixed" and d.Parent or d)
                    end
                end))
            end
            table.insert(generatorConns, Rooms.ChildAdded:Connect(function(room)
                task.wait(0.3)
                if generatorActive then scanRoom(room) end
                table.insert(generatorConns, room.DescendantAdded:Connect(function(d)
                    task.wait(0.2)
                    if not generatorActive then return end
                    if d.Name == "PresetGenerator" or d.Name == "Generator" or d.Name == "Fixed" then
                        pcall(setupGenerator, d.Name == "Fixed" and d.Parent or d)
                    end
                end))
            end))
        else
            for _, c in ipairs(generatorConns) do c:Disconnect() end
            generatorConns = {}
            trackedGenerators = {}
            for _, room in ipairs(Rooms:GetChildren()) do
                for _, d in ipairs(room:GetDescendants()) do
                    if d.Name == "PresetGenerator" or d.Name == "Generator" then
                        local proxy = d:FindFirstChild("ProxyPart") or d
                        pcall(RemoveESP, proxy)
                    end
                end
            end
        end
    end
})

-- Monster ESP
local monsterEspConns, monsterEspActive = {}, false
Esp:CreateToggle({
    Name = "Monster ESP", CurrentValue = false, Flag = "EspMonster",
    Callback = function(Value)
        monsterEspActive = Value
        local cfg = { fill = Color3.fromRGB(200, 0, 0), outline = Color3.fromRGB(255, 200, 0), textColor = RED, fillTransparency = 0.3 }
        local tracked = {}

        local function applyESP(child)
            if not monsterEspActive then return end
            if tracked[child] then return end
            tracked[child] = true
            local c = {
                fill = cfg.fill, outline = cfg.outline,
                textColor = cfg.textColor, fillTransparency = cfg.fillTransparency,
                label = "☠ " .. child.Name
            }
            pcall(AddESP, child, c)
            table.insert(monsterEspConns, child.AncestryChanged:Connect(function()
                if not child:IsDescendantOf(game) then
                    pcall(RemoveESP, child)
                    tracked[child] = nil
                end
            end))
        end

        local function scanContainer(container)
            for _, child in ipairs(container:GetChildren()) do
                if MONSTERS[child.Name] then applyESP(child) end
            end
            table.insert(monsterEspConns, container.ChildAdded:Connect(function(child)
                if monsterEspActive and MONSTERS[child.Name] then applyESP(child) end
            end))
        end

        if Value then
            scanContainer(workspace)
            scanContainer(workspace.GameplayFolder.Monsters)
        else
            for _, c in ipairs(monsterEspConns) do c:Disconnect() end
            monsterEspConns = {}
            for _, child in ipairs(workspace:GetChildren()) do
                if MONSTERS[child.Name] then pcall(RemoveESP, child) end
            end
            for _, child in ipairs(workspace.GameplayFolder.Monsters:GetChildren()) do
                pcall(RemoveESP, child)
            end
        end
    end
})

-- Monster Alert
local monsterAlertConns, monsterAlertActive = {}, false
Esp:CreateToggle({
    Name = "Monster Alert", CurrentValue = false, Flag = "MonsterAlert",
    Callback = function(Value)
        monsterAlertActive = Value
        if Value then
            local function listenContainer(container)
                table.insert(monsterAlertConns, container.ChildAdded:Connect(function(child)
                    if monsterAlertActive and MONSTERS[child.Name] then
                        NotifyOnce(child.Name, "⚠️ Monster Detected!", child.Name .. " has spawned!", 5, "alert-triangle")
                    end
                end))
            end
            listenContainer(workspace)
            listenContainer(workspace.GameplayFolder.Monsters)
        else
            for _, c in ipairs(monsterAlertConns) do c:Disconnect() end
            monsterAlertConns = {}
        end
    end
})

Esp:CreateSection("World")

local originalLighting, fullbrightEffects = {}, {}
Esp:CreateToggle({
    Name = "Fullbright", CurrentValue = false, Flag = "Fullbright",
    Callback = function(Value)
        local L = game:GetService("Lighting")
        if Value then
            originalLighting = {
                Brightness = L.Brightness, ClockTime = L.ClockTime,
                FogEnd = L.FogEnd, GlobalShadows = L.GlobalShadows,
                Ambient = L.Ambient, OutdoorAmbient = L.OutdoorAmbient,
            }
            fullbrightEffects = {}
            for _, e in ipairs(L:GetChildren()) do
                if e:IsA("BlurEffect") or e:IsA("ColorCorrectionEffect")
                or e:IsA("DepthOfFieldEffect") or e:IsA("SunRaysEffect") then
                    fullbrightEffects[e] = e.Enabled
                    e.Enabled = false
                end
            end
            L.Brightness = 10
            L.ClockTime = 14
            L.FogEnd = 100000
            L.GlobalShadows = false
            L.Ambient = Color3.fromRGB(255, 255, 255)
            L.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
        else
            L.Brightness     = originalLighting.Brightness
            L.ClockTime      = originalLighting.ClockTime
            L.FogEnd         = originalLighting.FogEnd
            L.GlobalShadows  = originalLighting.GlobalShadows
            L.Ambient        = originalLighting.Ambient
            L.OutdoorAmbient = originalLighting.OutdoorAmbient
            for effect, wasEnabled in pairs(fullbrightEffects) do
                if effect and effect.Parent then effect.Enabled = wasEnabled end
            end
            fullbrightEffects = {}
        end
    end
})

-- ================================================
-- AUTOMATIONS
-- ================================================

Auto:CreateSection("Safety")

-- Monster Dodge — corrigido para parar quando toggle é desligado
local dodgeActive = false

Auto:CreateToggle({
    Name = "Monster Dodge", CurrentValue = false, Flag = "MonsterDodge",
    Callback = function(Value)
        dodgeActive = Value

        if not Value then return end

        local platform      = nil
        local dodging       = false
        local dodgeConns    = {}
        local heartbeatConn = nil

        local function cleanupDodge()
            if heartbeatConn then heartbeatConn:Disconnect() heartbeatConn = nil end
            if platform and platform.Parent then platform:Destroy() platform = nil end
        end

        local function freezePlayer(freeze)
            local hum  = GetHumanoid()
            local root = GetRootPart()
            if hum then
                hum.WalkSpeed = freeze and 0 or 16
                hum.JumpPower = freeze and 0 or 50
            end
            if root then root.Anchored = freeze end
        end

        local function returnPlayer(savedCFrame, reason)
            cleanupDodge()
            local newRoot = GetRootPart()
            if newRoot then
                newRoot.Anchored = false
                newRoot.CFrame = savedCFrame
            end
            freezePlayer(false)
            dodging = false
            NotifyOnce("dodge_return", "🛡 Monster Dodge", reason .. " Returning.", 3, "shield-off")
        end

        local function startDodge(monsterName, monsterInstance)
            if dodging or not dodgeActive then return end
            dodging = true

            local root = GetRootPart()
            if not root then dodging = false return end

            local savedCFrame = root.CFrame

            freezePlayer(true)

            -- 80 studs acima
            platform = Instance.new("Part")
            platform.Size = Vector3.new(10, 1, 10)
            platform.CFrame = CFrame.new(savedCFrame.Position + Vector3.new(0, 80, 0))
            platform.Anchored = true
            platform.CanCollide = true
            platform.Transparency = 0.3
            platform.Material = Enum.Material.SmoothPlastic
            platform.BrickColor = BrickColor.new("Bright blue")
            platform.Parent = workspace

            root.Anchored = false
            root.CFrame = CFrame.new(platform.Position + Vector3.new(0, 4, 0))
            task.wait(0.05)
            root.Anchored = true

            local targetCFrame = CFrame.new(platform.Position + Vector3.new(0, 4, 0))
            heartbeatConn = RunService.Heartbeat:Connect(function()
                if not dodging then return end
                local r = GetRootPart()
                if r then r.CFrame = targetCFrame end
            end)

            NotifyOnce("dodge_" .. monsterName, "🛡 Monster Dodge", monsterName .. " spawned! Holding for up to 40s...", 5, "shield")

            local returned = false

            -- Timeout de 40 segundos
            task.delay(40, function()
                if dodging and not returned then
                    returned = true
                    returnPlayer(savedCFrame, monsterName .. " timeout.")
                end
            end)

            -- Aguarda monstro desaparecer
            local waitConn
            waitConn = monsterInstance.AncestryChanged:Connect(function()
                if monsterInstance:IsDescendantOf(workspace) or
                   monsterInstance:IsDescendantOf(workspace.GameplayFolder.Monsters) then return end
                waitConn:Disconnect()
                if not returned then
                    returned = true
                    returnPlayer(savedCFrame, monsterName .. " gone.")
                end
            end)
        end

        local function listenContainer(container)
            table.insert(dodgeConns, container.ChildAdded:Connect(function(child)
                if dodgeActive and MONSTERS[child.Name] then
                    startDodge(child.Name, child)
                end
            end))
        end

        listenContainer(workspace)
        listenContainer(workspace.GameplayFolder.Monsters)

        -- Monitora desligamento do toggle
        task.spawn(function()
            while dodgeActive do task.wait(0.3) end
            -- Toggle foi desligado
            for _, c in ipairs(dodgeConns) do c:Disconnect() end
            dodgeConns = {}
            cleanupDodge()
            freezePlayer(false)
            dodging = false
        end)
    end
})

-- ================================================
-- ANTIS
-- ================================================

Anti:CreateSection("Monsters")

Anti:CreateToggle({
    Name = "Remove Eyefestation", CurrentValue = false, Flag = "RemoveEyefestation",
    Callback = function(Value)
        local conn = nil
        local muteConn = nil

        local function muteSong(mute)
            pcall(function()
                local song = Player.PlayerGui.Main.Client.MainClient.LocalEntities.Eyefestation.Song
                song.Volume = mute and 0 or 1
                if mute and not muteConn then
                    muteConn = song:GetPropertyChangedSignal("Volume"):Connect(function()
                        if song.Volume > 0 then song.Volume = 0 end
                    end)
                end
            end)
        end

        if Value then
            local function removeInRoom(room)
                for _, d in ipairs(room:GetDescendants()) do
                    if d.Name == "EyefestationSpawn" then pcall(function() d:Destroy() end) end
                end
            end
            for _, room in ipairs(Rooms:GetChildren()) do
                task.defer(function() removeInRoom(room) end)
            end
            conn = Rooms.ChildAdded:Connect(function(room)
                task.wait(0.1) removeInRoom(room)
                room.DescendantAdded:Connect(function(d)
                    if d.Name == "EyefestationSpawn" then pcall(function() d:Destroy() end) end
                end)
            end)
            muteSong(true)
        else
            if conn then conn:Disconnect() conn = nil end
            if muteConn then muteConn:Disconnect() muteConn = nil end
            muteSong(false)
        end
    end
})

-- Anti WallDweller — remove do workspace diretamente
Anti:CreateToggle({
    Name = "Remove WallDweller", CurrentValue = false, Flag = "RemoveWallDweller",
    Callback = function(Value)
        local conn = nil
        local active = Value
        local WALLDWELLER_NAMES = {
            WallDweller = true,
            MeatWallDweller = true,
            RottenWallDweller = true,
            WallDwellers = true,
        }

        if Value then
            -- Remove existentes no workspace
            for _, child in ipairs(workspace:GetChildren()) do
                if WALLDWELLER_NAMES[child.Name] then
                    pcall(function() child:Destroy() end)
                end
            end
            -- Remove novos
            conn = workspace.ChildAdded:Connect(function(child)
                if active and WALLDWELLER_NAMES[child.Name] then
                    pcall(function() child:Destroy() end)
                end
            end)
        else
            active = false
            if conn then conn:Disconnect() conn = nil end
        end
    end
})

Anti:CreateToggle({
    Name = "Remove Statue", CurrentValue = false, Flag = "RemoveStatue",
    Callback = function(Value)
        local conn = nil
        local active = Value
        if Value then
            for _, d in ipairs(workspace.GameplayFolder.Monsters:GetChildren()) do
                if d.Name == "StatueRoot" then pcall(function() d:Destroy() end) end
            end
            conn = workspace.GameplayFolder.Monsters.ChildAdded:Connect(function(child)
                if active and child.Name == "StatueRoot" then
                    pcall(function() child:Destroy() end)
                end
            end)
        else
            active = false
            if conn then conn:Disconnect() conn = nil end
        end
    end
})

CreateAntiToggle(Anti, "Remove DiVine",         "RemoveDiVine",       function(n) return n == "DiVine" or n == "DiVineRoot" end)
CreateAntiToggle(Anti, "Remove Searchlights",   "RemoveSearchlights", function(n) return n == "Searchlights" end)
CreateAntiToggle(Anti, "Remove Monster Locker", "RemoveMonsterLocker",function(n) return n == "MonsterLocker" end)

Anti:CreateSection("Spawns")
CreateAntiToggle(Anti, "Remove Turrets",   "RemoveTurrets",   function(n) return n == "Turret" or string.match(n, "^TurretSpawn") ~= nil end)
CreateAntiToggle(Anti, "Remove Tripwires", "RemoveTripwires", function(n) return n == "Tripwire" or n == "TripwireSpawn" end)
CreateAntiToggle(Anti, "Remove Landmines", "RemoveLandmines", function(n) return n == "Landmine" or n == "LandmineSpawn" end)

Anti:CreateSection("Encounters")

Anti:CreateToggle({
    Name = "Remove Firewall", CurrentValue = false, Flag = "RemoveFirewall",
    Callback = function(Value)
        local conn = nil
        local active = Value
        if Value then
            local fw = workspace:FindFirstChild("Firewall")
            if fw then fw:Destroy() end
            conn = workspace.ChildAdded:Connect(function(child)
                if active and child.Name == "Firewall" then
                    pcall(function() child:Destroy() end)
                end
            end)
        else
            active = false
            if conn then conn:Disconnect() conn = nil end
        end
    end
})

Rayfield:LoadConfiguration()

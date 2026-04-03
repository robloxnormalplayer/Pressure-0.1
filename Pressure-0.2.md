local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Pressure 0.2",
    Icon = 0,
    LoadingTitle = "Pressure 0.2",
    LoadingSubtitle = "by normalplayer",
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
    Title = "Pressure 0.2",
    Content = "Please report any bugs in the comments of ScriptBlox, or leave a suggestion!",
    Duration = 8,
    Image = "message-circle",
})

local Esp    = Window:CreateTab("Visuals", "eye")
local Auto   = Window:CreateTab("Automations", "zap")
local Anti   = Window:CreateTab("Antis", "shield")

local WHITE  = Color3.fromRGB(255, 255, 255)
local RED    = Color3.fromRGB(255, 0, 0)
local Rooms  = workspace.GameplayFolder.Rooms
local Player = game:GetService("Players").LocalPlayer

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

-- ================================================
-- ESP FUNCTIONS
-- ================================================

local function AddESP(part, config)
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
    local h = part:FindFirstChildOfClass("Highlight")
    if h then h:Destroy() end
    local b = part:FindFirstChild("ESP_Label")
    if b then b:Destroy() end
end

local function ScanRooms(matchFn, onFound)
    local connections = {}
    local roomConn = nil

    local function scanDesc(d)
        local config = matchFn(d.Name)
        if config then
            local target = d:FindFirstChild("ProxyPart") or d
            onFound(target, config)
        end
    end

    local function scanRoom(room)
        for _, d in pairs(room:GetDescendants()) do pcall(scanDesc, d) end
    end

    for _, room in pairs(Rooms:GetChildren()) do
        scanRoom(room)
        table.insert(connections, room.DescendantAdded:Connect(function(d)
            task.wait(0.1) pcall(scanDesc, d)
        end))
    end

    roomConn = Rooms.ChildAdded:Connect(function(room)
        task.wait(0.5) scanRoom(room)
        table.insert(connections, room.DescendantAdded:Connect(function(d)
            task.wait(0.1) pcall(scanDesc, d)
        end))
    end)

    return connections, roomConn
end

local function CreateESPToggle(tab, name, flag, matchFn)
    local connections = {}
    local roomConn = nil
    tab:CreateToggle({
        Name = name, CurrentValue = false, Flag = flag,
        Callback = function(Value)
            if Value then
                connections, roomConn = ScanRooms(matchFn, function(target, config)
                    AddESP(target, config)
                end)
            else
                if roomConn then roomConn:Disconnect() roomConn = nil end
                for _, c in pairs(connections) do c:Disconnect() end
                connections = {}
                for _, room in pairs(Rooms:GetChildren()) do
                    for _, d in pairs(room:GetDescendants()) do
                        pcall(RemoveESP, d:FindFirstChild("ProxyPart") or d)
                    end
                end
            end
        end
    })
end

local function CreateAntiToggle(tab, name, flag, matchFn)
    local conn = nil
    tab:CreateToggle({
        Name = name, CurrentValue = false, Flag = flag,
        Callback = function(Value)
            if Value then
                local function removeInRoom(room)
                    for _, d in pairs(room:GetDescendants()) do
                        if matchFn(d.Name) then pcall(function() d:Destroy() end) end
                    end
                end
                for _, room in pairs(Rooms:GetChildren()) do removeInRoom(room) end
                conn = Rooms.ChildAdded:Connect(function(room)
                    task.wait(0.1)
                    removeInRoom(room)
                    room.DescendantAdded:Connect(function(d)
                        if matchFn(d.Name) then pcall(function() d:Destroy() end) end
                    end)
                end)
            else
                if conn then conn:Disconnect() conn = nil end
            end
        end
    })
end

-- ================================================
-- VISUALS TAB
-- ================================================

Esp:CreateSection("Items")

CreateESPToggle(Esp, "Keycard ESP", "EspKeycard",
    function(n) return KEYCARDS[n] end
)
CreateESPToggle(Esp, "Currency & Blueprint ESP", "EspCurrency",
    function(n) return GetCurrencyConfig(n) end
)
CreateESPToggle(Esp, "Item ESP", "EspItems",
    function(n) return GetItemConfig(n) end
)

Esp:CreateSection("Monsters")

-- Monster Locker ESP
local lockerConns, lockerRoomConn = {}, nil
Esp:CreateToggle({
    Name = "Monster Locker ESP", CurrentValue = false, Flag = "EspMonsterLocker",
    Callback = function(Value)
        local cfg = { label = "⚠ MONSTER LOCKER", fill = Color3.fromRGB(200, 0, 0), outline = Color3.fromRGB(255, 80, 80), textColor = Color3.fromRGB(255, 80, 80), fillTransparency = 0.3 }
        if Value then
            lockerConns, lockerRoomConn = ScanRooms(
                function(n) return n == "MonsterLocker" and cfg or nil end,
                function(target, config) AddESP(target, config) end
            )
        else
            if lockerRoomConn then lockerRoomConn:Disconnect() lockerRoomConn = nil end
            for _, c in pairs(lockerConns) do c:Disconnect() end
            lockerConns = {}
            for _, room in pairs(Rooms:GetChildren()) do
                for _, d in pairs(room:GetDescendants()) do
                    if d.Name == "MonsterLocker" then pcall(RemoveESP, d) end
                end
            end
        end
    end
})

-- Fake Door ESP
local fakeDoorConns, fakeDoorRoomConn = {}, nil
Esp:CreateToggle({
    Name = "Fake Door ESP", CurrentValue = false, Flag = "EspFakeDoor",
    Callback = function(Value)
        local cfg = { label = "✖ FAKE DOOR", fill = Color3.fromRGB(180, 0, 0), outline = RED, textColor = RED, fillTransparency = 0.3 }
        if Value then
            fakeDoorConns, fakeDoorRoomConn = ScanRooms(
                function(n) return n == "Door" and cfg or nil end,
                function(target, config)
                    if target.Parent and target.Parent.Name == "TricksterDoor" then
                        AddESP(target, config)
                    end
                end
            )
        else
            if fakeDoorRoomConn then fakeDoorRoomConn:Disconnect() fakeDoorRoomConn = nil end
            for _, c in pairs(fakeDoorConns) do c:Disconnect() end
            fakeDoorConns = {}
            for _, room in pairs(Rooms:GetChildren()) do
                for _, d in pairs(room:GetDescendants()) do
                    if d.Name == "Door" and d.Parent and d.Parent.Name == "TricksterDoor" then
                        pcall(RemoveESP, d)
                    end
                end
            end
        end
    end
})

-- Monster ESP
local monsterEspConns = {}
Esp:CreateToggle({
    Name = "Monster ESP", CurrentValue = false, Flag = "EspMonster",
    Callback = function(Value)
        local cfg = { fill = Color3.fromRGB(200, 0, 0), outline = Color3.fromRGB(255, 200, 0), textColor = RED, fillTransparency = 0.3 }
        local tracked = {}
        local function applyESP(child)
            if tracked[child] then return end
            tracked[child] = true
            cfg.label = "☠ " .. child.Name
            pcall(AddESP, child, cfg)
            table.insert(monsterEspConns, child.AncestryChanged:Connect(function()
                if not child:IsDescendantOf(game) then
                    pcall(RemoveESP, child)
                    tracked[child] = nil
                end
            end))
        end
        local function scanContainer(container)
            for _, child in pairs(container:GetChildren()) do
                if MONSTERS[child.Name] then applyESP(child) end
            end
            table.insert(monsterEspConns, container.ChildAdded:Connect(function(child)
                if MONSTERS[child.Name] then applyESP(child) end
            end))
        end
        if Value then
            scanContainer(workspace)
            scanContainer(workspace.GameplayFolder.Monsters)
        else
            for _, c in pairs(monsterEspConns) do c:Disconnect() end
            monsterEspConns = {}
            for _, child in pairs(workspace:GetChildren()) do
                if MONSTERS[child.Name] then pcall(RemoveESP, child) end
            end
            for _, child in pairs(workspace.GameplayFolder.Monsters:GetChildren()) do
                pcall(RemoveESP, child)
            end
        end
    end
})

-- Monster Alert
local monsterAlertConns = {}
Esp:CreateToggle({
    Name = "Monster Alert", CurrentValue = false, Flag = "MonsterAlert",
    Callback = function(Value)
        local function listenContainer(container)
            table.insert(monsterAlertConns, container.ChildAdded:Connect(function(child)
                if MONSTERS[child.Name] then
                    Rayfield:Notify({
                        Title = "⚠️ Monster Detected!",
                        Content = child.Name .. " has spawned!",
                        Duration = 5,
                        Image = "alert-triangle",
                    })
                end
            end))
        end
        if Value then
            listenContainer(workspace)
            listenContainer(workspace.GameplayFolder.Monsters)
        else
            for _, c in pairs(monsterAlertConns) do c:Disconnect() end
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
            for _, e in pairs(L:GetChildren()) do
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

Auto:CreateToggle({
    Name = "Monster Dodge", CurrentValue = false, Flag = "MonsterDodge",
    Callback = function(Value)
        if not Value then return end

        local platform = nil
        local dodging  = false
        local dodgeConns = {}

        local function stopDodge()
            dodging = false
            if platform and platform.Parent then
                platform:Destroy()
                platform = nil
            end
        end

        local function startDodge(monsterName, monsterInstance)
            if dodging then return end
            dodging = true

            local root = GetRootPart()
            if not root then dodging = false return end

            local savedCFrame = root.CFrame

            -- Cria plataforma acima
            platform = Instance.new("Part")
            platform.Size = Vector3.new(8, 1, 8)
            platform.CFrame = CFrame.new(savedCFrame.Position + Vector3.new(0, 25, 0))
            platform.Anchored = true
            platform.CanCollide = true
            platform.Transparency = 0.4
            platform.Material = Enum.Material.SmoothPlastic
            platform.BrickColor = BrickColor.new("Bright blue")
            platform.Parent = workspace

            root.CFrame = CFrame.new(platform.Position + Vector3.new(0, 3, 0))

            Rayfield:Notify({
                Title = "🛡 Monster Dodge",
                Content = monsterName .. " spawned! Lifting up until it despawns...",
                Duration = 5,
                Image = "shield",
            })

            -- Espera o monstro desaparecer
            local waitConn
            waitConn = monsterInstance.AncestryChanged:Connect(function()
                if not monsterInstance:IsDescendantOf(workspace) and
                   not monsterInstance:IsDescendantOf(workspace.GameplayFolder.Monsters) then
                    waitConn:Disconnect()

                    -- Volta ao local original
                    local newRoot = GetRootPart()
                    if newRoot then
                        newRoot.CFrame = savedCFrame
                    end

                    stopDodge()

                    Rayfield:Notify({
                        Title = "🛡 Monster Dodge",
                        Content = monsterName .. " is gone. Returning to position.",
                        Duration = 3,
                        Image = "shield-off",
                    })
                end
            end)
        end

        -- Escuta spawn de monstros nos dois containers
        local function listenContainer(container)
            local conn = container.ChildAdded:Connect(function(child)
                if Value and MONSTERS[child.Name] then
                    startDodge(child.Name, child)
                end
            end)
            table.insert(dodgeConns, conn)
        end

        listenContainer(workspace)
        listenContainer(workspace.GameplayFolder.Monsters)

        -- Espera toggle desligar para limpar
        task.spawn(function()
            while Value do task.wait(0.5) end
            for _, c in pairs(dodgeConns) do c:Disconnect() end
            dodgeConns = {}
            stopDodge()
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
                for _, d in pairs(room:GetDescendants()) do
                    if d.Name == "EyefestationSpawn" then pcall(function() d:Destroy() end) end
                end
            end
            for _, room in pairs(Rooms:GetChildren()) do removeInRoom(room) end
            conn = Rooms.ChildAdded:Connect(function(room)
                task.wait(0.1)
                removeInRoom(room)
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

CreateAntiToggle(Anti, "Remove DiVine",       "RemoveDiVine",       function(n) return n == "DiVine" or n == "DiVineRoot" end)
CreateAntiToggle(Anti, "Remove Searchlights", "RemoveSearchlights", function(n) return n == "Searchlights" end)

Anti:CreateSection("Spawns")
CreateAntiToggle(Anti, "Remove Turrets",   "RemoveTurrets",   function(n) return n == "Turret" end)
CreateAntiToggle(Anti, "Remove Tripwires", "RemoveTripwires", function(n) return n == "Tripwire" end)
CreateAntiToggle(Anti, "Remove Landmines", "RemoveLandmines", function(n) return n == "Landmine" end)

Rayfield:LoadConfiguration()

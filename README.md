local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Rayfield/releases/main/source.lua"))()

-- Ana Pencere
local Window = Rayfield:CreateWindow({
    Name = "🎮 Ultimate Hile Menüsü",
    LoadingTitle = "Hileler Yükleniyor...",
    LoadingSubtitle = "by Sirius",
    Theme = "Default"
})

-- Hareket Tab
local MovementTab = Window:CreateTab("🚀 Hareket", 4483362458)

-- FLY
MovementTab:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            -- Fly scripti
            local plr = game.Players.LocalPlayer
            local mouse = plr:GetMouse()
            localplayer = plr
            
            if workspace:FindFirstChild("Core") then
                workspace.Core:Destroy()
            end
            
            local Core = Instance.new("Part")
            Core.Name = "Core"
            Core.Size = Vector3.new(0.05, 0.05, 0.05)
            
            spawn(function()
                Core.Parent = workspace
                local Weld = Instance.new("Weld", Core)
                Weld.Part0 = Core
                Weld.Part1 = localplayer.Character.LowerTorso
                Weld.C0 = CFrame.new(0, 0, 0)
            end)
            
            workspace:WaitForChild("Core")
            
            local torso = workspace.Core
            flying = true
            local speed = 50
            local keys = {a = false, d = false, w = false, s = false}
            local e1
            local e2
            
            function start()
                local pos = Instance.new("BodyPosition", torso)
                local gyro = Instance.new("BodyGyro", torso)
                pos.Name = "EPIXPOS"
                pos.maxForce = Vector3.new(math.huge, math.huge, math.huge)
                pos.position = torso.Position
                gyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
                gyro.cframe = torso.CFrame
                
                repeat
                    wait()
                    localplayer.Character.Humanoid.PlatformStand = true
                    local new = gyro.cframe - gyro.cframe.p + pos.position
                    
                    if not keys.w and not keys.s and not keys.a and not keys.d then
                        speed = 50
                    end
                    
                    if keys.w then
                        new = new + workspace.CurrentCamera.CoordinateFrame.lookVector * speed
                        speed = speed + 0
                    end
                    
                    if keys.s then
                        new = new - workspace.CurrentCamera.CoordinateFrame.lookVector * speed
                        speed = speed + 0
                    end
                    
                    if keys.d then
                        new = new * CFrame.new(speed, 0, 0)
                        speed = speed + 0
                    end
                    
                    if keys.a then
                        new = new * CFrame.new(-speed, 0, 0)
                        speed = speed + 0
                    end
                    
                    if speed > 10 then
                        speed = 50
                    end
                    
                    pos.position = new.p
                    
                    if keys.w then
                        gyro.cframe = workspace.CurrentCamera.CoordinateFrame * CFrame.Angles(-math.rad(speed * 0), 0, 0)
                    elseif keys.s then
                        gyro.cframe = workspace.CurrentCamera.CoordinateFrame * CFrame.Angles(math.rad(speed * 0), 0, 0)
                    else
                        gyro.cframe = workspace.CurrentCamera.CoordinateFrame
                    end
                until flying == false
                
                if gyro then gyro:Destroy() end
                if pos then pos:Destroy() end
                flying = false
                localplayer.Character.Humanoid.PlatformStand = false
                speed = 50
            end
            
            e1 = mouse.KeyDown:connect(function(key)
                if not torso or not torso.Parent then flying = false e1:disconnect() e2:disconnect() return end
                if key == "w" then keys.w = true
                elseif key == "s" then keys.s = true
                elseif key == "a" then keys.a = true
                elseif key == "d" then keys.d = true
                elseif key == "x" then flying = false e1:disconnect() e2:disconnect() end
            end)
            
            e2 = mouse.KeyUp:connect(function(key)
                if key == "w" then keys.w = false
                elseif key == "s" then keys.s = false
                elseif key == "a" then keys.a = false
                elseif key == "d" then keys.d = false end
            end)
            
            start()
        else
            flying = false
            if workspace:FindFirstChild("Core") then
                workspace.Core:Destroy()
            end
        end
    end
})

-- NOCLIP
MovementTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            noclip = true
            game:GetService('RunService').Stepped:connect(function()
                if noclip then
                    game.Players.LocalPlayer.Character.Humanoid:ChangeState(11)
                end
            end)
        else
            noclip = false
        end
    end
})

-- SPEED HACK
MovementTab:CreateSlider({
    Name = "Hız Çarpanı",
    Range = {1, 100},
    Increment = 1,
    Suffix = "x",
    CurrentValue = 16,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})

-- JUMP POWER
MovementTab:CreateSlider({
    Name = "Zıplama Gücü",
    Range = {50, 200},
    Increment = 5,
    Suffix = "power",
    CurrentValue = 50,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})

-- OYUNCU Tab
local PlayerTab = Window:CreateTab("👤 Oyuncu", 4483362458)

-- GOD MODE
PlayerTab:CreateToggle({
    Name = "God Mode (Ölümsüzlük)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            game.Players.LocalPlayer.Character.Humanoid.Name = "1"
            local l = game.Players.LocalPlayer.Character["1"]:Clone()
            l.Parent = game.Players.LocalPlayer.Character
            l.Name = "Humanoid"
            wait(0.1)
            game.Players.LocalPlayer.Character["1"]:Destroy()
            game:GetService("Workspace").CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
            game.Players.LocalPlayer.Character.Animate.Disabled = true
            wait(0.1)
            game.Players.LocalPlayer.Character.Animate.Disabled = false
        else
            game.Players.LocalPlayer.Character.Humanoid.Health = 0
        end
    end
})

-- INFINITE JUMP
PlayerTab:CreateToggle({
    Name = "Sonsuz Zıplama",
    CurrentValue = false,
    Callback = function(Value)
        infinitejump = Value
        game:GetService("UserInputService").JumpRequest:connect(function()
            if infinitejump then
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")
            end
        end)
    end
})

-- GÖRÜNÜRLÜK Tab
local VisualsTab = Window:CreateTab("👁️ Görünürlük", 4483362458)

-- ESP
VisualsTab:CreateToggle({
    Name = "ESP (Oyuncuları Gör)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            -- ESP scripti
            function createESP(player)
                local highlight = Instance.new("Highlight")
                highlight.Parent = player.Character
                highlight.Adornee = player.Character
                highlight.FillColor = Color3.fromRGB(255, 0, 0)
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0
            end
            
            for _, player in ipairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer then
                    createESP(player)
                end
            end
            
            game.Players.PlayerAdded:Connect(function(player)
                player.CharacterAdded:Connect(function()
                    createESP(player)
                end)
            end)
        else
            for _, player in ipairs(game.Players:GetPlayers()) do
                if player.Character then
                    local highlight = player.Character:FindFirstChild("Highlight")
                    if highlight then
                        highlight:Destroy()
                    end
                end
            end
        end
    end
})

-- X-RAY
VisualsTab:CreateToggle({
    Name = "X-Ray (Duvar Görme)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            for _, part in ipairs(workspace:GetDescendants()) do
                if part:IsA("BasePart") and part.Transparency == 0 then
                    part.LocalTransparencyModifier = 0.5
                end
            end
        else
            for _, part in ipairs(workspace:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.LocalTransparencyModifier = 0
                end
            end
        end
    end
})

-- OTOMASYON Tab
local AutomationTab = Window:CreateTab("⚡ Otomasyon", 4483362458)

-- AUTO CLICK
local autoClick = false
AutomationTab:CreateToggle({
    Name = "Auto Click (Otomatik Tıklama)",
    CurrentValue = false,
    Callback = function(Value)
        autoClick = Value
        if Value then
            spawn(function()
                while autoClick do
                    wait(0.1) -- Her 0.1 saniyede bir tıklama
                    mouse1click()
                end
            end)
        end
    end
})

-- AUTO FARM (Örnek - oyuna göre değiştirilebilir)
AutomationTab:CreateToggle({
    Name = "Auto Farm (Beta)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            -- Bu kısım oyuna özel olarak düzenlenmeli
            Rayfield:Notify({
                Title = "Auto Farm",
                Content = "Bu özellik oyuna göre ayarlanmalıdır",
                Duration = 3
            })
        end
    end
})

-- TELEPORT
AutomationTab:CreateButton({
    Name = "Spawn'a Işınlan",
    Callback = function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 100, 0)
    end
})

-- AYARLAR Tab
local SettingsTab = Window:CreateTab("⚙️ Ayarlar", 4483362458)

-- THEME DEĞİŞTİRME
SettingsTab:CreateDropdown({
    Name = "Tema Seç",
    Options = {"Default", "Ocean", "AmberGlow", "Light"},
    CurrentOption = "Default",
    Callback = function(Option)
        Rayfield:SetTheme(Option)
    end
})

-- UI KAPATMA
SettingsTab:CreateKeybind({
    Name = "Arayüzü Kapat/Aç",
    CurrentKeybind = "K",
    HoldToInteract = false,
    Callback = function(Keybind)
        Rayfield:Destroy()
    end
})

-- BİLDİRİM
Rayfield:Notify({
    Title = "Hile Menüsü Yüklendi",
    Content = "Tüm özellikler hazır! Dikkatli kullanın.",
    Duration = 5
})

-- LOAD MESSAGE
print("🎯 Ultimate Hile Menüsü Aktif!")
print("🚀 Fly: W,A,S,D ile uç")
print("👁️ ESP ile oyuncuları gör")
print("⚡ Auto Click aktif!")

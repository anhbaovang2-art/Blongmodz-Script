-- =========================================================
-- BlongModz Rayfield (Fixed theme & fix-lag behavior)
-- Giữ nguyên hầu hết code gốc; chỉ sửa theme và phần Fix Lag/UltraFPS safe
-- =========================================================

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local themes = {
    ["BlongModz Dark"] = {
        Name = "BlongModz Dark",
        Accent = Color3.fromRGB(255, 111, 97),
        Background = Color3.fromRGB(20, 20, 20),
        Window = Color3.fromRGB(30, 30, 30),
        Tab = Color3.fromRGB(25, 25, 25),
        Glow = Color3.fromRGB(0, 0, 0),
        FontColor = Color3.fromRGB(255, 255, 255),
    },
    ["Purple"] = {
        Name = "Purple",
        Accent = Color3.fromRGB(138, 43, 226),
        Background = Color3.fromRGB(20, 0, 50),
        Window = Color3.fromRGB(40, 0, 80),
        Tab = Color3.fromRGB(60, 0, 100),
        Glow = Color3.fromRGB(100, 0, 200),
        FontColor = Color3.fromRGB(255, 255, 255),
    },
    ["Aqua"] = {
        Name = "Aqua",
        Accent = Color3.fromRGB(64, 224, 208),
        Background = Color3.fromRGB(0, 30, 30),
        Window = Color3.fromRGB(0, 50, 50),
        Tab = Color3.fromRGB(0, 70, 70),
        Glow = Color3.fromRGB(0, 175, 175),
        FontColor = Color3.fromRGB(255, 255, 255),
    },
    ["Blue"] = {
        Name = "Blue",
        Accent = Color3.fromRGB(0, 122, 204),
        Background = Color3.fromRGB(10, 10, 50),
        Window = Color3.fromRGB(0, 0, 100),
        Tab = Color3.fromRGB(0, 0, 130),
        Glow = Color3.fromRGB(0, 0, 255),
        FontColor = Color3.fromRGB(255, 255, 255),
    },
    ["Green"] = {
        Name = "Green",
        Accent = Color3.fromRGB(0, 255, 0),
        Background = Color3.fromRGB(10, 30, 10),
        Window = Color3.fromRGB(0, 50, 0),
        Tab = Color3.fromRGB(0, 70, 0),
        Glow = Color3.fromRGB(0, 255, 0),
        FontColor = Color3.fromRGB(255, 255, 255),
    }
}

-- >>> FIX: define selectedTheme BEFORE BuildUI (để tránh nil khi tham chiếu)
local selectedTheme = themes["BlongModz Dark"] -- default theme

local Window, MainTab, FixLagTab, SettingTab

-- Safe ApplyTheme: thử Rayfield:SetupTheme nếu có, nếu không thì cố gắng chỉnh UI root
local function ApplyTheme(theme)
    if not theme then return end
    -- try built-in setup first
    pcall(function()
        if Rayfield.SetupTheme then
            Rayfield:SetupTheme(theme)
            return
        end
    end)

    -- fallback: try to change Rayfield.UI properties (nếu tồn tại)
    pcall(function()
        if Rayfield.UI then
            local ui = Rayfield.UI
            -- set some common fields if present
            if ui.BackgroundColor3 ~= nil then
                ui.BackgroundColor3 = theme.Background
            end
            -- traverse children for typical names and set some colors
            for _, v in pairs(ui:GetDescendants()) do
                pcall(function()
                    if (v:IsA("Frame") or v:IsA("ImageLabel") or v:IsA("ImageButton")) and (v.Name:lower():match("main") or v.Name:lower():match("window") or v.Name:lower():match("background") or v.Name:lower():match("container")) then
                        v.BackgroundColor3 = theme.Window or theme.Background
                    end
                    if v:IsA("TextLabel") or v:IsA("TextButton") then
                        v.TextColor3 = theme.FontColor or Color3.new(1,1,1)
                    end
                end)
            end
        end
    end)
end

-- Save original lighting settings to restore later (used by UltraFPS toggle)
local savedLighting = {}
local function saveLightingState()
    local lighting = game:GetService("Lighting")
    savedLighting = {
        GlobalShadows = lighting.GlobalShadows,
        FogEnd = lighting.FogEnd,
        Ambient = lighting.Ambient,
        OutdoorAmbient = lighting.OutdoorAmbient,
        Brightness = lighting.Brightness,
        ClockTime = lighting.ClockTime,
        Technology = (pcall(function() return lighting.Technology end) and lighting.Technology) or nil,
    }
end
local function restoreLightingState()
    local lighting = game:GetService("Lighting")
    if savedLighting.GlobalShadows ~= nil then
        pcall(function() lighting.GlobalShadows = savedLighting.GlobalShadows end)
    end
    if savedLighting.FogEnd ~= nil then
        pcall(function() lighting.FogEnd = savedLighting.FogEnd end)
    end
    if savedLighting.Ambient ~= nil then
        pcall(function() lighting.Ambient = savedLighting.Ambient end)
    end
    if savedLighting.OutdoorAmbient ~= nil then
        pcall(function() lighting.OutdoorAmbient = savedLighting.OutdoorAmbient end)
    end
    if savedLighting.Brightness ~= nil then
        pcall(function() lighting.Brightness = savedLighting.Brightness end)
    end
    if savedLighting.ClockTime ~= nil then
        pcall(function() lighting.ClockTime = savedLighting.ClockTime end)
    end
    if savedLighting.Technology ~= nil then
        pcall(function() lighting.Technology = savedLighting.Technology end)
    end
end

-- Build UI (main)
local function BuildUI()
    -- NOTE: removed Theme = selectedTheme here (was causing nil / unsupported)
    Window = Rayfield:CreateWindow({
        Name = "BlongModz",
        LoadingTitle = "Đang tải script...",
        LoadingSubtitle = "Vui lòng chờ",
        ConfigurationSaving = {
            Enabled = true,
            FolderName = "BlongModz",
            FileName = "config",
        },
    })

    -- Áp dụng theme một cách an toàn sau khi window được tạo
    ApplyTheme(selectedTheme)

    MainTab = Window:CreateTab("Chức năng chính")
    FixLagTab = Window:CreateTab("Fix Lag")
    SettingTab = Window:CreateTab("Cài đặt")

    -- Các nút loadstring sau
    MainTab:CreateButton({
        Name = "Bật Voidware Script",
        Callback = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VW-Add/main/nightsintheforest.lua", true))()
            Rayfield:Notify({Title = "Voidware", Content = "Đã bật script Voidware!", Duration = 3})
        end
    })

    MainTab:CreateButton({
        Name = "Bật Infinite Health (Bất tử)",
        Callback = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/ProBaconHub/DATABASE/refs/heads/main/99%20Nights%20in%20the%20Forest/Infinite%20Health.lua"))()
            Rayfield:Notify({Title = "Bất tử", Content = "Đã bật Infinite Health!", Duration = 3})
        end
    })

    MainTab:CreateButton({Name = "Bật FPS Flick", Callback = function() loadstring(game:HttpGet("https://pastebin.com/raw/MzugyTz9"))() end})
    MainTab:CreateButton({Name = "Bật EvadeEvent (Farm kẹo)", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/gumanba/Scripts/main/EvadeEvent"))() end})
    MainTab:CreateButton({Name = "Bật Quantum Blox Fruit", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/flazhy/QuantumOnyx/refs/heads/main/QuantumOnyx.lua"))() end})
    MainTab:CreateButton({Name = "Bật Fake Dash TSB", Callback = function() loadstring(game:HttpGet("https://raw.githubusercontent.com/Cyborg883/FakeDash/refs/heads/main/Protected_5833389828844912.lua"))() end})
    MainTab:CreateButton({Name = "Bật Fake Emote TSB", Callback = function() loadstring(game:HttpGet("https://rawscripts.net/raw/The-Strongest-Battlegrounds-Gojo-tsb-script-WORKS-ON-SOLARA-18641"))() end})
    MainTab:CreateButton({Name = "Bật Bloxstrap", Callback = function() getgenv().autosetup = { path = 'Bloxstrap', setup = true } loadstring(game:HttpGet('https://raw.githubusercontent.com/qwertyui-is-back/Bloxstrap/main/Initiate.lua'), 'lol')() end})

    MainTab:CreateToggle({
        Name = "Noclip (Không xuyên tường)",
        CurrentValue = false,
        Flag = "NoclipToggle",
        Callback = function(state)
            local noclip = state
            local RunService = game:GetService("RunService")
            local plr = game.Players.LocalPlayer

            -- Lưu reference event để tránh đăng nhiều kết nối
            if noclip then
                -- create connection once
                if not _G.__blong_noclip_conn then
                    _G.__blong_noclip_conn = RunService.Stepped:Connect(function()
                        local chr = plr.Character
                        if noclip and chr and chr:FindFirstChild("HumanoidRootPart") then
                            local hrp = chr.HumanoidRootPart
                            local ray = workspace:Raycast(hrp.Position, Vector3.new(0, -5, 0))
                            if ray and ray.Instance and ray.Instance.CanCollide then
                                hrp.CanCollide = true
                            else
                                hrp.CanCollide = false
                            end
                        elseif chr and chr:FindFirstChild("HumanoidRootPart") then
                            chr.HumanoidRootPart.CanCollide = true
                        end
                    end)
                end
            else
                -- disconnect if exists
                if _G.__blong_noclip_conn then
                    pcall(function() _G.__blong_noclip_conn:Disconnect() end)
                    _G.__blong_noclip_conn = nil
                end
                -- ensure collide true
                local chr = plr.Character
                if chr and chr:FindFirstChild("HumanoidRootPart") then
                    chr.HumanoidRootPart.CanCollide = true
                end
            end
        end
    })

    MainTab:CreateButton({
        Name = "Bật Fly GUI",
        Callback = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
        end
    })

    MainTab:CreateSlider({
        Name = "Tăng tốc độ chạy",
        Range = {16, 100},
        Increment = 1,
        Suffix = "Speed",
        CurrentValue = 16,
        Flag = "SpeedSlider",
        Callback = function(value)
            local plr = game.Players.LocalPlayer
            local humanoid = plr.Character and plr.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = value
            end
        end
    })

    MainTab:CreateSlider({
        Name = "Tăng tốc độ nhảy",
        Range = {50, 150},
        Increment = 1,
        Suffix = "JumpPower",
        CurrentValue = 50,
        Flag = "JumpSlider",
        Callback = function(value)
            local plr = game.Players.LocalPlayer
            local humanoid = plr.Character and plr.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.JumpPower = value
            end
        end
    })

    MainTab:CreateToggle({
        Name = "Anti Void",
        CurrentValue = false,
        Flag = "AntiVoidToggle",
        Callback = function(state)
            antiVoid = state
            local plr = game.Players.LocalPlayer
            -- disconnect previous if any
            if _G.__blong_antivoid_conn then
                pcall(function() _G.__blong_antivoid_conn:Disconnect() end)
                _G.__blong_antivoid_conn = nil
            end
            if state then
                _G.__blong_antivoid_conn = game:GetService("RunService").Heartbeat:Connect(function()
                    local chr = plr.Character
                    local hrp = chr and chr:FindFirstChild("HumanoidRootPart")
                    if hrp and hrp.Position.Y < -10 then
                        hrp.CFrame = CFrame.new(hrp.Position.X, 500, hrp.Position.Z)
                    end
                end)
            end
        end
    })

    -- Fix Lag Tab
    FixLagTab:CreateSection("Chọn phiên bản giảm lag")
    FixLagTab:CreateButton({
        Name = "Giảm Lag v1 (-100 Độ họa)",
        Callback = function()
            local lighting = game:GetService("Lighting")
            saveLightingState() -- save before change
            pcall(function()
                lighting.GlobalShadows = false
                lighting.FogEnd = -100
                Notify = Rayfield.Notify
            end)
            Rayfield:Notify({Title = "Giảm Lag", Content = "Bật giảm lag v1 (-100 độ họa)", Duration = 3})
        end
    })
    FixLagTab:CreateButton({
        Name = "Giảm Lag v2 (-1000 Độ họa)",
        Callback = function()
            local lighting = game:GetService("Lighting")
            saveLightingState()
            pcall(function()
                lighting.GlobalShadows = false
                lighting.FogEnd = -1000
            end)
            Rayfield:Notify({Title = "Giảm Lag", Content = "Bật giảm lag v2 (-1000 độ họa)", Duration = 3})
        end
    })
    FixLagTab:CreateButton({
        Name = "Giảm Lag v3 (Max độ họa)",
        Callback = function()
            local lighting = game:GetService("Lighting")
            saveLightingState()
            pcall(function()
                lighting.GlobalShadows = false
                lighting.FogEnd = -999999999999
            end)
            Rayfield:Notify({Title = "Giảm Lag", Content = "Bật giảm lag v3 (Max độ họa cực mạnh)", Duration = 3})
        end
    })
    FixLagTab:CreateButton({
        Name = "Giảm Lag v4 (Siêu VIP: Bầu trời & mặt đất xám, xóa đầu & chân skin)",
        Callback = function()
            local lighting = game:GetService("Lighting")
            saveLightingState()
            pcall(function()
                lighting.GlobalShadows = false
                lighting.FogEnd = -999999999999
                lighting:SetMinutesAfterMidnight(1200)
                local sky = lighting:FindFirstChildOfClass("Sky")
                if not sky then
                    sky = Instance.new("Sky")
                    sky.Parent = lighting
                end
                -- set sky colors to gray-ish values
                pcall(function()
                    sky.StarColor = Color3.new(0.5,0.5,0.5)
                end)

                for _, part in pairs(workspace:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        pcall(function()
                            part.Color = Color3.fromRGB(128, 128, 128)
                            part.Material = Enum.Material.SmoothPlastic
                        end)
                    end
                end

                local plr = game.Players.LocalPlayer
                local chr = plr.Character
                if chr then
                    local head = chr:FindFirstChild("Head")
                    local leftLeg = chr:FindFirstChild("LeftLowerLeg")
                    local rightLeg = chr:FindFirstChild("RightLowerLeg")
                    if head then pcall(function() head.Transparency = 1 end) end
                    if leftLeg then pcall(function() leftLeg.Transparency = 1 end) end
                    if rightLeg then pcall(function() rightLeg.Transparency = 1 end) end
                end
            end)
            Rayfield:Notify({Title = "Giảm Lag", Content = "Bật giảm lag v4 Siêu VIP", Duration = 3})
        end
    })

    FixLagTab:CreateToggle({
        Name = "Tăng FPS & Giảm Độ Họa",
        CurrentValue = false,
        Flag = "UltraFPS",
        Callback = function(state)
            local lighting = game:GetService("Lighting")
            if state then
                saveLightingState()
                pcall(function()
                    lighting.GlobalShadows = false
                    lighting.FogEnd = -999999999999
                    -- lower details across workspace parts safely
                    for _, part in pairs(workspace:GetDescendants()) do
                        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                            pcall(function()
                                part.Material = Enum.Material.SmoothPlastic
                                if part:FindFirstChildWhichIsA("Decal") then
                                    for _,d in pairs(part:GetDescendants()) do
                                        if d:IsA("Decal") then d.Transparency = 1 end
                                    end
                                end
                                part.Reflectance = 0
                                -- set moderate transparency to reduce render cost (but not break)
                                part.Transparency = 0.3
                            end)
                        end
                    end
                end)
                Rayfield:Notify({Title = "FPS Boost", Content = "Bật tăng FPS cùng giảm đồ họa cực mạnh!", Duration = 3})
            else
                -- restore lighting and try to restore parts
                restoreLightingState()
                pcall(function()
                    for _, part in pairs(workspace:GetDescendants()) do
                        if part:IsA("BasePart") then
                            pcall(function()
                                part.Material = Enum.Material.SmoothPlastic
                                part.Reflectance = 0
                                part.Transparency = 0
                            end)
                        end
                    end
                end)
                Rayfield:Notify({Title = "FPS Boost", Content = "Tắt tăng FPS và khôi phục đồ họa!", Duration = 3})
            end
        end
    })

    -- Hiện FPS 7 màu (giữ nguyên)
    do
        local fpsLabel = Instance.new("TextLabel")
        fpsLabel.Size = UDim2.new(0, 100, 0, 30)
        fpsLabel.Position = UDim2.new(0, 5, 0, 5)
        fpsLabel.BackgroundTransparency = 1
        fpsLabel.TextSize = 24
        fpsLabel.Font = Enum.Font.SourceSansBold
        fpsLabel.Text = "FPS: 0"
        -- safe parent: nếu CoreGui RobloxGui tồn tại thì gán, nếu không, gán vào CoreGui
        local coreGui = game:GetService("CoreGui")
        local ok, robloxGui = pcall(function() return coreGui:FindFirstChild("RobloxGui") end)
        if ok and robloxGui then
            fpsLabel.Parent = robloxGui
        else
            fpsLabel.Parent = coreGui
        end

        local colors = {
            Color3.fromRGB(255, 0, 0),
            Color3.fromRGB(255, 127, 0),
            Color3.fromRGB(255, 255, 0),
            Color3.fromRGB(0, 255, 0),
            Color3.fromRGB(0, 127, 255),
            Color3.fromRGB(0, 0, 255),
            Color3.fromRGB(139, 0, 255),
        }

        local runService = game:GetService("RunService")
        local frameCount = 0
        local timeElapsed = 0
        local colorIndex = 1

        runService.Heartbeat:Connect(function(dt)
            frameCount = frameCount + 1
            timeElapsed = timeElapsed + dt
            if timeElapsed >= 1 then
                fpsLabel.Text = "FPS: " .. frameCount
                fpsLabel.TextColor3 = colors[colorIndex]
                colorIndex = colorIndex + 1
                if colorIndex > #colors then
                    colorIndex = 1
                end
                timeElapsed = 0
                frameCount = 0
            end
        end)
    end

    -- Cài đặt
    SettingTab:CreateToggle({
        Name = "Mở/Đóng menu",
        CurrentValue = true,
        Flag = "MenuToggle",
        Callback = function(state)
            Window:ToggleVisibility()
        end
    })

    SettingTab:CreateDropdown({
        Name = "Chọn Theme",
        Options = {"BlongModz Dark", "Purple", "Aqua", "Blue", "Green"},
        CurrentOption = "BlongModz Dark",
        Callback = function(option)
            if themes[option] then
                selectedTheme = themes[option]
                -- Thay vì destroy toàn bộ GUI (có thể gây lỗi), chỉ apply theme
                ApplyTheme(selectedTheme)
                Rayfield:Notify({Title = "Theme", Content = "Đã đổi sang theme " .. option, Duration = 2})
            end
        end
    })
end

-- Build UI lần đầu
BuildUI()

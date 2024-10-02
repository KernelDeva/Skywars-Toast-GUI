-- Load Kavo UI Library
local success, err = pcall(function()
    return loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Lib/main/source.lua"))()
end)

if not success then
    error("This script must be run in a Roblox environment. Error: " .. tostring(err))
end

-- Create the UI Window
local window = KavoUI:CreateWindow("Skywars Toast GUI by auti4sm")

-- Initialize global variables
_G.ReachEnabled = false
_G.ReachDistance = 10 -- Default reach distance
_G.FloatEnabled = false
_G.FloatHeight = 0.1 -- Default float height
_G.SpinSpeed = 10 -- Default spin speed
_G.HitSound = "" -- Hit sound ID placeholder
_G.LoadingSound = "rbxassetid://294904971" -- Loadup sound ID
_G.GoodbyeSound = "rbxassetid://8284260932" -- Goodbye sound ID

-- Create Tabs
local Tab1 = window:CreateTab("Main")
local Tab2 = window:CreateTab("Settings")
local Tab3 = window:CreateTab("Hitsounds")
local Tab4 = window:CreateTab("Keybinds")

-- Store active connections
local activeConnections = {}

-- Function to handle error-safe execution
local function safeExecution(func)
    local success, err = xpcall(func, debug.traceback)
    if not success then
        toastNotification("Error!", "An error occurred: " .. tostring(err))
    end
end

-- Create Toast Notification Function
local function toastNotification(title, message)
    KavoUI:CreateToast(title, message, 3) -- Duration of 3 seconds
end

-- Function to play the loading sound
local function playLoadingSound()
    local sound = Instance.new("Sound")
    sound.SoundId = _G.LoadingSound
    sound.Parent = workspace
    sound:Play()
    sound.Ended:Wait()
    sound:Destroy()
end

-- Function to play the goodbye sound
local function playGoodbyeSound()
    local sound = Instance.new("Sound")
    sound.SoundId = _G.GoodbyeSound
    sound.Parent = workspace
    sound:Play()
    sound.Ended:Wait()
    sound:Destroy()
end

-- Main UI Functionality
safeExecution(function()
    -- Play loading sound on execution
    playLoadingSound()

    -- Create Reach Option
    Tab1:CreateToggle("Enable Sword Reach", function(state)
        _G.ReachEnabled = state
        toastNotification("Sword Reach", state and "Enabled" or "Disabled")
    end)

    -- Create Reach Input
    Tab2:CreateInput("Sword Reach Distance", "Enter reach distance", function(value)
        _G.ReachDistance = tonumber(value) or 10 -- Default to 10 if input is invalid
        toastNotification("Sword Reach Distance", "Set to " .. _G.ReachDistance)
    end)

    -- Create Float Option
    Tab1:CreateToggle("Enable Float", function(state)
        _G.FloatEnabled = state
        toastNotification("Float", state and "Enabled" or "Disabled")
    end)

    -- Create Float Height Input
    Tab2:CreateInput("Float Height", "Enter float height", function(value)
        _G.FloatHeight = tonumber(value) or 0.1 -- Default to 0.1 if input is invalid
        toastNotification("Float Height", "Set to " .. _G.FloatHeight)
    end)

    -- Create Spin Speed Input
    Tab2:CreateInput("Spin Speed", "Enter spin speed", function(value)
        _G.SpinSpeed = tonumber(value) or 10 -- Default to 10 if input is invalid
        toastNotification("Spin Speed", "Set to " .. _G.SpinSpeed)
    end)

    -- Create Hitsound Input
    Tab3:CreateInput("Hit Sound ID", "Enter hit sound ID", function(value)
        _G.HitSound = "rbxassetid://" .. value -- Set hit sound ID
        toastNotification("Hit Sound ID", "Set to " .. _G.HitSound)
    end)

    -- Create Keybind Tab
    Tab4:CreateToggle("Enable Open/Close Keybind (F)", function(state)
        if state then
            toastNotification("Keybinds", "Press F to toggle the GUI")
        else
            toastNotification("Keybinds", "Keybind disabled")
        end
    end)

    -- Main Update Loop
    local connection
    connection = game:GetService("RunService").RenderStepped:Connect(function()
        local player = game.Players.LocalPlayer
        local character = player.Character

        -- Implement Sword Reach
        if _G.ReachEnabled and character then
            local sword = character:FindFirstChildOfClass("Tool") -- Assuming the sword is the first Tool
            if sword and sword:IsA("Tool") then
                local handle = sword:FindFirstChild("Handle")
                if handle then
                    handle.Size = Vector3.new(1, 1, _G.ReachDistance) -- Set handle size to increase reach
                end
            end
        else
            resetSwordSize(character) -- Reset sword size when reach is disabled
        end

        -- Implement Float
        if _G.FloatEnabled and character and character:FindFirstChild("HumanoidRootPart") then
            local rootPart = character.HumanoidRootPart
            rootPart.Position = rootPart.Position + Vector3.new(0, _G.FloatHeight, 0) -- Adjust float height
        end

        -- Implement Spin
        if _G.SpinSpeed > 0 and character and character:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(_G.SpinSpeed), 0)
        end
    end)

    -- Closing and cleanup actions
    window.OnClose:Connect(function()
        playGoodbyeSound() -- Play goodbye sound on close
        toastNotification("Goodbye!", "Thanks for using the Skywars Toast GUI! 🌸")

        -- Disconnect all active connections
        if connection then
            connection:Disconnect()
        end
        
        -- Additional cleanup tasks
        resetSwordSize(game.Players.LocalPlayer.Character) -- Reset sword size
        _G.ReachEnabled = false -- Reset reach variable
        _G.FloatEnabled = false -- Reset float variable
        _G.SpinSpeed = 10 -- Reset spin speed
        _G.ReachDistance = 10 -- Reset reach distance
        _G.FloatHeight = 0.1 -- Reset float height
        _G.HitSound = "" -- Reset hit sound ID placeholder
    end)

    -- Notify about keybind to close GUI
    toastNotification("Keybind", "Press K to close the GUI")
end)

-- Function to reset the sword size
function resetSwordSize(character)
    local sword = character:FindFirstChildOfClass("Tool") -- Assuming the sword is the first Tool
    if sword and sword:IsA("Tool") then
        local handle = sword:FindFirstChild("Handle")
        if handle then
            handle.Size = Vector3.new(1, 1, 1) -- Reset handle size to default
        end
    end
end

-- Key binding to close the GUI
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed then
        if input.KeyCode == Enum.KeyCode.K then
            window:Destroy() -- Close the GUI when K is pressed
            toastNotification("Goodbye!", "Bye :D") -- Notify user when GUI is closed
        elseif input.KeyCode == Enum.KeyCode.F then
            window.Visible = not window.Visible -- Toggle visibility when F is pressed
            toastNotification("Toggle GUI", window.Visible and "GUI Opened!" or "GUI Closed!")
        end
    end
end)

-- Style all GUI elements with pixel smooth pink and white text
local function styleGUI()
    local pixelFont = Enum.Font.Pixel -- Set pixel font
    local pinkColor = Color3.fromRGB(255, 182, 193) -- Pink color
    local whiteColor = Color3.fromRGB(255, 255, 255) -- White color

    -- Apply styles to tabs, labels, and text
    for _, tab in ipairs(window.Tabs) do
        tab.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Background color
        for _, button in ipairs(tab.Buttons) do
            button.Font = pixelFont
            button.TextColor3 = pinkColor
        end
        for _, label in ipairs(tab.Labels) do
            label.Font = pixelFont
            label.TextColor3 = whiteColor
        end
    end
end

styleGUI() -- Call the styling function

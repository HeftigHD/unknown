
# Raptors Hub UI Lib
This library was made by the Raptors! Let's go Raptors!

## Getting Started
Load the UI Lib from GitHub by doing this:
```lua
local UI = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeftigHD/unknown/main/raps_ui_release.txt"))()
```

## Root API

### Toggle Tabs
```lua
<void> UI.ToggleTabs()
```
This toggles the tabs. You can bind this to a hotkey, such as right control.
Example:
```lua
game:GetService("UserInputService").InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.RightControl then
		UI.ToggleTabs()
	end
end)
```

### Create Tab
```lua
<Tab> UI.CreateTab(<string> name)
```
Creates a tab, see the API for the Tab object below. Make sure the Tab name is unique.

### Notify
```lua
<void> UI.Notify(<Tuple> args)
```
Creates a notification using multiple values as its text. Similar to print/warn.

### Update Theme
```lua
<void> UI.UpdateTheme()
```
Updates the UI theme.
Example:
```lua
UI.Settings.MainColor = Color3.new(0,1,0)
UI.Settings.OffColor = Color3.new(1,0,0)
UI.UpdateTheme()
```

### Settings Table
```lua
<table> UI.Settings
```
A settings table containing the UI colors
| Name | Type |
|--|--|
| MainColor | Color3 |
| OffColor | Color3  |

### Options Table
```lua
<table> UI.Options
```
A table containing the data of the Tab settings, where the key is the Internal Name you specified. Does not include Toggles, use UI.Toggleables to access those. These are stored as `Setting` objects, see the API for it below.
Example:
| Key| Type |
|--|--|
| AimbotOn | Setting|
| BoxSize | Setting|
| ... | ... |

### Tabs Table
```lua
<table> UI.Tabs
```
A table containing all created Tab objects. The tab names are the key.

### Toggleables Table
```lua
<table> UI.Toggleables
```
A table containing all created Toggles. The Toggle object itself is not stored here, rather it is the `obj.Toggle` tables of every Toggle object. The keys in this table are the internal names of the Toggles. Refer to the Toggle table of the Toggle class below.

## Classes
Some of the classes in the UI Lib. Some of these are returned as objects by the UI Lib.
## Class Tab
The API for the Tab object.
### Add Toggle
```lua
<Toggle> obj.AddToggle(<string> Internal Name, <string> Display Name, [<function> callback])
```
Adds a toggle button to the tab, you can specify an optional callback for when the toggle is pressed. Make sure Internal Name is unique among your other Toggles and Settings. This can be used to hold settings. Information about the Toggle object can be found below.
### Add Text
```lua
<Text> obj.AddText(<string> Text)
```
Adds a text settings container to the tab. This can be used to hold settings. Information about the Text object can be found below.

## Class SettingsContainer
The base class for frames that can contain settings in it. Only inherited by Toggle and Text.
### Add Setting
```lua
<void> obj.AddSetting(
	<string> Internal Name,
	<string> Display Name,
	[<union<string,boolean,number,Color3>> Value]
	[<function> Callback]
	[<string> Type - Valid Values: (KeyListener, Slider)]
	[<string> SliderRange - Format: (X1-X2), only use if Type is Slider]
)
```
Adds a setting to a SettingsContainer object. Make sure Internal Name is unique among your other Toggles and Settings. It can be controlled in UI.Options.
Notes:
- Callback is optional
- Type is optional, but if you set it as `"Slider"`, you must set the `SliderRange` as well.

## Class Toggle - Inherits SettingsContainer
The API for the Toggle object.
### Toggle table
```lua
<table> obj.Toggle
```
Contains the data of the toggle.
| Name | Description | API |
|--|--|--|
| Text| The label text of the toggle | `obj.Toggle.Text` |
| Set | Used to set the value of the toggle (true/false)  | `obj.Toggle.Set(<Variant> value)` |
| OptionValue | The current value of the toggle (true/false) | `obj.Toggle.OptionValue`
| AddSetting | Shortcut to add a setting to this container (Same API as SettingsContainer) | `obj.Toggle.AddSetting(...)`

## Class Text - Inherits SettingsContainer
The API for the Text object. Doesn't have any unique API, only has AddSetting since it is a SettingsContainer.

## Class Setting
The API for a setting object.
### Name
```lua
<string> obj.Name
```
The Internal Name of the setting.

### Option Value
```lua
<Variant> obj.OptionValue
```
The value of the setting.

### Update Function
```lua
<function> obj.Update(...)
```
A function that is ran when the setting is updated. Might not always be present in the object.

## Example
An example UI with the Raptors Hub UI Lib!
```lua
local UI = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeftigHD/unknown/main/raps_ui_release.txt"))()

function createLogger(name)
    return function(...)
        print(name,...)
    end
end

-- Tab 1
local tab1 = UI.CreateTab("Tab One")

local toggle1 = tab1.AddToggle("Toggle1","Toggle One",createLogger("Toggle 1 Updated"))
local text1 = tab1.AddText("Test Text")

toggle1.AddSetting("Set1","A bool",true,createLogger("Setting 1 Updated"))
toggle1.AddSetting("Set2","A string","yo",createLogger("Setting 2 Updated"))
toggle1.AddSetting("Set3","A key","E",createLogger("Setting 3 Updated"),"KeyListener")

text1.AddSetting("Set4","A number",12,createLogger("Setting 4 Updated"))
text1.AddSetting("Set5","A slider",2,createLogger("Setting 5 Updated"),"Slider","1-20")
text1.AddSetting("Set6","A Color",Color3.new(0.5,0.6,0.7),createLogger("Setting 6 Updated"))

-- Tab 2
local tab2 = UI.CreateTab("Tab Two")
tab2.AddText("We hope you enjoy!")
tab2.AddText("Let's go Raptors!")

-- Hotkey
game:GetService("UserInputService").InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.RightControl then
		print("Toggled tabs!")
		UI.ToggleTabs()
	end
end)

-- Example accessing options
while wait(5) do
    print("The color is",UI.Options.Set6.OptionValue)
end
```
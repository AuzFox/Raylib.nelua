<div align="center">
<p>

<img width="200" src="./assets/logo.png"></img>

# Raylib.nelua - [Raylib](https://www.raylib.com/) binding for the [Nelua Programming Language](https://nelua.io/)

</p>

[Installation](./INSTALL.md) • [Development](./DEVELOPMENT.md) • [Contributing](./CONTRIBUTING.md) • [License](./LICENSE)

</div>

**Raylib.nelua** is a [Raylib](https://www.raylib.com/) binding for latest Raylib 5.0 version. A simple and easy-to-use library to learn videogames programming.

Raylib.nelua. It's like Raylib on steroids, except without the harmful side effects. This delightful little binding brings the power and simplicity of Nelua to the world of Raylib game development.

### Code Example

<details>
<summary><b>Raylib - Basic Window</b></summary>

```lua
require "raylib"

-- Initialization
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <comptime> = 800
local SCREEN_HEIGHT: uint16 <comptime> = 450

rl.setConfigFlags(rl.configFlags.VSYNC_HINT) -- Enable VSYNC
rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "raylib-nelua [core] example - basic window")

--------------------------------------------------------------------------------------

-- Main game loop

while not rl.windowShouldClose() do        -- Detect window close button or ESC key
  -- Update
  ----------------------------------------------------------------------------------
  -- TODO: Update your variables here
  ----------------------------------------------------------------------------------

  -- Draw
  ----------------------------------------------------------------------------------
  
  rl.beginDrawing()

    rl.clearBackground(rl.RAYWHITE)

    rl.drawText("Congrats! You created your first window!", 190, 200, 20, rl.LIGHTGRAY)

  rl.endDrawing()
  -----------------------------------------------------------------------------------
end

-- De-Initialization
-------------------------------------------------------------------------------------
rl.closeWindow()       -- Close window and OpenGL context
-------------------------------------------------------------------------------------
```

</details>

<details>
<summary><b>Raylib - Basic Keyboard Input</b></summary>

```lua
require "raylib"

-- Constants
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <const> = 800
local SCREEN_HEIGHT: uint16 <const> = 450
--------------------------------------------------------------------------------------

-- Initialization
--------------------------------------------------------------------------------------

rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "Raylib.nelua [core] example - keyboard input")
rl.setTargetFPS(60)

local ballPosition = rl.vector2{ SCREEN_WIDTH/2, SCREEN_HEIGHT/2 }

-- Main game loop

while not rl.windowShouldClose() do        -- Detect window close button or ESC key
  -- Update
  ----------------------------------------------------------------------------------
  if rl.isKeyDown(rl.keyboardKey.RIGHT) then ballPosition.x = ballPosition.x + 2 end
  if rl.isKeyDown(rl.keyboardKey.LEFT) then ballPosition.x = ballPosition.x - 2 end
  if rl.isKeyDown(rl.keyboardKey.UP) then ballPosition.y = ballPosition.y - 2 end
  if rl.isKeyDown(rl.keyboardKey.DOWN) then ballPosition.y = ballPosition.y + 2 end 

  -- Draw
  ----------------------------------------------------------------------------------
  rl.drawing(function()
    rl.clearBackground(rl.RAYWHITE)

    rl.drawText("move the ballwith arrow keys", 10, 10, 20, rl.DARKGRAY)

    rl.drawCircleV(ballPosition, 50, rl.MAROON)
  end)
end

-- De-Initialization
-------------------------------------------------------------------------------------
rl.closeWindow()       -- Close window and OpenGL context
-------------------------------------------------------------------------------------
```

</details>

<details>
<summary><b>Raylib - Texture Loading and Drawing</b></summary>

```lua
require "raylib"

-- Constants
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <const> = 800
local SCREEN_HEIGHT: uint16 <const> = 450
--------------------------------------------------------------------------------------

-- Initialization
--------------------------------------------------------------------------------------

rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "Raylib.nelua [textures] example - texture loading and drawing")
rl.setTargetFPS(60)

-- NOTE: Textures MUST be loaded after Window initialization (OpenGL context is required)
local texture = rl.loadTexture("./resources/logo.png") -- Texture loading

-- Main game loop

while not rl.windowShouldClose() do        -- Detect window close button or ESC key
   -- Update
   -- TODO: Update your variables here
   ----------------------------------------------------------------------------------

   -- Draw
   ----------------------------------------------------------------------------------
   rl.drawing(function()
      rl.clearBackground(rl.RAYWHITE)

      rl.drawTexture(texture, SCREEN_WIDTH/2 - texture.width/2, SCREEN_HEIGHT/2 - texture.height/2, rl.WHITE)

      rl.drawText("this IS a texture!", 360, 370, 10, rl.GRAY);
    end)
end

-- De-Initialization
-------------------------------------------------------------------------------------
rl.unloadTexture(texture) -- Texture unloading
rl.closeWindow()       -- Close window and OpenGL context
-------------------------------------------------------------------------------------
```

</details>

<details>
<summary><b>Raylib - Export to Web via Emscripten</b></summary>

```lua
require "raylib"
require 'allocators.gc'

##[[
    if PLATFORM_WEB then
     cflags '-O3 -Wall' -- Change your optimisation options to suit your needs.
     cflags './source/dependencies/lib/libraylib.a -I./source/dependencies/include/ -L./source/dependencies/lib/' -- Include & Library locations
     cflags '--preload-file ./source/assets'
     cflags '-s USE_GLFW=3 -DPLATFORM_WEB -s WASM=1 -s USE_WEBGL2=1' -- Recommended to not touch.
    end
]]

## if PLATFORM_WEB then
   collectgarbage('stop') -- conservative GCs cannot run automatically with emscripten
## end

-- Initialization
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <const> = 800
local SCREEN_HEIGHT: uint16 <const> = 450

## if not PLATFORM_WEB then
  rl.setConfigFlags(rl.configFlags.VSYNC_HINT) -- Enable VSYNC if we're building for Desktop
## end

rl.initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "Raylib.nelua [core] example - basic window")

local function update()

  rl.beginDrawing()
    rl.clearBackground(rl.RAYWHITE)
    rl.drawText("Congrats! You created your first window!", 190, 200, 20, rl.LIGHTGRAY)
  rl.endDrawing()

   ## if PLATFORM_WEB then
      collectgarbage() -- safe to collect garbage here
   ## end
end


## if PLATFORM_WEB then
   rl.wasmSetMainLoop(update, 0, 1) -- If building for web, pass wasmSetMainLoop which calls emscripten_set_main_loop. Don't use if you're passing ASYNCIFY flag!
## else

   while not rl.windowShouldClose() do
      update()
   end

##end

--------------------------------------------------------------------------------------


-- De-Initialization
-------------------------------------------------------------------------------------
rl.closeWindow()       -- Close window and OpenGL context
-------------------------------------------------------------------------------------
```

> **Notes**
>
> - Emscripten must be installed on your system.
>
> - You have to provide your own Raylib 4.5-dev (build from source) libraylib.a and include files for web export. [Please read here](https://github.com/raysan5/raylib/wiki/Working-for-Web-(HTML5)).
>
> - Your library and include files have to be placed in a folder "libs"/"include" respectively. You can, however change those settings in raylib.nelua.
>
> - You have to pass `rl.wasmSetMainLoop(UpdateFunctionHere, 0, 1)` function as seen on example above if you're not planning to use `-s ASYNCIFY` flag. [Read more about this here](https://github.com/raysan5/raylib/wiki/Working-for-Web-(HTML5)#42-use-standard-raylib-whilewindowshouldclose-loop).
>
> - To build, pass emcc to to Nelua compiler

</details>

<details>
<summary><b>Raylib - 3D Free Camera</b></summary>

```lua
require "raylib"

-- Constants
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <const> = 800
local SCREEN_HEIGHT: uint16 <const> = 450
--------------------------------------------------------------------------------------

-- Initialization
--------------------------------------------------------------------------------------

rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "Raylib.nelua [core] example - 3d camera free")

local camera = rl.camera3D{} -- Define the camera to look into our 3d world
camera.position = rl.vector3{ 10, 10, 10 } -- Camera position
camera.target = rl.vector3{ 0, 0, 0 } -- Camera looking at point
camera.up = rl.vector3{ 0, 1, 0 } -- Camera up vector (rotation towards target)
camera.fovy = 45 -- Camera field-of-view Y
camera.projection = rl.cameraProjection.PERSPECTIVE -- Camera projection type

local cubePosition = rl.vector3{ 0, 0, 0 }


rl.disableCursor() -- Limit cursor to relative movement inside the window

rl.setTargetFPS(60)
------------------------------------------------------------------------------------

-- Main game loop
while not rl.windowShouldClose() do        -- Detect window close button or ESC key
  
  -- Update
  ----------------------------------------------------------------------------------
  rl.updateCamera(&camera, rl.cameraMode.FREE)
  
  if rl.isKeyDown(rl.keyboardKey.Z) then camera.target = rl.vector3{ 0, 0, 0 } end


  -- Draw
  ----------------------------------------------------------------------------------
  rl.drawing(function()
    rl.clearBackground(rl.RAYWHITE)

    rl.mode3D(camera, function()
      rl.drawCube(cubePosition, 2, 2, 2, rl.RED)
      rl.drawCubeWires(cubePosition, 2, 2, 2, rl.MAROON)

      rl.drawGrid(10, 1)
    end)

    rl.drawRectangle(10, 10, 320, 133, rl.fade(rl.SKYBLUE, 0.5))
    rl.drawRectangleLines( 10, 10, 320, 133, rl.BLUE)

    rl.drawText("Free camera default controls:", 20, 20, 10, rl.BLACK)
    rl.drawText("- Mouse Wheel to Zoom in-out", 40, 40, 10, rl.DARKGRAY)
    rl.drawText("- Mouse Wheel Pressed to Pan", 40, 60, 10, rl.DARKGRAY)
    rl.drawText("- Alt + Mouse Wheel Pressed to Rotate", 40, 80, 10, rl.DARKGRAY)
    rl.drawText("- Alt + Ctrl + Mouse Wheel Pressed for Smooth Zoom", 40, 100, 10, rl.DARKGRAY)
    rl.drawText("- Z to zoom to (0, 0, 0)", 40, 120, 10, rl.DARKGRAY)

  end)
end

-- De-Initialization
-------------------------------------------------------------------------------------
rl.closeWindow()       -- Close window and OpenGL context
-------------------------------------------------------------------------------------
```

</details>

<details>
<summary><b>Raygui - Portable Window</b></summary>

```lua
require "raylib"
require "raygui"

-- Initialization
---------------------------------------------------------------------------------------
local SCREEN_WIDTH <const> = 800
local SCREEN_HEIGHT <const> = 600

rl.setConfigFlags(rl.configFlags.WINDOW_UNDECORATED)
rl.initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "raygui.nelua - portable window")

-- General variables
local mousePosition = rl.vector2{ x = 0, y = 0 }
local windowPosition = rl.vector2{ x = 500, y = 200 }
local panOffset = mousePosition
local dragWindow: boolean = false

rl.setWindowPosition(windowPosition.x, windowPosition.y)

local exitWindowPress: int32

rl.setTargetFPS(60)
--------------------------------------------------------------------------------------

-- Main game loop
while not rlgui.intToBool(exitWindowPress) and not rl.windowShouldClose() do
    -- Update
    ----------------------------------------------------------------------------------
    mousePosition = rl.getMousePosition()

    if rl.isMouseButtonPressed(rl.mouseButton.LEFT_BUTTON) and not dragWindow then
        if rl.checkCollisionPointRec(mousePosition, rl.rectangle{ x = 0, y = 0, width = SCREEN_WIDTH, height = 20 }) then
            dragWindow = true
            panOffset = mousePosition
        end
    end

    if dragWindow then
        windowPosition.x = windowPosition.x + (mousePosition.x - panOffset.x)
        windowPosition.y = windowPosition.y + (mousePosition.y - panOffset.y)

        rl.setWindowPosition(math.floor(windowPosition.x), math.floor(windowPosition.y))

        if rl.isMouseButtonReleased(rl.mouseButton.LEFT_BUTTON) then
            dragWindow = false
        end
    end
    ----------------------------------------------------------------------------------

    -- Draw
    ----------------------------------------------------------------------------------
    rl.beginDrawing()

    rl.clearBackground(rl.RAYWHITE)

    exitWindowPress = rlgui.windowBox({ x = 0, y = 0, width = SCREEN_WIDTH, height = SCREEN_HEIGHT }, "#198# PORTABLE WINDOW")

    rl.drawText(string.format("Mouse Position: [ %.0f, %.0f ]", mousePosition.x, mousePosition.y), 10, 40, 10, rl.DARKGRAY)

    rl.endDrawing()
    ----------------------------------------------------------------------------------
end

-- De-Initialization
--------------------------------------------------------------------------------------
rl.closeWindow() -- Close window and OpenGL context
--------------------------------------------------------------------------------------
```

</details>

<details>
<summary><b>Raygui - Scroll Panel</b></summary>

```lua
require "raylib"
require "raygui"

-- Initialization
--------------------------------------------------------------------------------------
local SCREEN_WIDTH <const> = 800
local SCREEN_HEIGHT <const> = 450

rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "raygui.nelua - Scroll Panel")

rl.setTargetFPS(60)

local panelRec = rl.rectangle{ x = 20, y = 40, width = 200, height = 150 }
local panelContentRec = rl.rectangle{ x = 0, y = 0, width = 340, height = 340 }
local panelView = rl.rectangle{}
local panelScroll = rl.vector2{ x = 99, y = -20 }
local showContentArea = true
---------------------------------------------------------------------------------------

-- Draw and process scroll bar style edition controls
local function drawStyleEditControls()

  local style: int32
  
  -- ScrollPanel style controls
  rlgui.groupBox(rl.rectangle{ x = 550, y = 170, width = 220, height = 205 }, "SCROLLBAR STYLE")

  style = rlgui.getStyle(rlgui.control.SCROLLBAR, rlgui.controlProperty.BORDER_WIDTH)
  rlgui.label(rl.rectangle{ x = 555, y = 195, width = 110, height = 10 }, "BORDER_WIDTH")
  rlgui.spinner(rl.rectangle{ x = 670, y = 190, width = 90, height = 20 }, "", &style, 0, 6, false)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.controlProperty.BORDER_WIDTH, style)

  style = rlgui.getStyle(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_SIZE)
  rlgui.label(rl.rectangle{ x = 555, y = 220, width = 110, height = 10 }, "ARROWS_SIZE")
  rlgui.spinner(rl.rectangle{ x = 670, y = 215, width = 90, height = 20 }, "", &style, 4, 14, false)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_SIZE, style)

  style = rlgui.getStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_PADDING)
  rlgui.label(rl.rectangle{ x = 555, y = 245, width = 110, height = 10 }, "SLIDER_PADDING")
  rlgui.spinner(rl.rectangle{ x = 670, y = 240, width = 90, height = 20 }, "", &style, 0, 14, false)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_PADDING, style)

  local scrollBarArrows = rlgui.getStyleAsBool(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_VISIBLE)
  rlgui.checkBox(rl.rectangle{ 565, 280, 20, 20 }, "ARROWS_VISIBLE", &scrollBarArrows)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_VISIBLE, rlgui.boolToInt(scrollBarArrows))

  style = rlgui.getStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_PADDING)
  rlgui.label(rl.rectangle{ x = 555, y = 325, width = 110, height = 10 }, "SLIDER_PADDING")
  rlgui.spinner(rl.rectangle{ x = 670, y = 320, width = 90, height = 20 }, "", &style, 0, 14, false)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_PADDING, style)

  style = rlgui.getStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_WIDTH)
  rlgui.label(rl.rectangle{ x = 555, y = 350, width = 110, height = 10 }, "SLIDER_WIDTH")
  rlgui.spinner(rl.rectangle{ x = 670, y = 345, width = 90, height = 20 }, "", &style, 2, 100, false)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.sliderProperty.SLIDER_WIDTH, style)

  local text = rlgui.getStyle(rlgui.control.LISTVIEW, rlgui.listViewProperty.SCROLLBAR_SIDE) == rlgui.SCROLLBAR_LEFT_SIDE and "SCROLLBAR: LEFT" or "SCROLLBAR: RIGHT"
  local toggleScrollBarSide = rlgui.getStyle(rlgui.control.LISTVIEW, rlgui.listViewProperty.SCROLLBAR_SIDE)
  rlgui.toggle(rl.rectangle{ x = 560, y = 110, width = 200, height = 35 }, text, &toggleScrollBarSide)
  rlgui.setStyle(rlgui.control.LISTVIEW, rlgui.listViewProperty.SCROLLBAR_SIDE, toggleScrollBarSide)
  ----------------------------------------------------------

  -- ScrollBar style controls
  rlgui.groupBox(rl.rectangle{ x = 550, y = 20, width = 220, height = 135 }, "SCROLLPANEL STYLE")

  style = rlgui.getStyle(rlgui.control.LISTVIEW, rlgui.listViewProperty.SCROLLBAR_WIDTH)
  rlgui.label(rl.rectangle{ x = 555, y = 35, width = 110, height = 10 }, "SCROLLBAR_WIDTH")
  rlgui.spinner(rl.rectangle{ x = 670, y = 30, width = 90, height = 20 }, "", &style, 6, 30, false)
  rlgui.setStyle(rlgui.control.LISTVIEW, rlgui.listViewProperty.SCROLLBAR_WIDTH, style)

  style = rlgui.getStyle(DEFAULT, rlgui.controlProperty.BORDER_WIDTH)
  rlgui.label(rl.rectangle{ x = 555, y = 60, width = 110, height = 10 }, "BORDER_WIDTH")
  rlgui.spinner(rl.rectangle{ x = 670, y = 55, width = 90, height = 20 }, "", &style, 0, 20, false)
  rlgui.setStyle(DEFAULT, rlgui.controlProperty.BORDER_WIDTH, style)
  ----------------------------------------------------------
end

-- Main game loop
while not rl.windowShouldClose() do
    -- Update
    ----------------------------------------------------------------------------------
    -- TODO: Implement required update logic
    ----------------------------------------------------------------------------------

    -- Draw
    ----------------------------------------------------------------------------------
    rl.beginDrawing()

    rl.clearBackground(rl.RAYWHITE)

    rl.drawText(string.format("[%f, %f]", panelScroll.x, panelScroll.y), 4, 4, 20, rl.RED)

    rlgui.scrollPanel(panelRec, "", panelContentRec, panelScroll, panelView)

    rl.beginScissorMode(panelView.x, panelView.y, panelView.width, panelView.height)
    rlgui.grid(rl.rectangle{ x = panelRec.x + panelScroll.x, y = panelRec.y + panelScroll.y, width = panelContentRec.width, height = panelContentRec.height }, "", 16, 3, rl.vector2{})
    rl.endScissorMode()

    if showContentArea then
        rl.drawRectangle(panelRec.x + panelScroll.x, panelRec.y + panelScroll.y, panelContentRec.width, panelContentRec.height, rl.fade(rl.RED, 0.1))
    end

    drawStyleEditControls()

    rlgui.checkBox(rl.rectangle{ x = 565, y = 80, width = 20, height = 20 }, "SHOW CONTENT AREA", &showContentArea)

    rlgui.sliderBar(rl.rectangle{ x = 590, y = 385, width = 145, height = 15 }, "WIDTH", string.format("%0.f", panelContentRec.width), &panelContentRec.width, 1, 600)
    rlgui.sliderBar(rl.rectangle{ x = 590, y = 410, width = 145, height = 15 }, "HEIGHT", string.format("%0.f", panelContentRec.height), &panelContentRec.height, 1, 400)

    rl.endDrawing()
    ----------------------------------------------------------------------------------
end

-- De-Initialization
--------------------------------------------------------------------------------------
rl.closeWindow() -- Close window and OpenGL context
--------------------------------------------------------------------------------------
```

</details>

<p>

More examples can be found [here.](https://github.com/Its-Kenta/Raylib.nelua/tree/main/examples)

</p>

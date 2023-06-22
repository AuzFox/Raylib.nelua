<div align="center">
<p>

<img width="200" src="./assets/logo.png"></img>

# Raylib.nelua - [Raylib](https://www.raylib.com/) binding for the [Nelua Programming Language](https://nelua.io/)

</p>

[Installation](./INSTALL.md) • [Development](./DEVELOPMENT.md) • [Contributing](./CONTRIBUTING.md) • [License](./LICENSE)

</div>

**Raylib.nelua** is a [Raylib](https://www.raylib.com/) binding for latest Raylib 4.5 version. A simple and easy-to-use library to learn videogames programming.

Raylib.nelua. It's like Raylib on steroids, except without the harmful side effects. This delightful little binding brings the power and simplicity of Nelua to the world of Raylib game development.

### Code Example

<details>
<summary><b>Basic Window</b></summary>

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
<summary><b>Basic Keyboard Input</b></summary>

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
<summary><b>Texture Loading and Drawing</b></summary>

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
<summary><b>3D Free Camera</b></summary>

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
<summary><b>Export to Web via Emscripten</b></summary>

```lua
require "raylib"
require 'allocators.gc'

##[[
    if PLATFORM_WEB then
     cflags '-O3 -Wall' -- Change your optimisation options to suit your needs.
     cflags './source/dependencies/lib/libraylib.a -I./source/dependencies/include/ -L./source/dependencies/lib/' -- Include & Library locations
     cflags '--preload-file ./source/assets'
     cflags '-s USE_GLFW=3 -DPLATFORM_WEB -s WASM=1 -s USE_WEBGL2=1 -s ASYNCIFY' -- Recommended to not touch.
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
> - You have to pass `rl.wasmSetMainLoop(UpdateFunctionHere, 0, 1)`  function.
>
> - To build, pass emcc to to Nelua compiler

</detais>

<p>

More examples can be found [here.](https://github.com/Its-Kenta/Raylib.nelua/tree/main/examples)

</p>

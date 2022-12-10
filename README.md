 <a href="https://nelua.io/"><img style="vertical-align:middle" src=https://i.imgur.com/QMUU0ed.png></a>
# Raylib-Nelua
[Nelua](https://nelua.io/) binding for [Raylib](http://www.raylib.com/) a simple and easy-to-use library to learn videogames programming.

*Using Raylib Version 4.5-dev*

## Supported Platforms
- Windows
- MacOS
- Linux
- [Web](#web-export-via-emscripten)

## Covered APIs
- [X] raylib.h - Finished
- [X] raymath.h - Finished, Vector2 and Vector3 math are all using methods, matrix and quaternions use functions!
- [X] web export - Ability to export to web via emscripten. [Please read below](#web-export-via-emscripten).
- [0] easings.h - Planned
- [0] raygui.h - Planned

## Installation

**1.** Download or clone the repository and place **raylib.nelua** in your project folder or anywhere else you want.

**2.** Create your main file that holds your code. (as example main.nelua) and start writing your Raylib code in Nelua:
```lua
require "raylib"

-- Initialization
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <comptime> = 800
local SCREEN_HEIGHT: uint16 <comptime> = 450

rl.setConfigFlags(rl.configFlags.FLAG_VSYNC_HINT) -- Enable VSYNC
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

## Usage

**1.** Run the code by passing a C Flag to the C Compiler (path to raylib.nelua) `nelua main.nelua` adjust it accordingly to where you have saved raylib.nelua 

**2.** Advised to use [.neluacfg.lua as seen here as example.](https://github.com/edubart/nelua-lang/discussions/67) for easier running & building of your project.

## Web export via Emscripten
Exporting your Raylib-Nelua game/App is now possible with Emscripten! There are few important bits to take notes on before attempting.

**1.** Emscripten must be installed on your system.

**2.** You have to provide your own Raylib 4.5-dev (build from source) libraylib.a and include files for web export. [Please read here](https://github.com/raysan5/raylib/wiki/Working-for-Web-(HTML5)).

**3.** Your library and include files have to be placed in a folder "libs"/"include" respectively. You can, however change those settings in raylib.nelua.

**4.** When compiling down your code to wasm you have to pass `rl.wasmSetMainLoop(UpdateFunctionHere, 0, 1)` which under the hood is technically just emscripten_set_main_loop.

Those are highly recommended settings to get started, but [I encourage you to read about the emscripten_set_main_loop function.]((https://emscripten.org/docs/api_reference/emscripten.h.html#c.emscripten_set_main_loop))

**4.5** Example code when exporting to web:
```lua
require "raylib"
require 'allocators.gc'
## if PLATFORM_WEB then
   collectgarbage('stop') -- conservative GCs cannot run automatically with emscripten
## end

-- Initialization
--------------------------------------------------------------------------------------
local SCREEN_WIDTH: uint16 <comptime> = 800
local SCREEN_HEIGHT: uint16 <comptime> = 450

## if not PLATFORM_WEB then
   rl.setConfigFlags(rl.configFlags.FLAG_VSYNC_HINT) -- Enable VSYNC if we're building for Desktop
## end

rl.initWindow(SCREEN_WIDTH,SCREEN_HEIGHT, "raylib-nelua [core] example - basic window")

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

**5.** Build your game using the following command `sudo nelua -V --cc emcc -b -o build/index ./main.nelua`

**6.** Enjoy!

## TODO
- [0] Create easings.h binding.
- [0] Create raygui.h binding.
- [0] Move emscripten cflags out of raylib.nelua and encourage user specific settings.
- [0] Document the API.

... Possibly more.

## Development

Further plans to incorporate easings.h raymath.h and raygui.h is planned to bring a complete Raylib experience to Nelua.

There is a good chance that certain functions are missing; report them if you do!
They will be included right away.

Contributions are encouraged. 

## Contributing

1. Fork it (<https://github.com/your-github-user/Raylib-Nelua/fork>)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

If you plan to contribute please make sure you follow the same coding convention to match overall style of this binding (camelCases)

## Contributors

- [Kenta](https://github.com/Its-Kenta) - Creator and maintainer

## Credits

- [Andre-LA](https://github.com/Andre-LA/) - Their old raylib binding that has been used as example & Raylib-Nelua logo.
- [AbdulKalam21](https://github.com/AbdulKalam21) - Using their old raylib binding as a template.

 <a href="https://nelua.io/"><img style="vertical-align:middle" src=https://i.imgur.com/QMUU0ed.png></a>
# Raylib-Nelua
[Nelua](https://nelua.io/) binding for [Raylib](http://www.raylib.com/) a simple and easy-to-use library to learn videogames programming.

*Using Raylib Version 4.5-dev*

## Supported Platforms
- Windows
- MacOS
- Linux
- Web? (Has not been tested yet.)

## Covered APIs
- [X] raylib.h
- [0] raymath.h - Planned
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

**1.** Run the code by passing a C Flag to the C Compiler (path to raylib.nelua) `nelua main.nelua --cflags="-L ."` adjust it accordingly to where you have saved raylib.nelua 

**2.** Advised to use [.neluacfg.lua as seen here as example.](https://github.com/edubart/nelua-lang/discussions/67) for easier running & building of your project.

## Development

Further plans to incorporate easings.h raymath.h and raygui.h is planned to bring a complete Raylib experience to Nelua.

Contributions are more than welcome.

## Contributing

1. Fork it (<https://github.com/your-github-user/gimbap/fork>)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Contributors

- [Kenta](https://github.com/Its-Kenta) - Creator and maintainer

## Credits

- [Andre-LA](https://github.com/Andre-LA/) - Their old raylib binding that has been used as example & Raylib-Nelua logo.
- [AbdulKalam21](https://github.com/AbdulKalam21) - Using their old raylib binding as a template.
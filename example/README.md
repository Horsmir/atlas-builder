# Atlas builder example: Using the Atlas with raylib and playing animations

This is an example of how to use the atlas builder. This example uses Raylib. It draws graphics from the atlas and also shows how to play animations that live in the atlas. These animations come from aseprite files that have multiple frames.

## The important files

- `main.odin`: Loads atlas and implements a tiny example game in raylib. Displays textures from the atlas as well as renders text using the font inside the atlas.
- `atlas.png`: Generated when the atlas builder runs. Contains an atlas generated from the textures in `textures` folder as well as a font generated from `font.ttf`. Any file in `textures` starting with `tileset_` is treated as a tileset.
- `atlas.odin`: Generated when the atlas builder runs. Contains metadata about where in `atlas.png` you can find the different textures, animations and letters.
- `animation.odin`: Implements 2D animation based on data in `atlas.odin`. Used by `main.odin` to display the player's animation.

## Overview

When you run `build.bat` or `build.sh` then this happens:

First `odin run ..` is executed. This will run the stuff in the parent directory, i.e. the atlas builder. The atlas builder will look inside the current directory for a `textures` folder and a `font.ttf`. From those two things it builds `atlas.png` and `atlas.odin`.

`atlas.png` contains the atlas generated from the the textures and the font. Everything in `atlas.png` is packed in an efficient manner to save texture space.

`atlas.odin` says where in `atlas.png` everything ended up. It is meant to be compiled as part of your game. Inside `alas.odin` you'll find lists of textures, animations and font glyphs and where they are within `atlas.png`.

After the atlas building is done, the build script then runs `odin build .`, which builds the stuff in this directory, i.e. builds the example that uses the atlas.

`main.odin` will open a window using raylib. It will load `atlas.png`. `atlas.odin` is within this directory and therefore compiled along with it. `main.odin` will use the info in `atlas.odin` to locate the textures it needs within `atlas.png` and draw them. Since they can all be drawn using the same texture, the number of draw calls is very low.

`main.odin` also uses `animation.odin` to play the player's animation, that also lives within the atlas.

`main.odin` draws text twice: Once using the same camera as the game is drawn with, and once using a "UI" camera.

In the end the game only uses two draw calls: One for the player animation, background art and background text and one for the UI text.
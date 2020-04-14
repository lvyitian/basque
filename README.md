# Basque

Basque is a top-down 2d game engine.

---

![Basque demo](assets/images/basque_demo.gif 'Basque demo')
![Basque demo 2](assets/images/basque_demo_2.gif 'Basque demo 2')

---

Binaries for all platforms are available under the GitHub Action here:

[![Compiling ...](https://github.com/ryanpcmcquen/basque/workflows/Compiling%20.../badge.svg)](https://github.com/ryanpcmcquen/basque/actions)

---

Too lazy to do local setup? You can give it a spin using Gitpod (no sound or key repeats though)!

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/ryanpcmcquen/basque)

---

#### Global keyboard shortcuts:

<kbd>↑</kbd>: Move player North

<kbd>→</kbd>: Move player East

<kbd>↓</kbd>: Move player South

<kbd>←</kbd>: Move player West

<kbd>q</kbd>: Quit

<kbd>e</kbd>: Toggle edit mode

#### Edit mode shortcuts:

<kbd>l</kbd>: Toggle map library

<kbd>Mouse button 1</kbd> (left click): Place tile

<kbd>Mouse button 2</kbd> (right click): Select tile

---

### Why do this?

Why not just use Godot/Unity/et cetera? Basque has a very different priority list than these engines. It is _not_ a generic engine. There isn't much here, but it is a good starting point if you are looking to roll your own engine, here is what it does:

-   Compiles cross platform (Linux/Mac/Windows, should work on BSD or Haiku as well).
-   Visual map editing (with an easy to understand plain text format).
-   Spritesheet animation.
-   Background music.
-   Scrolling.
-   Collision detection.
-   Some frame rate limiting.
-   Accepts command line arguments (currently only for enabling fullscreen mode).
-   Resolution detection (estimates window size to 80% of the screen or uses the full resolution for fullscreen mode, this can be switched off).
-   High DPI support (this still needs to be tested on all 3 platforms).

Everything that is here should not be considered a final or optimal solution, but rather, 'a good start'. At this point the engine will start becoming more specific to the game I am building, which is why I see this as the best time to open source it. Hopefully, it can be useful to others, either as a starting point, or as a guide of how to do some things with SDL2. Note that all of the code here is not necessarily the best solution to these problems, but it is _a_ solution.

---

### Map editing/layouts/attributes:

The current map system utilizes three main files:

-   ![map_attributes.txt](assets/data/map_attributes.txt)
-   ![map_layout.txt](assets/data/map_layout.txt)
-   ![map_library.txt](assets/data/map_library.txt)

While editing the map, you may use the visual map editor (available in `DEBUG_MODE` by pressing <kbd>e</kbd>) or edit the text files directly. Basque checks the timestamps of the files, and will reload them if they change, you do not need to recompile or relaunch Basque to do this. `map_layout.txt` also has suffixed versions created, with the intention of supporting future redo/undo functionality. The current number of versions that are kept is 100. This is of course, configurable.

`map_attributes.txt` follows the following format (all items are integers):

```
TILE_INDEX:
{ X_CLIP, Y_CLIP }, { NORTH_BORDER, EAST_BORDER, SOUTH_BORDER, WEST_BORDER },
```

Comments preceded with `//` in the attributes file are allowed. The other file types do not allow comments.

`map_layout.txt` and `map_library.txt` are comma separated integers, representing the tile indices. Newlines represent new rows.

For example:

```
  0,  1,  2,  3,  4,  1,  5,  _
 27, 16, 16, 16, 16, 16, 17,  _
 27, 16, 16, 16, 16, 16, 17,  _
 27, 16, 28, 16, 16, 16, 17,  _
```

The visual map editor (available in `DEBUG_MODE` by pressing <kbd>e</kbd>) automatically writes the indices with even spacing. `_` is a shortcut for a blank tile (the index for which is configurable under `configuration.h`).

`map_library.txt` is intended to be a resource for picking tiles (available in `DEBUG_MODE -> EDIT_MODE` by pressing <kbd>l</kbd>), while editing the world map (`map_layout.txt`). It has a few special properties. It will not show the player, and it always appears starting at the top left corner of the screen. It is also _not_ editable by the map editor.

---

### Basque currently requires:

-   SDL2
-   SDL2_image
-   SDL2_mixer
-   SDL2_ttf

---

### Getting SDL2 installed:

#### Linux:

##### apt:

```
sudo apt-get install libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev
```

##### dnf:

```
sudo dnf install SDL2-devel SDL2_image-devel SDL2_mixer-devel SDL2_ttf-devel
```

##### eopkg:

```
sudo eopkg install sdl2-devel sdl2-image-devel sdl2-mixer-devel sdl2-ttf-devel
```

Or whatever the equivalent package is for your distro.

#### Mac:

```
brew install sdl2 sdl2_image sdl2_mixer sdl2_ttf
```

#### Windows:

Run the script here: [install_sdl_on_windows.cmd](install_sdl_on_windows.cmd)

---

Or follow one of the below procedures:

##### Cmd or Powershell:

```
nuget install sdl2.nuget -NonInteractive -ExcludeVersion -OutputDirectory C:\INCLUDE\
nuget install sdl2_image.nuget -NonInteractive -ExcludeVersion -OutputDirectory C:\INCLUDE\
nuget install sdl2_mixer.nuget -NonInteractive -ExcludeVersion -OutputDirectory C:\INCLUDE\
nuget install sdl2_ttf.nuget -NonInteractive -ExcludeVersion -OutputDirectory C:\INCLUDE\
```

```
mkdir C:\INCLUDE\SDL2\

:: Headers:
robocopy C:\INCLUDE\sdl2.nuget\build\native\include\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_image.nuget\build\native\include\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_mixer.nuget\build\native\include\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_ttf.nuget\build\native\include\ C:\INCLUDE\SDL2\

:: Libs:
robocopy C:\INCLUDE\sdl2.nuget\build\native\lib\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_image.nuget\build\native\lib\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_mixer.nuget\build\native\lib\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_ttf.nuget\build\native\lib\x64\dynamic\ C:\INCLUDE\SDL2\

:: DLLs:
robocopy C:\INCLUDE\sdl2.nuget.redist\build\native\bin\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_image.nuget.redist\build\native\bin\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_mixer.nuget.redist\build\native\bin\x64\dynamic\ C:\INCLUDE\SDL2\
robocopy C:\INCLUDE\sdl2_ttf.nuget.redist\build\native\bin\x64\dynamic\ C:\INCLUDE\SDL2\
```

#### Cmd or Powershell as admin:

```
:: System DLLs:
robocopy C:\INCLUDE\sdl2.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\System32\ *.dll
robocopy C:\INCLUDE\sdl2.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\SysWOW64\ *.dll

robocopy C:\INCLUDE\sdl2_image.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\System32\ *.dll
robocopy C:\INCLUDE\sdl2_image.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\SysWOW64\ *.dll

robocopy C:\INCLUDE\sdl2_mixer.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\System32\ *.dll
robocopy C:\INCLUDE\sdl2_mixer.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\SysWOW64\ *.dll

robocopy C:\INCLUDE\sdl2_ttf.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\System32\ *.dll
robocopy C:\INCLUDE\sdl2_ttf.nuget.redist\build\native\bin\x64\dynamic\ C:\Windows\SysWOW64\ *.dll
```

##### Old school:

1. Download the latest VC development files from: https://libsdl.org

2. Place the entire contents of `include` and `lib` under `C:\INCLUDE\SDL2`.

3. Copy all DLLs under `lib` to `C:\Windows\System32` and `C:\Windows\SysWOW64`.

4. Repeat for _SDL2_image_, _SDL2_mixer_, and _SDL2_ttf_.

5. Profit.

---

### Compiling:

#### Linux & Mac:

```
make
```

#### Windows:

```
nmake
```

---

### Running:

After [compiling](#compiling) for your platform, you can launch with `./basque` (or `basque.exe` on Windows). Passing `-f` or `--fullscreen` will launch the app in fullscreen, rather than the resolution specified in [configuration.h](source/configuration.h).

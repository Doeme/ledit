# Ledit
Very simple GPU Rendered text editor without any bullshit.  
With keybinds inspired by emacs.  
Ledit runs on all 3 major operating systems: GNU/linux, macOS and Windows.
if you just want to test the editor then [download it from the releases](https://github.com/liz3/ledit/releases/latest)

## Motivation
The base motivation was just simply that i wanted to have a look into OpenGL and doing GPU accelerated things, i did not plan to create a text editor from the get go.  
After starting to experiment a bit i decided to call this a small side Project and implement a proper editor.

![image](https://github.com/liz3/ledit/blob/master/assets/screenshot.png?raw=true)
![image](https://github.com/liz3/ledit/blob/master/assets/screenshot2.png?raw=true)
![image](https://github.com/liz3/ledit/blob/master/assets/screenshot3.png?raw=true)

## Encodings
Ledit will work correctly with ascii and 2 byte unicode/UTF-8, 3-4 byte unicode characters will not work.

## Building
To build ledit you only need [CMake](https://cmake.org/) and a C/C++ compiler which supports C++17.
ledit requires GLFW and freetype2 but these are included in the [third-party](/third-party) folder.

### GNU/Linux & MacOS
To get the prerequisites run:
```sh
./init.sh
```
Then you can build the executable with
```
./build_release.sh
```
For debug builds use `build_debug.sh`

### Windows
Ledit builds with MSVC and does not require a unix betewen layer like Cgywin.
You will need at least a recent version of the windows MSVC C++ compiler, its better to install entire visual studio for the sake of the case that vs installer might leave out needed components.
then run in a CMD in the folder:
```
git submodule update --init
mkdir Release
cd Release
cmake ..
cmake --build . --config Release
```

## Structure
```
- src/main.cc: main rendering logic and keyboard callbacks.
- src/state.h: logic for controlling and state point.
- src/cursor.h: this is the most important file besides main, it manages the text state, what to render and where. and implements all logic components for manipulation.
- src/shader.h: manages shader loading.
- src/font_atlas.h: font atlas and width calculation.
- src/shaders.h: inlined shaders.
- src/highlighting.h: simple highlighting engine.
- src/languages.h: contains modes for certain languages for highlighting.
- src/provider.h: This contains the config parser and providers for folder autocomplete and other related things.
- src/selection.h: Small structure to keep track of selection state.
- src/la.(cc/h): Vectors implementation for coords and RGBA colors.
- third-party: ledit dependencies
```
There are more but these are self explaining.
## Config
ledit can have a config in your home directory `~/.ledit/config.json`.  
The following values can be set(without the comments)  
For the colors there are default values, for the font face either remove it completely or make sure its a valid path.
```¯
{
  "colors": {
    "comment_color": [
      127, 127, 127, 127 // Comment color if a active mode is present, in RGBA (0-255)
    ],
    "default_color": [
      242, 242, 242, 242 // Default color for text, in RGBA (0-255)
    ],
    "keyword_color": [
      153, 25, 51, 255 // Keyword color if a active mode is present, in RGBA (0-255)
    ],
    "special_color": [
      51, 51, 204, 255 // Special words color if a active mode is present, in RGBA (0-255)
    ],
    "string_color": [
      51, 153, 102, 255 // String literak color if a active mode is present, in RGBA (0-255)
    ],
   "background_color": [
     0, 0, 0, 255   // Editor background color RGBA (0-255)
    ]
   "selection_color": [
     0, 0, 0, 255   // Selection area color RGBA (0-255)
    ]
   "highlight_color": [
     0, 0, 0, 255   // Color of the active line background highlight. RGBA (0-255)
    ]
   "number_color": [
     0, 0, 160, 255   // Color used for numbers. RGBA (0-255)
    ]
   "line_number_color": [
     0, 0, 160, 255   // Color used for line numbers RGBA (0-255)
    ]
   "status_color": [
     0, 0, 160, 255   // Color used for the status line, buffer information RGBA (0-255)
    ]
   "minibuffer_color": [
     0, 0, 160, 255   // Color used for the Minibuffer, actions like search, replace, save new and so on. RGBA (0-255)
    ]
  },
  "window_transparency": true // if the window is allowed to be transparent
  "font_face": "/Users/liz3/Library/Fonts/FiraCode-Regular.ttf" // TTF font face path
}
```
## Info
Here are some infos.
### Standard input & output
Ledit can work with stdin/out by passing `-` as file name, **NOTE**: saving will print once then exit!
### Keybinds
C stands for CTRL, M for alt/meta.
```
ESC:
Escape serves two purposes and behaves a bit like VIM.
If you are currently in a minibuffer(command action) that cancels that action, escape again closes the ledit instance.
Otherwise this instantly closes the instance.

Navigation:
C-a - jump to line start.
C-e - jump to line end.
C-f - advance one character.
C-b - reverse one character.
C-p - go up one line.
C-n - go down one line
M-f - jump forward by one word.
M-b - jump backwards by one word.

More generalised Navigation:
C-x-a - jump to file start.
C-x-e - jump to to the last line in the file.
C-x-g - asks for a line number to jump to.

Search:
C-s will prompt for input and with enter its then possible to search that term case sensitive!

Manipulation:

C-d - delete character after the cursor
M-d - delete the next word including a whitespace.
C-shift-p - move current line up
C-shift-n - move current line down
C-w - cut selection
C-r - replace, first asks for search then for replace\, use SHFT-RET to replace all matches.  
C-x-/ - If a mode is active either comment or uncomment the cursor line or the selected lines, does not work for raw text mode.

Operations:
C-x-s - Save to last path, if no path present, ledit will ask for a path.
C-x-n - Save to new location, note that this will not overwrite the default save path, to overwrite the default path, save then load.
C-x-o - Load new file, this will replace the current file, non existing files will still load but be marked as New Files.
C-x-k - switch between open files(buffers) in the session,
Note: pressing this again will rotate through files that where open.

C-z - Undo.
M-w/C-c - Copy
C-y/C-v - Paste

C-space - Toggles selection mode on and off.

Misc:
C-x-l - Toggle Line numbers.
C-+   - Increase font size
C--(-)- decrease font size
C-x-0 - Load new font file, note that doing this will persist it in the config.
C-x-h - Toggle highlighting of the active line.
C-x-m - Switch active mode for current buffer.
C-x-w - close current buffer if its not the only one, otherwise use ESC.

```
# LICENSE
Ledit is free software following [GPL 2.0](/LICENSE).
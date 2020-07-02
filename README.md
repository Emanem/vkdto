# vkdto
Vulkan Dynamic Text Overlay - Heavily based on mesa overlay

## Status
This software is highly experimental and not ready for any use apart development.

## How to use
Compile the shared object, then copy `vkdto.x86_64.json` in `/usr/share/vulkan/implicit_layer.d/`. Once done, execute your application with `VKDTO_HUD=1` and `VKDTO_FILE=<update file here>` to start the overlay have it working.

## How does vkdto work
_vkdto_ works by constantly loading a `wchar_t` encoded file (specified with `VKDTO_FILE=<update file here>`) and displaying it's content using a (idally _monospace_) system font (it tries to load _Ubuntu_ font currently).
Once the `wchar_t` file is fully loaded, it will display its content via _Vulkan_ overlay (using [ImGui](https://github.com/ocornut/imgui)) on the top-left of the screen.

Currently there are no options to move the text overlay, specify the font size, file refresh rate and so fort. The code is indeed in alpha state; having mentioned this, most likely these options are going to be added in future.

_vkdto_ also supports some _meta characters_ to define some attributes such as text color, inverted text color/background and bold font; see the file [hashtext_fmt.h](https://github.com/Emanem/vkdto/blob/master/src/hashtext_fmt.h) for the binary definitions. For example, to print some text green, the meta characters should be used as follows:
```
Some normal text here <GREEN_ON>my green text<GREEN_OFF> some other text...
```
And all of this should be encoded as a `wchar_t`.

Furthermore, the `#` character is the _escape_ charcater, hence when needing to print such character, one should write two of those. As example:
```
This is a hash: ##
```
Will result in: _This is a hash #_

Another important constraint is that _vkdto_ required `sizeof(uint32_t) == sizeof(wchar_t)` hence should work ok on Linux, alas not on Windows.

### vkdto options
It is possible to specify further options (such as overlay position, font size, ...). In order to do so, one has to export the environment variable `VKDTO_OPT=(options string)` where `(options string)` is defined as follows:
```
<param>=<value>:<param>=<value>:...
```
An example could be `VKDTO_OPT="pos=tc:font_size=15.6". Paramaters/values can be as follows:

* *pos* - overlay position</br>Can have following values _tl_, _tc_, _tr_ for top-left, top-center and top-right and also _bl_, _bc_, _br_ for the same but bottom
* *font_size* - font size</br>A floating point value can be specified for the font size in pixels

### vkdto debug log
A debug log mechanism has been added; for now a very small number of functions/logic is traced; in order to enable this debug log, export the environment variable `VKDTO_DEBUG_LOG=<log filename>`.

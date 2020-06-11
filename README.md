# vkdto
Vulkan Dynamic Text Overlay - Heavily based on mesa overlay

## Status
This software is highly experimental and not ready for any use apart development.

## How to use
Compile the shared object, then copy `vkdto.x86_64.json` in `/usr/share/vulkan/implicit_layer.d/`. Once done, execute your application with `VKDTO_HUD=1` and `VKDTO_FILE=<update file here>` to start the overlay have it working.

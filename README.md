# RBXDecomp/RBLX-Launcher

[Discord](https://discord.gg/2JcBQFxVPv) | [Contributing](/CONTRIBUTING.md)

I don't want to make a fork

This is an X64 build of RBX decomp

use VS2022

This is a decompilation of Roblox as it was released in December 2007. We're writing the source back until Visual C++ compiles it into the same binaries, instruction for instruction, and the end goal is a codebase we can build on.

We follow the conventions and tooling of the [LEGO Racers decompilation](https://github.com/isledecomp/racers).

Keep in mind that this code reproduces the original release as it was, bugs included. It only builds for 32-bit Windows. If you're after a modern, cross-platform codebase, see [OpenRBX](https://github.com/OpenRBX-org/OpenRBX).

Both `WebService.dll` from RBXGS, the game server, and the client, `Roblox.exe`, are functionally complete. Work still continues to match the remaining functions.

## Building

You will need:

use VS2022

cd to the folder and type...

```
cmake -B build64 -G "Visual Studio 17 2022" -A x64

```

This gives you `build\WebService.dll` and `build\Roblox.exe`. Stick with `RelWithDebInfo`, comparing your build against the original needs the debug symbols it produces.

## Project Structure

* `WebService/` - the translation units specific to RBXGS
* `Client/` - the engine's static libraries and client project
* `3rdparty/` - third-party SDKs the original built against from outside its tree
* `util/` - headers the decompilation needs
* `cmake/` - CMake modules
* `tools/` - Python tooling and its requirements
* `reccmp/` - data files for reccmp
* `assets/` - icons for the progress report



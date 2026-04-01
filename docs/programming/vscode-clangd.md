# Problems with clangd in vscode

## Included files not found

Try `set(CMAKE_EXPORT_COMPILE_COMMANDS ON)` in your **CMakeLists.txt** <br>
Then reconfigure with CMake


## STD headers not found when cross compiling with devkitpro

I'm cross compiling my c++ project to the Nintendo DS with CMake but clangd won't find all the standard header files. <br>
Creating the following **settings.json** in **.vscode** solved my problem. <br>
**Replace `<Path/to/devkitpro>` with your actual devkitpro installation**

``` json
{
    "clangd.arguments": [
        "--query-driver=<Path/to/devkitpro>/devkitARM/bin/arm-none-eabi*",
        "--compile-commands-dir=${workspaceFolder}/build"
    ]
}
```
# 🏭 OpenGL-IMGui-Docking-CMake-Template

![Alt Text](resources/images/IMGui-CMake.png)


A quick and easy way to get started using OpenGL 3.3 Core Profile together with [imgui](https://github.com/ocornut/imgui) (Docking Branch) in a CMake environment.

This template only contains a wrapper class for shaders, and one for the window with appropriate `Initialize/LoadContent/Update/Render` as a game loop.

The shader class allows for **hot-reloading** of the fragment shader, so whenever you modify the `testing.fs` file in the `build/resources/shaders/` directory, the shader will automagically be reloaded! 

**NOTE:** The shader files in `resources/shaders/` are copied into the `build/` directory upon build, so if you want to save your hot-reloaded changes, then you must also modify the shaders in `resources/shaders/` directory.

## Getting started

1. Create a new repo by clicking `Use this template` up to the right!
2. Download some [GLFW pre-compiled binaries](https://www.glfw.org/download) and put the `libglfw3.a or libglfw3.lib` file in `libs/glfw/`. 
3. Compile with CMake and then run it! Piece of cake!

## Remarks

* Uses [imgui version 1.88](https://github.com/ocornut/imgui/releases/tag/v1.88)
* Works with Visual Studio CMake tools, Version 17 (VS 2022), and CLion
* Has been tested on MingW64 compiler for Windows (so it may require some fixing for it to work for gcc or clang)


---

## (Original IMGui Readme...)

Dear ImGui
=====

<center><b><i>"Give someone state and they'll have a bug one day, but teach them how to represent state in two separate locations that have to be kept in sync and they'll have bugs for a lifetime."</i></b></center> <a href="https://twitter.com/rygorous/status/1507178315886444544">-ryg</a>

----

[![Build Status](https://github.com/ocornut/imgui/workflows/build/badge.svg)](https://github.com/ocornut/imgui/actions?workflow=build) [![Static Analysis Status](https://github.com/ocornut/imgui/workflows/static-analysis/badge.svg)](https://github.com/ocornut/imgui/actions?workflow=static-analysis)

<sub>(This library is available under a free and permissive license, but needs financial support to sustain its continued improvements. In addition to maintenance and stability there are many desirable features yet to be added. If your company is using Dear ImGui, please consider reaching out.)</sub>

Businesses: support continued development and maintenance via invoiced sponsoring/support contracts:
<br>&nbsp;&nbsp;_E-mail: contact @ dearimgui dot com_
<br>Individuals: support continued development and maintenance [here](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=WGHNC6MBFLZ2S). Also see [Sponsors](https://github.com/ocornut/imgui/wiki/Sponsors) page.

| [The Pitch](#the-pitch) - [Usage](#usage) - [How it works](#how-it-works) - [Releases & Changelogs](#releases--changelogs) - [Demo](#demo) - [Integration](#integration) |
:----------------------------------------------------------: |
| [Gallery](#gallery) - [Support, FAQ](#support-frequently-asked-questions-faq) -  [How to help](#how-to-help) - [Sponsors](https://github.com/ocornut/imgui/wiki/Sponsors) - [Credits](#credits) - [License](#license) |
| [Wiki](https://github.com/ocornut/imgui/wiki) - [Languages & frameworks backends/bindings](https://github.com/ocornut/imgui/wiki/Bindings) - [Software using Dear ImGui](https://github.com/ocornut/imgui/wiki/Software-using-dear-imgui) - [User quotes](https://github.com/ocornut/imgui/wiki/Quotes) |

### The Pitch

Dear ImGui is a **bloat-free graphical user interface library for C++**. It outputs optimized vertex buffers that you can render anytime in your 3D-pipeline-enabled application. It is fast, portable, renderer agnostic, and self-contained (no external dependencies).

Dear ImGui is designed to **enable fast iterations** and to **empower programmers** to create **content creation tools and visualization / debug tools** (as opposed to UI for the average end-user). It favors simplicity and productivity toward this goal and lacks certain features commonly found in more high-level libraries.

Dear ImGui is particularly suited to integration in game engines (for tooling), real-time 3D applications, fullscreen applications, embedded applications, or any applications on console platforms where operating system features are non-standard.

- Minimize state synchronization.
- Minimize state storage on user side.
- Minimize setup and maintenance.
- Easy to use to create dynamic UI which are the reflection of a dynamic data set.
- Easy to use to create code-driven and data-driven tools.
- Easy to use to create ad hoc short-lived tools and long-lived, more elaborate tools.
- Easy to hack and improve.
- Portable, minimize dependencies, run on target (consoles, phones, etc.).
- Efficient runtime and memory consumption.
- Battle-tested, used by [many major actors in the game industry](https://github.com/ocornut/imgui/wiki/Software-using-dear-imgui).

### Usage

**The core of Dear ImGui is self-contained within a few platform-agnostic files** which you can easily compile in your application/engine. They are all the files in the root folder of the repository (imgui*.cpp, imgui*.h). **No specific build process is required**. You can add the .cpp files into your existing project.

**Backends for a variety of graphics API and rendering platforms** are provided in the [backends/](https://github.com/ocornut/imgui/tree/master/backends) folder, along with example applications in the [examples/](https://github.com/ocornut/imgui/tree/master/examples) folder. See the [Integration](#integration) section of this document for details. You may also create your own backend. Anywhere where you can render textured triangles, you can render Dear ImGui.

After Dear ImGui is set up in your application, you can use it from \_anywhere\_ in your program loop:
```cpp
ImGui::Text("Hello, world %d", 123);
if (ImGui::Button("Save"))
    MySaveFunction();
ImGui::InputText("string", buf, IM_ARRAYSIZE(buf));
ImGui::SliderFloat("float", &f, 0.0f, 1.0f);
```
![sample code output (dark, segoeui font, freetype)](https://user-images.githubusercontent.com/8225057/191050833-b7ecf528-bfae-4a9f-ac1b-f3d83437a2f4.png)
![sample code output (light, segoeui font, freetype)](https://user-images.githubusercontent.com/8225057/191050838-8742efd4-504d-4334-a9a2-e756d15bc2ab.png)

```cpp
// Create a window called "My First Tool", with a menu bar.
ImGui::Begin("My First Tool", &my_tool_active, ImGuiWindowFlags_MenuBar);
if (ImGui::BeginMenuBar())
{
    if (ImGui::BeginMenu("File"))
    {
        if (ImGui::MenuItem("Open..", "Ctrl+O")) { /* Do stuff */ }
        if (ImGui::MenuItem("Save", "Ctrl+S"))   { /* Do stuff */ }
        if (ImGui::MenuItem("Close", "Ctrl+W"))  { my_tool_active = false; }
        ImGui::EndMenu();
    }
    ImGui::EndMenuBar();
}

// Edit a color stored as 4 floats
ImGui::ColorEdit4("Color", my_color);

// Generate samples and plot them
float samples[100];
for (int n = 0; n < 100; n++)
    samples[n] = sinf(n * 0.2f + ImGui::GetTime() * 1.5f);
ImGui::PlotLines("Samples", samples, 100);

// Display contents in a scrolling region
ImGui::TextColored(ImVec4(1,1,0,1), "Important Stuff");
ImGui::BeginChild("Scrolling");
for (int n = 0; n < 50; n++)
    ImGui::Text("%04d: Some text", n);
ImGui::EndChild();
ImGui::End();
```
![my_first_tool_v188](https://user-images.githubusercontent.com/8225057/191055698-690a5651-458f-4856-b5a9-e8cc95c543e2.gif)

Dear ImGui allows you to **create elaborate tools** as well as very short-lived ones. On the extreme side of short-livedness: using the Edit&Continue (hot code reload) feature of modern compilers you can add a few widgets to tweak variables while your application is running, and remove the code a minute later! Dear ImGui is not just for tweaking values. You can use it to trace a running algorithm by just emitting text commands. You can use it along with your own reflection data to browse your dataset live. You can use it to expose the internals of a subsystem in your engine, to create a logger, an inspection tool, a profiler, a debugger, an entire game-making editor/framework, etc.

### How it works

Check out the Wiki's [About the IMGUI paradigm](https://github.com/ocornut/imgui/wiki#about-the-imgui-paradigm) section if you want to understand the core principles behind the IMGUI paradigm. An IMGUI tries to minimize superfluous state duplication, state synchronization, and state retention from the user's point of view. It is less error-prone (less code and fewer bugs) than traditional retained-mode interfaces, and lends itself to creating dynamic user interfaces.

Dear ImGui outputs vertex buffers and command lists that you can easily render in your application. The number of draw calls and state changes required to render them is fairly small. Because Dear ImGui doesn't know or touch graphics state directly, you can call its functions  anywhere in your code (e.g. in the middle of a running algorithm, or in the middle of your own rendering process). Refer to the sample applications in the examples/ folder for instructions on how to integrate Dear ImGui with your existing codebase.

_A common misunderstanding is to mistake immediate mode GUI for immediate mode rendering, which usually implies hammering your driver/GPU with a bunch of inefficient draw calls and state changes as the GUI functions are called. This is NOT what Dear ImGui does. Dear ImGui outputs vertex buffers and a small list of draw calls batches. It never touches your GPU directly. The draw call batches are decently optimal and you can render them later, in your app or even remotely._

### Releases & Changelogs

See [Releases](https://github.com/ocornut/imgui/releases) page for decorated Changelogs.
Reading the changelogs is a good way to keep up to date with the things Dear ImGui has to offer, and maybe will give you ideas of some features that you've been ignoring until now!

### Demo

Calling the `ImGui::ShowDemoWindow()` function will create a demo window showcasing a variety of features and examples. The code is always available for reference in `imgui_demo.cpp`. [Here's how the demo looks](https://raw.githubusercontent.com/wiki/ocornut/imgui/web/v167/v167-misc.png).

You should be able to build the examples from sources. If you don't, let us know! If you want to have a quick look at some Dear ImGui features, you can download Windows binaries of the demo app here:
- [imgui-demo-binaries-20220504.zip](https://www.dearimgui.org/binaries/imgui-demo-binaries-20220504.zip) (Windows, 1.88 WIP, built 2022/05/04, master) or [older binaries](https://www.dearimgui.org/binaries).

The demo applications are not DPI aware so expect some blurriness on a 4K screen. For DPI awareness in your application, you can load/reload your font at a different scale and scale your style with `style.ScaleAllSizes()` (see [FAQ](https://www.dearimgui.org/faq)).

### Integration

On most platforms and when using C++, **you should be able to use a combination of the [imgui_impl_xxxx](https://github.com/ocornut/imgui/tree/master/backends) backends without modification** (e.g. `imgui_impl_win32.cpp` + `imgui_impl_dx11.cpp`). If your engine supports multiple platforms, consider using more imgui_impl_xxxx files instead of rewriting them: this will be less work for you, and you can get Dear ImGui running immediately. You can _later_ decide to rewrite a custom backend using your custom engine functions if you wish so.

Integrating Dear ImGui within your custom engine is a matter of 1) wiring mouse/keyboard/gamepad inputs 2) uploading a texture to your GPU/render engine 3) providing a render function that can bind textures and render textured triangles. The [examples/](https://github.com/ocornut/imgui/tree/master/examples) folder is populated with applications doing just that. If you are an experienced programmer at ease with those concepts, it should take you less than two hours to integrate Dear ImGui into your custom engine. **Make sure to spend time reading the [FAQ](https://www.dearimgui.org/faq), comments, and the examples applications!**

Officially maintained backends/bindings (in repository):
- Renderers: DirectX9, DirectX10, DirectX11, DirectX12, Metal, OpenGL/ES/ES2, SDL_Renderer, Vulkan, WebGPU.
- Platforms: GLFW, SDL2, Win32, Glut, OSX, Android.
- Frameworks: Allegro5, Emscripten.

[Third-party backends/bindings](https://github.com/ocornut/imgui/wiki/Bindings) wiki page:
- Languages: C, C# and: Beef, ChaiScript, Crystal, D, Go, Haskell, Haxe/hxcpp, Java, JavaScript, Julia, Kotlin, Lobster, Lua, Odin, Pascal, PureBasic, Python, Ruby, Rust, Swift...
- Frameworks: AGS/Adventure Game Studio, Amethyst, Blender, bsf, Cinder, Cocos2d-x, Diligent Engine, Flexium, GML/Game Maker Studio2, GLEQ, Godot, GTK3+OpenGL3, Irrlicht Engine, LÖVE+LUA, Magnum, Monogame, NanoRT, nCine, Nim Game Lib, Nintendo 3DS & Switch (homebrew), Ogre, openFrameworks, OSG/OpenSceneGraph, Orx, Photoshop, px_render, Qt/QtDirect3D, SDL_Renderer, SFML, Sokol, Unity, Unreal Engine 4, vtk, VulkanHpp, VulkanSceneGraph, Win32 GDI, WxWidgets.
- Note that C bindings ([cimgui](https://github.com/cimgui/cimgui)) are auto-generated, you can use its json/lua output to generate bindings for other languages.

[Useful Extensions/Widgets](https://github.com/ocornut/imgui/wiki/Useful-Extensions) wiki page:
- Text editors, node editors, timeline editors, plotting, software renderers, remote network access, memory editors, gizmos, etc.

Also see [Wiki](https://github.com/ocornut/imgui/wiki) for more links and ideas.

### Gallery

For more user-submitted screenshots of projects using Dear ImGui, check out the [Gallery Threads](https://github.com/ocornut/imgui/issues/5243)!

For a list of third-party widgets and extensions, check out the [Useful Extensions/Widgets](https://github.com/ocornut/imgui/wiki/Useful-Extensions) wiki page.

|  |  |
|--|--|
| Custom engine [erhe](https://github.com/tksuoran/erhe) (docking branch)<BR>[![erhe](https://user-images.githubusercontent.com/8225057/190203358-6988b846-0686-480e-8663-1311fbd18abd.jpg)](https://user-images.githubusercontent.com/994606/147875067-a848991e-2ad2-4fd3-bf71-4aeb8a547bcf.png) | Custom engine for [Wonder Boy: The Dragon's Trap](http://www.TheDragonsTrap.com) (2017)<BR>[![the dragon's trap](https://user-images.githubusercontent.com/8225057/190203379-57fcb80e-4aec-4fec-959e-17ddd3cd71e5.jpg)](https://cloud.githubusercontent.com/assets/8225057/20628927/33e14cac-b329-11e6-80f6-9524e93b048a.png) |
| Custom engine (untitled)<BR>[![editor white](https://user-images.githubusercontent.com/8225057/190203393-c5ac9f22-b900-4d1e-bfeb-6027c63e3d92.jpg)](https://raw.githubusercontent.com/wiki/ocornut/imgui/web/v160/editor_white.png) | Tracy Profiler ([github](https://github.com/wolfpld/tracy))<BR>[![tracy profiler](https://user-images.githubusercontent.com/8225057/190203401-7b595f6e-607c-44d3-97ea-4c2673244dfb.jpg)](https://raw.githubusercontent.com/wiki/ocornut/imgui/web/v176/tracy_profiler.png) |

### Support, Frequently Asked Questions (FAQ)

See: [Frequently Asked Questions (FAQ)](https://github.com/ocornut/imgui/blob/master/docs/FAQ.md) where common questions are answered.

See: [Wiki](https://github.com/ocornut/imgui/wiki) for many links, references, articles.

See: [Articles about the IMGUI paradigm](https://github.com/ocornut/imgui/wiki#about-the-imgui-paradigm) to read/learn about the Immediate Mode GUI paradigm.

See: [Upcoming Changes](https://github.com/ocornut/imgui/wiki/Upcoming-Changes).

Getting started? For first-time users having issues compiling/linking/running or issues loading fonts, please use [GitHub Discussions](https://github.com/ocornut/imgui/discussions). For other questions, bug reports, requests, feedback, you may post on [GitHub Issues](https://github.com/ocornut/imgui/issues). Please read and fill the New Issue template carefully.

Private support is available for paying business customers (E-mail: _contact @ dearimgui dot com_).

**Which version should I get?**

We occasionally tag [Releases](https://github.com/ocornut/imgui/releases) (with nice releases notes) but it is generally safe and recommended to sync to master/latest. The library is fairly stable and regressions tend to be fixed fast when reported. Advanced users may want to use the `docking` branch with [Multi-Viewport](https://github.com/ocornut/imgui/issues/1542) and [Docking](https://github.com/ocornut/imgui/issues/2109) features. This branch is kept in sync with master regularly.

**Who uses Dear ImGui?**

See the [Quotes](https://github.com/ocornut/imgui/wiki/Quotes), [Sponsors](https://github.com/ocornut/imgui/wiki/Sponsors), and [Software using Dear ImGui](https://github.com/ocornut/imgui/wiki/Software-using-dear-imgui) Wiki pages for an idea of who is using Dear ImGui. Please add your game/software if you can! Also, see the [Gallery Threads](https://github.com/ocornut/imgui/issues/5243)!

How to help
-----------

**How can I help?**

- See [GitHub Forum/Issues](https://github.com/ocornut/imgui/issues) and [GitHub Discussions](https://github.com/ocornut/imgui/discussions).
- You may help with development and submit pull requests! Please understand that by submitting a PR you are also submitting a request for the maintainer to review your code and then take over its maintenance forever. PR should be crafted both in the interest of the end-users and also to ease the maintainer into understanding and accepting it.
- See [Help wanted](https://github.com/ocornut/imgui/wiki/Help-Wanted) on the [Wiki](https://github.com/ocornut/imgui/wiki/) for some more ideas.
- Have your company financially support this project with invoiced sponsoring/support contact (please reach out: contact at dearimgui dot com).

Sponsors
--------

Ongoing Dear ImGui development is and has been financially supported by users and private sponsors.
<BR>Please see the **[detailed list of current and past Dear ImGui supporters](https://github.com/ocornut/imgui/wiki/Sponsors)** for details.
<BR>From November 2014 to December 2019, ongoing development has also been financially supported by its users on Patreon and through individual donations.

**THANK YOU to all past and present supporters for helping to keep this project alive and thriving!**

Dear ImGui is using software and services provided free of charge for open source projects:
- [PVS-Studio](https://www.viva64.com/en/b/0570/) for static analysis.
- [GitHub actions](https://github.com/features/actions) for continuous integration systems.
- [OpenCppCoverage](https://github.com/OpenCppCoverage/OpenCppCoverage) for code coverage analysis.

Credits
-------

Developed by [Omar Cornut](https://www.miracleworld.net) and every direct or indirect [contributors](https://github.com/ocornut/imgui/graphs/contributors) to the GitHub. The early version of this library was developed with the support of [Media Molecule](https://www.mediamolecule.com) and first used internally on the game [Tearaway](https://tearaway.mediamolecule.com) (PS Vita).

Recurring contributors (2022): Omar Cornut [@ocornut](https://github.com/ocornut), Rokas Kupstys [@rokups](https://github.com/rokups) (a large portion of work on automation systems, regression tests and other features are currently unpublished).

Sponsoring, support contracts and other B2B transactions are hosted and handled by [Lizardcube](https://www.lizardcube.com).

Omar: "I first discovered the IMGUI paradigm at [Q-Games](https://www.q-games.com) where Atman Binstock had dropped his own simple implementation in the codebase, which I spent quite some time improving and thinking about. It turned out that Atman was exposed to the concept directly by working with Casey. When I moved to Media Molecule I rewrote a new library trying to overcome the flaws and limitations of the first one I've worked with. It became this library and since then I have spent an unreasonable amount of time iterating and improving it."

Embeds [ProggyClean.ttf](http://upperbounds.net) font by Tristan Grimmer (MIT license).
<br>Embeds [stb_textedit.h, stb_truetype.h, stb_rect_pack.h](https://github.com/nothings/stb/) by Sean Barrett (public domain).

Inspiration, feedback, and testing for early versions: Casey Muratori, Atman Binstock, Mikko Mononen, Emmanuel Briney, Stefan Kamoda, Anton Mikhailov, Matt Willis. Also thank you to everyone posting feedback, questions and patches on GitHub.

License
-------

Dear ImGui is licensed under the MIT License, see [LICENSE.txt](https://github.com/ocornut/imgui/blob/master/LICENSE.txt) for more information.

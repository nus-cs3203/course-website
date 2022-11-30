<br>
<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#introduction)Introduction
=============================

The Cross-platform Startup SPA Solution with CMake offers a starting point for your project development and testing.

In CS3203 we offer two alternatives for your project development:

1.  The easiest: use Visual Studio 2022/2019 as your IDE to develop in C++ under Windows.
2.  The not-so-easy: use CMake to develop in C++ cross-platform.

Please note that there is **no difference in grading** when choosing one of the two alternatives, as long as your README states clearly if we are to use a Windows machine or a MacOS/Linux machine for your final testing. The compilation and running process should be flawless no matter which alternative you choose.

_**This documentation is applicable to cross-platform development using CMake!**_

The files provided contain the AutoTester libraries needed and used for final testing in CS3203. Please make sure that you read the documentation about the AutoTester to be able to integrate your code with the AutoTester.

Download [spa-cp.zip](../../archive/spa-cp-2022-Jan-07.zip) and unzip it to your local directory.

[](#1-getting-started-installing-the-tools)1\. Getting Started: Installing the Tools
====================================================================================

Before adopting this Cross-platform Startup SPA Solution, please note that:

*   _**All members in the team have to use CMake (even if they develop on Windows, MacOS, or Linux)!**_ Do not mix using Windows Startup SPA Solution with using CMake in the same team!
    
*   Some basic knowledge of CMake is required. When working with CMake, you mainly control the building process via editing CMakeLists.txt file as opposed to using the IDE’s GUI to add new files and add dependencies.
    
*   In CMake, we use a unit testing framework called [Catch2](https://github.com/catchorg/Catch2). This is because CPPunit for unit testing in Visual Studio is not available in MacOS or Linux.
    
*   _**Team members who are using Windows may still use Visual Studio. However, those who are on MacOS or Linux need to choose a different IDE since Visual Studio for Mac/Linux does not support C++ by default.**_
    
*   _**Warning**_: We recommend that the entire team uses the same platform and coding environment if possible. While it is possible to develop cross-platform code which works the same on both platform, there are significant differences between the clang/gcc compiler toolchain and MSVC toolchain. If the whole team is not developing on the same platform, you might end up spending large amount of time ensuring your code works the same on both platform.
    

[](#11-installing-visual-studio-20222019)1.1. Installing Visual Studio 2022/2019
================================================================================

If you are developing in Windows, we recommend using Visual Studio Community 2022/2019.

Do refer to [Section 1.1 in the Documentation for Windows Startup SPA Solution](Windows-Startup-SPA-Solution#11-installing-visual-studio-2022-2019) on how to install Visual Studio 2022/2019.

CMake is also installed during the installation of Visual Studio 2022/2019.

[](#12-installing-clion)1.2 Installing CLion
============================================

If you are developing in MacOS, we recommend using [CLion](https://www.jetbrains.com/clion/).

*   CLion is free for educational use. NUS students can use their NUSnet emails to register a license.
*   While CLion is available in Windows, debugging is not supported by CLion. However, it's Intellisense may be better than Visual Studio when you use the Open Folder method.

On macOS, CLion will require Xcode. Please proceed with the installation.

You will also need to install CMake and configure CLion for using CMake.

[](#2-cross-platform-startup-spa-solution)2\. Cross-platform Startup SPA Solution
=================================================================================

Our solution is organized by into separate project folders and each contains its own `CMakeLists.txt` file. The root `CMakeLists.txt` control which one to build by using `add_subdirectory()` commands. Additional CMake scripts and utility functions are to be placed under `cmake` folder.

[](#21-content-of-cross-platform-startupspasolution)2.1. Content of Cross-platform StartupSPASolution
-----------------------------------------------------------------------------------------------------

Cross-platform Startup SPA Solution contains five folders (under src):

1.  _**spa**_: This folder contains the implementation for your SPA. When compiled, this folder produces a library that is used by all other projects.
    
    It is possible for dependent libraries' headers to be accessible to those projects that use them. Although there are many ways of achieving this such as `include_directories($SOMEPATH)` at root `CMakeLists.txt`, CMake provide a clean way to achieve this via `target_include_directories()` function. Please refer to `src/spa/CMakeLists.txt`.
    
2.  _**autotester**_: This folder contains routines to call the methods you have implemented under SPA.
    
    This library is found via CMake script under `cmake/FindAutotester.cmake`. After `find_package(Autotester)` is successful, CMake variable `${AUTOTESTER_LIBRARIES}` will be populated which you can then use in `target_link_libraries()` call to link against.
    
3.  _**unit\_testing**_: This folder will contain unit tests created for your SPA.
    
    We are using unit testing framework [Catch2](https://github.com/catchorg/Catch2). This is a header only library which provide macros for writing unit tests. After linking against the library (included under lib folder), unit\_testing (and integration\_testing) produces executables which run the unit tests. Since normally, an executable will require a main method to build successfully, we have added an empty `main.cpp` with definition `#define CATCH_CONFIG_MAIN`. This tells Catch2 to generate a main method in that file. Please ensure there's only one file with that definition.
    
    If you are using CLion, it has built in support for Catch2. To run the tests specified in the project:
    
    *   `Run > Edit Configurations > click the + sign at the top left corner` and select catch test.
    *   Give it a name (unit tests) and choose the unit\_testing in the target.
    *   When you run, CLion will provide a GUI for test report.
4.  _**integration\_testing**_: This folder will contain integration tests created for your SPA (using same unit testing framework as unit\_testing).
    

[](#22-working-with-cmake-in-visual-studio)2.2. Working with CMake in Visual Studio
-----------------------------------------------------------------------------------

There are two ways to work with CMake in Visual Studio.

*   _**Recommended**_: Use [Open Folder](https://blogs.msdn.microsoft.com/vcblog/2016/10/05/bring-your-c-codebase-to-visual-studio-with-open-folder/) mode without using .sln solution files.
    
    *   `File > Open > Folder > (navigate to the folder with CMakeLists.txt)`
        
    *   Visual Studio automatically generates the CMake cache and detects targets specified in `CMakeLists.txt`.
        
    *   Internally, Visual Studio uses ninja build system(which is configurable via `CMakeSettings.json`) to build the final binaries. You can control this process(such as where CMake cache folder is) via `CMakeSettings.json` and `.vs` folder. Project specific settings such as arguments to pass during debugging is stored in `.vs/launch.vs.json`. Our `CMakeSettings.json` configures a 32 bit build and the location of built binaries.
        
*   _**Not Recommended**_: Use `.sln` mode where you generate Visual Studio `.sln` file from CMake and open the `.sln` file normally. When you are making changes such as adding files or reorganizing code folder structure, you should edit the `CMakeLists.txt` and regenerate the `.sln` files, instead of relying on the Visual Studio.
    
    Once the folder is open, debugging works the same as Visual Studio `.sln` files. Just putting a breakpoint on the line you want to break and in debug mode it will pause at that line accordingly. No special configuration is required.
    

[](#23-working-with-cmake-in-clion)2.3. Working with CMake in CLion
-------------------------------------------------------------------

To open a project:

*   `File > Open > Folder > (navigate to the folder with CMakeLists.txt)`
    
*   CLion will then proceed to generate CMake cache after which you can build the project.(Ctrl+F9).
    

[](#24-working-on-macos-using-terminal)2.4. Working on macOS using Terminal
---------------------------------------------------------------------------

If you don't use CLion and you mainly work in terminal, you can follow the instructions below to build the project.

*   Install Homebrew: `$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
*   Install all the dependencies: `$ brew install cmake llvm`
*   Unzip the cmake project into your desired directory and change directory into that.
*   Create a new folder called build: `$ mkdir build`
*   Navigate into build: `$ cd build`
*   Execute cmake: `$ cmake ..`
*   Run make: `$ make -j4`
*   Navigate to the tests folder: `$ cd ../tests/`
*   Run the autotester in directory `test`, to check if you have everything setup correctly: `$ ../build/src/autotester/autotester Sample_source.txt Sample_queries.txt out.xml`

[](#25-working-on-fedora-linux-using-terminal)2.5. Working on Fedora (Linux) using Terminal
-------------------------------------------------------------------------------------------

If you are an open source fanatic and manage to convince your whole team to switch to Fedora, the instructions are pretty simple for Fedora using GCC:

*   Install all the dependencies: `# dnf install cmake make gcc-c++`
*   Unzip the cmake project into your desired directory and change directory into that.
*   Create a new folder called build: `$ mkdir build`
*   Navigate into build: `$ cd build`
*   Execute cmake: `$ cmake ..`
*   Run make: `$ make -j4`
*   Navigate to the tests folder: `$ cd ../tests/`
*   Run the autotester in directory `test`, to check if you have everything setup correctly: `$ ../build/src/autotester/autotester Sample_source.txt Sample_queries.txt out.xml`

[](#faq)FAQ
===========

1.  When I follow the instructions, I get this error: `Beginning to parse Simple Program. End of parsing Simple Program. terminate called after throwing an instance of 'std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >' Aborted (core dumped)`. What should I do?
    
    *   Check your program parameters. Make sure the 3 paths that you have input as parameters are valid on your system.
2.  My program runs, but the output does not look correct (i.e. files are not parsed correctly)
    
    *   Make sure your text files are using the correct [line endings](http://www.cs.toronto.edu/~krueger/csc209h/tut/line-endings.html) for your platform! You can convert line endings using linux utilities like dos2unix or unix2doc.
3.  I want to use Ubuntu, but I get errors.
    
    *   Note that Ubuntu is not a supported platform, but if you wish to use it for your development, you can try compiling using clang: use `CXX=clang++ CC=clang cmake ..` instead of just `cmake ..`
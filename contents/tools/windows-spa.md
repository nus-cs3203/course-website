<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#introduction)Introduction
=============================

The Windows Startup SPA Solution offers a starting point for your project development and testing.

In CS3203 we offer two alternatives for your project development:

1.  The easiest: use Visual Studio 2022/2019 as your IDE to develop in C++ under Windows.
2.  The not-so-easy: use CMake to develop in C++ cross-platform.

Please note that there is **no difference in grading** when choosing one of the two alternatives, as long as your README states clearly if we are to use a Windows machine or a MacOS/Linux machine for your final testing. The compilation and running process should be flawless no matter which alternative you choose.

**This documentation is applicable to development in VS2022/2019 on Windows!**

The files provided contain the AutoTester libraries needed and used for final testing in CS3203. Please make sure that you read the documentation about the AutoTester to be able to integrate your code with the AutoTester.

Download [spa-win.zip](../../archive/spa-win-2022-Jan-07.zip) and unzip it to your local directory.

[](#1-getting-started-installing-the-tools)1\. Getting Started: Installing the Tools
====================================================================================

[](#11-installing-visual-studio-20222019)1.1. Installing Visual Studio 2022/2019
--------------------------------------------------------------------------------

_**Before**_ attending the Tools Lab, you should download and install Visual Studio Community 2022/2019 via the [Microsoft Azure Education portal](https://portal.azure.com/#blade/Microsoft_Azure_Education/EducationMenuBlade/software).

When you start your installation you will be asked to choose what to install. You should choose at least the following packages:

*   Desktop Development with C++
*   Visual Studio extension development
*   Linux development with C++ (only if you plan to CMake using the not-so-easy alternative for project development)
*   .NET cross-platform development tools (only if you plan to CMake using the not-so-easy alternative for project development)

_**The installation might take more than one hour!**_

If you have other versions of Visual Studio 2019, such as Visual Studio Enterprise 2022/2019, it should work as well.

[](#2-visual-studio-20222019-startup-spa-solution)2\. Visual Studio 2022/2019 Startup SPA Solution
==================================================================================================

This sample solution StartupSPASolution.sln can be used as a start-up framework for your project development and testing. This solution helps you organize code and test-cases, and run AutoTester.

There are two configurations: Debug & Release. We present the solution file in the Debug configuration, and the Release configuration should be handled in a similar fashion.

*   _**Debug configuration**_ allows you to debug your code line by line, watch variable, put breakpoints, etc.
    
*   _**Release configuration**_ does not allow debug functionalities, and generates an optimized output (executable/ library) that runs faster than the Debug version. It is used only in the final stages of the development and testing (submission). You should ensure that your team's implementation work as intended in Release configuration.
    

Please choose Win32 as the platform for Visual Studio development, due to compatibility issues between AutoTester.

[](#21-content-of-startupspasolution)2.1 Content of StartupSPASolution
----------------------------------------------------------------------

StartupSPASolution.sln solution contains five projects:

1.  _**SPA**_: This is a static library project. This project contains the implementation for your SPA. Currently, only a few key headers and CPP files have been created and added to this project. The files belonging to project SPA are located under folder `StartupSPASolution\source`. You can add multiple folders/files with your source code.
    
    All the other four projects are dependent on this project. Hence, when building any of the other four projects, this project is automatically built. The output after successfully building this project is a static library `SPA.lib` located under `StartupSPASolution\Debug`.
    
2.  _**AutoTester**_: This is a Visual C++ Windows Console Application (without precompiled headers). This project integrates your SPA implementation with the AutoTester. Currently, only a few files are needed and added in this project. The files belonging to project AutoTester are located under folder `StartupSPASolution\AutoTester\source`.
    
    Follow the comments in `StartupSPASolution\AutoTester\source\TestWrapper.cpp` and the instructions given for AutoTester to fill in this file with the calls to your SPA (`SPA.lib`).
    
    The folder `StartupSPASolution\Tests` contains your SIMPLE code files and query files, in the format specified in the AutoTester documentation. This project depends on SPA project (`SPA.lib`) and on AutoTester library contained in `StartupSPASolution\lib`.
    
    The output after successfully building this project is an executable file, AutoTester.exe, located under `StartupSPASolution\Debug`.
    
3.  _**UnitTesting**_: This is a Visual C++ Native Unit Test project. In this project, you should add unit tests created for your SPA. Currently, only one CPP file has been created and added to this project. The files belonging to project UnitTesting are located under folder `StartupSPASolution\UnitTesting`.
    
    This project depends on SPA project (`SPA.lib`) and on Unit Tests headers. The output after successfully building this project is a dynamic library, `UnitTesting.dll`, located under `StartupSPASolution\Debug`. To run the tests, you need to navigate to `Tests → Run`.
    
4.  _**IntegrationTesting**_: This is a Visual C++ Native Unit Test project. In this project, you should put integration tests created for your SPA. Currently, only one CPP file has been created and added to this project. The files belonging to project IntegrationTesting are located under folder `StartupSPASolution\IntegrationTesting`.
    
    This project depends on SPA project (`SPA.lib`) and on Integration Tests headers. The output after successfully building this project is a dynamic library, `IntegrationTesting.dll`, located under `StartupSPASolution\Debug`. To run the tests, you need to navigate to `Tests → Run`.
    

[](#22-running-the-projects-in-startupspasolution)2.2. Running the Projects in StartupSPASolution
-------------------------------------------------------------------------------------------------

To run your projects from Visual Studio, first set your start-up project to either AutoTester (`Right-click on Autotester Project > Set as Startup Project`).

Note that only AutoTester project can be executed using the Run option from VS (green Run button). UnitTesting and IntegrationTesting have to be run from Tests menu, while SPA is a library without a main function (cannot run).

1.  Running the AutoTester from VS requires you to correctly set your command line arguments from AutoTester Project Properties -> Debugging -> Command Arguments.
    
2.  Running the AutoTester executable from command line is possible by opening a command prompt and typing AutoTester.exe from the Debug folder (where AutoTester.exe is located).
    

[](#23-potential-problems)2.3. Potential Problems
-------------------------------------------------

While the StartupSPASolution solution builds and links successfully, you may have problems in linking your projects and adding dependencies once you start modifying this solution. Here are a few things that you need to check in case you have linking errors (given for Debug configuration, while Release configuration is similar):

*   Solution properties: under “Project Dependencies” show the correct chain of dependencies among the projects in your solution.
    
*   Project properties: check the following:
    
    1.  _**Configuration Properties/General**_: ensure that the “Configuration Type” is either Static Library(.lib for SPA project), Application (.exe for AutoTester projects), or Dynamic Library (.dll for for UnitTesting and IntegrationTesting).
        
    2.  _**C/C++/Code Generation**_: ensure that the “Runtime Library” is “Multi-threaded Debug (/MDd)”. A linking error appears if this field is not set properly. You will not need to change this setting if you have linked the appropriate library.
        
    3.  _**C/C++ General**_: ensure that “Additional Include Directories” are correctly filled in. Fill in this field if you use external headers/files that are not defined in the project. Be sure to add the correct path to the external files. A building error appears if this field is not set properly.
        
    4.  _**Linker/Input**_: ensure that the “Additional Dependencies” contain the correct path and name to your libraries that you use in the project. Be sure to use the appropriate version of AutoTester for the correct build configuration (AutoTester\_debug.lib, AutoTester\_release.lib appropriately). A linking error (LNK2038) appears if the dependencies are not set correctly. Also, a linking warning regarding debug information might appear when the libraries are not built using Debug information (/MD instead of /MDd see point 2.). You may ignore this warning.
        
    5.  _**Linking warning and errors**_ regarding “machine type 'x64' conflicts with target machine type 'x86'”. The whole solution should use 32-bit target platform settings. In general, you cannot mix 32-bit and 64-bit projects.
        

[](#faq)FAQ
===========

1.  When I build the project, I get an error about cannot opening the lib file. How do I fix this?
    
    *   The AutoTester lib file is missing, download a fresh copy and check your `.gitignore` files.
2.  When I run AutoTester, I get "Could not find pdf. AutoTester.exe was built with /DEBUG:FASTLINK which requires object files for debugging." How do I fix this?
    
    *   Conflicts with the `.vs` directory is causing this. Make sure that `.vs` is ignored properly and not copied across machines.
        
    *   Avoid using file syncing software like Dropbox or OneDrive and use git.
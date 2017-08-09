# mbed SDK build script environment
## Introduction
Mbed test framework allows users to test their mbed devices’ applications, build mbed SDK library, re-run tests, run mbed SDK regression, add new tests and get all this results automatically. Everything is done on your machine so you have a full control over compilation, and tests you run.

It's is using Python 2.7 programming language to drive all tests so make sure Python 2.7 is installed on your system and included in your system PATH. To compile mbed SDK and tests you will need one or more supported compilers installed on your system.

To follow this short introduction you should already:
* Know what mbed SDK is in general.
* Know how to install Python 2.7, ARM target cross compilers.
* You have C/C++ programming experience and at least willingness to learn a bit about Python. 

## Test automation limitations
Note that for tests which require connected external peripherals, for example Ethernet, SD flash cards, external EEPROM tests, loops etc. you need to:

* Modify test source code to match components' pin names to actual mbed board pins where peripheral is connected or
* Wire your board the same way test defines it.

## Prerequisites
mbed test suite and build scripts are Python 2.7 applications and require Python 2.7 runtime environment and setuptools to install dependencies.

What we need:
* Installed Python 2.7 programming language.
* Installed setuptools
* Optionally you can install pip which is the PyPA recommended tool for installing Python packages from command line.

mbed SDK in its repo root directory specifies `setup.py` file which holds information about all packages which are dependencies for it. Bear in mind only few simple steps are required to install all dependencies.

First, clone mbed SDK repo and go to mbed SDk repo's directory.

Second, invoke `setup.py` so `setuptools` can install mbed SDK's dependencies (external Python modules required by mbed SDK): when your system requires administrator rights to install new Python packages.

### Configure workspace tools to work with your compilers
Before we can run our first test we need to configure our test environment a little!
Now we need to tell workspace tools where our compilers are.

* Please to go `mbed` directory and create empty file called `mbed_settings.py`.

* Populate this file the Python code from the previous tutorial. 

Note: Workspace tools will use the variable only if you explicit ask for it from command line. So do not worry and replace only paths for your installed compilers.

Note: Because this is a Python script and the variables are Python string variables please use double backlash or single slash as path's directories delimiter to avoid incorrect path format.

## Build Mbed SDK library from sources
Let's build mbed SDK library off-line from sources using your compiler. We've already cloned mbed SDK sources, we've also installed compilers and added their paths to `mbed_settings.py`.
We now should be ready to use workspace tools script `build.py` to compile and build mbed SDK from sources.

We are still using console. You should be already in `mbed/tools/` directory if not go to `mbed/tools/` and type below command:

```
$ python build.py -m LPC1768 -t ARM
```

or if you want to take advantage from multi-threaded compilation please use option `-j X` where `X` is number of cores you want to use to compile mbed SDK.

Above command will build mbed SDK for LPC1768 platform using ARM compiler.

Note: Workspace tools track changes in source code. So if for example mbed SDK or test source code changes `build.py` script will recompile project with all dependencies. If there are no changes in code consecutive mbed SDK re-builds using build.py will not rebuild project if this is not necessary. Try to run last command once again, we can see script `build.py` will not recompile project (there are no changes). 

### build.py script

Build script located in mbed/tools/ is our core script solution to drive compilation, linking and building process for:

* mbed SDK (with libs like e.g. Ethernet, RTOS, USB, USB host).
* Tests which also can be linked with libraries like RTOS or Ethernet.

Note: Test suite also uses the same build script, inheriting the same properties like auto dependency tracking and project rebuild in case of source code changes.

Build.py script is a powerful tool to build mbed SDK for all available platforms using all supported by mbed cross-compilers. Script is using our workspace tools build API to create desired platform-compiler builds.

## make.py script
`make.py` is a `mbed/tools/` script used to build tests (we call them sometimes 'programs') one by one manually. This script allows you to flash board, execute and test it. However, this script is deprecated and will not be described here. Instead please use `singletest.py` file to build mbed SDK, tests and run automation for test cases.

## project.py script
`project.py` script is used to export test cases ('programs') from test case portfolio to off-line IDE. This is a easy way to export test project to IDEs such as:
* codesourcery.
* coide.
* ds5_5.
* emblocks.
* gcc_arm.
* iar.
* kds.
* lpcxpresso.
* uvision.

You can export project using command line. All you need to do is to specify mbed platform name (option `-m`), your IDE (option `-i`) and project name you want to export (option `-n` or (option `-p`).

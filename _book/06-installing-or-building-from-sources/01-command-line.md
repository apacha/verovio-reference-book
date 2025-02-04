---
title: "Command-line version"
---

The Verovio codebase is C++17 compliant and is cross-platform. It has been tested on several operating systems and architectures. This section describes how to build and install the command-line version of the toolkit from the command-line or using some of the most popular IDEs. There are currently no pre-built binaries of the command-line toolkit available except for Homebrew on macOS. However, building it is very straight-forward.

### Homebrew on macOS

For macOS users using [Homebrew](https://brew.sh), the command-line version can be installed with:

```bash
brew install verovio
```

This also installs the resources and you will be ready to go.

You can also install the latest development source with: 

```bash
brew install verovio --HEAD
```

### Building on macOS or Linux

To build the command-line tool, you need [CMake](https://cmake.org) to be installed on your machine as well as a compiler supporting C++17. The commands to build are the following:

```bash
cd tools
cmake ../cmake
make
```

You can increase the building speed by using the `-j` option when running make that specifies the number of jobs to be run in parallel:

```bash
make -j 8
```

The generates a `verovio` binary within `./tools`. You can run Verovio from there or install it. Installing it means copying the executable and the resource files to directories which paths are globally accessible. You simply need to run:

```bash
sudo make install
```

If you do not install it and run it from `./tools` or from another directory, you need to use the `-r` option to set the appropriate resource directory. The parameter of the `-r` option has to be a path to the `./data` folder of the codebase.

To see the current default resource path, look for the "resource path" section in the full help output. You should see something like this:

```bash
verovio -h full
...
-r, --resource-path <s>        Path to the directory with Verovio resources (default: "/usr/local/share/verovio")
```

Keep in mind that if you have installed, you should not run another version without re-installing it or using the `-r` options to point to a non-default path, because otherwise the resources installed can be invalid. A typical problem is missing font glyphs that a newer version needs but that are not in the older version of the resources.

(Until version 2.6.0, the cmake command was `cmake .` and not `cmake ../cmake`.)

#### Basic usage

To seeing the basic command-line options, run:

```bash
verovio --help
```

To see all command-line options, run:

```bash
verovio -h full
```

For typesetting an MEI file with the default options, you need to do:

```bash
verovio -o output.svg Hummel_Concerto_for_trumpet.mei
```

If you use a version locally that is not installed, do not forget to add the `-r` parameter:

```bash
./verovio -r ../data -o output.svg Hummel_Concerto_for_trumpet.mei
```

#### Additional building options

By default, the executable is not stripped. To strip it during the installation do

```bash
sudo make install/strip
```

To build Verovio without Plaine and Easy support, run:

```bash
cmake ../cmake -DNO_PAE_SUPPORT=ON
```

To allow PAE support again, you must run the command

```bash
cmake ../cmake -DNO_PAE_SUPPORT=OFF
```

since running `cmake ../cmake` will not clear the state of the define variable.

The other building options are:

* `NO_ABC_SUPPORT` for the ABC importer to be turned on/off
* `NO_MXL_SUPPORT` for the compressed MusicXML importer to be turned on/off
* `NO_HUMDRUM_SUPPORT` for the Humdrum importer to be turned on/off
* `MUSICXML_DEFAULT_HUMDRUM` to use the MusicXML Humdrum importer by default instead of the direct MusicXML importer
* `BUILD_AS_LIBRARY` for Verovio to be built as dynamic shared library instead of a command-line executable

#### Uninstall a previous version

If you have installed Verovio with Homebrew, run:

```bash
brew uninstall verovio
```

To uninstall a previously installed version of Verovio from the system, run:

```bash
rm -f /usr/local/bin/verovio
rm -rf /usr/local/share/verovio
```

Occasionally there are problems with updates necessary to the `Makefile` when compiling a new version of Verovio with make. It may be necessary to clear out the automatically generated cmake files and regenerate them. To do that, run:

```bash
rm -rf CMakeFiles CMakeCache.txt Makefile cmake_install.cmake
```

### Windows 10

To build Verovio on Windows 10 from the command-line, you will need to have [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools) and [make](https://sourceforge.net/projects/gnuwin32/) installed on your computer.

Run the following commands from the _x86 Native Tools Command Prompt for VS_ (with administrator privileges):

```bash
cd <sourceCode>/tools 
cmake ../cmake -G "NMake Makefiles"
nmake
nmake install
```

After the installation, add `<sourceCode>/tools` to the [PATH](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/) of your system.

When running the commands, the resource path should be provided explicitly with the following option: 

```bash
-r "C:/Program Files (x86)/Verovio/share/verovio"
```

### Xcode

For macOS users, there is also an Xcode project in the Verovio root directory. 

By default, humdrum support is turned off in Xcode. To turn in on, you need to use the `Verovio-Humdrum` building scheme.

### Visual Studio

* Install [CMake](https://cmake.org)
* Go into the tools folder of Verovio
* Execute `cmake ../cmake -DNO_PAE_SUPPORT=ON` (add `-DCMAKE_GENERATOR_PLATFORM=x64` for a x64 solution)
* Open the resulting `Verovio.sln` with Visual Studio and build it from there

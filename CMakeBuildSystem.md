**Work in progress**



[CMake](http://www.cmake.org) is a cross-platform meta-build system. The tool translates the rules for building, checking and installing the OpenPhdGuiding project, written in the cmake language,
into rules specific to your development environment and platform. The same script is used for creating Visual Studio, XCode or Makefile projects.
The advantages of cmake are many, among which one can cite:
  * the maintenance of only one language for addressing the build of OpenPhdGuiding on all platforms
  * the possibility to manage big projects and complex dependencies
  * the possibility to add unit tests and installation procedures

This page describes the steps to configure PHD2 (OpenPhdGuiding) for your specific platform, as well as some hints and best practices for new `cmake` developers.
If you need more information about CMake, you may first consult the CMake documentation online. Also, a lot of resources are available on the Internet.

# Introduction #
The full build of the project in made in three steps
  1. build and/or install the dependencies
  1. configure cmake
  1. build the project


# Preliminary notes #

## Checking out the code ##
The PHD2 code is hosted on the SVN server located here: http://open-phd-guiding.googlecode.com/svn/.
The branch that supports the cmake configuration is named **`new_build_system`**. The checkout command is then the following

```
svn checkout http://open-phd-guiding.googlecode.com/svn/branches/new_build_system open-phd-guiding
```

For that to work, you need an SVN client:

  * On Debian/Ubuntu, the installation would be
```
sudo apt-get install build-essential subversion
```
  * On OSX, the svn client should be shipped with the OS (accessible from the terminal),
  * On Windows, there are several free and commercial SVN clients. Popular choices are TortoiseSVN, or the svn command-line tool available with [cygwin](http://cygwin.com/).

From now on, we suppose the code has been checked out in **`$PHD2_SRC`** (the `open-phd-guiding` path above).

## Development environments ##
The following development environment have been tested with the current `cmake` build system:
  * Windows: Visual Studio 2013. Express or Community Editions are OK.
  * OSX: official builds are done with XCode4. XCode 5 will also work.
  * Linux: gcc4.6, make

## Supported architectures ##
Currently on Windows and OSX, the supported architecture is 32 bits only. This is because of the external libraries used in the project for driving specific astronomy hardware. Most of this specificity
is handled in the cmake script. However, wxWidgets should be built accordingly.

## Visual Studio versions ##
We suppose _Visual Studio 2013_ installed for Windows developers. The code name in _cmake_ for this version of Visual Studio is _Visual Studio 12_ ("12" stands for the version, which is _Visual Studio 2013_).

## XCode versions ##
We suppose XCode 4 or XCode 5 installed for Mac OSX developers.

# Project dependencies #
The cmake construction of !PHD2 depends only on these external dependencies
  * CMake
  * wxWidgets 3.0.2
  * OpenCV 2.4.10 on Windows
  * The [Indi](http://www.indilib.org/) and [Nova](http://libnova.sourceforge.net/) library packages on Ubuntu


This means that these dependencies should be available in order to be able to construct the project, and `cmake` should know where to find these dependencies.
OpenPhdGuiding has in fact many more dependencies, but they are shipped with the project and built within the project automatically (or chosen from the
system wide installed libraries). These dependencies are:
  * openSSAG
  * libdc
  * libusb
  * cfitsio
  * and several more


## Installing CMake ##
[CMake](http://www.cmake.org) provides binary installers for Windows and OSX. On Linux, you should be able to install `cmake` via the package manager
```
sudo apt-get install cmake
```

## Building and installing wxWidgets ##

### Win32 ###

We will assume you have installed the wxWidgets files in a directory $WXWIN. C:\wxWidgets-3.0.2\ is the standard location.

To build the wxWidgets library, open a Visual Studio console ("VS2013 x86 Native Tools Command Prompt") and run the following commands to build both the Debug and Release versions of the libraries.
```
cd %WXWIN%\build\msw
msbuild wx_vc12.sln /p:configuration=Debug /p:platform=win32 /m:8
msbuild wx_vc12.sln /p:configuration=Release /p:platform=win32 /m:8
```

The `wx_vc12.sln` Visual Studio solution is for Visual Studio 12 (Visual 2013, see [here](#Visual_Studio_versions.md)).

### OSX ###

We assume you have un-tarred the wxWidgets sources in a directory `$WXWIDGETS_SRC`, and will install the headers and libraries in a directory `$WXWIN`.

```
cd $WXWIDGETS_SRC
./configure --enable-universal_binary=i386,x86_64 --disable-shared --with-libpng=builtin --with-cocoa --prefix=$WXWIN \
             --with-macosx-sdk=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/ \
             --with-macosx-version-min=10.7 \
             CXXFLAGS="-stdlib=libc++ -std=c++11" \
             OBJCXXFLAGS="-stdlib=libc++ -std=c++11" \
             CPPFLAGS="-stdlib=libc++"  \
             LDFLAGS="-stdlib=libc++" CXX=clang++ CXXCPP="clang++ -E" CC=clang CPP="clang -E"
make
make install
```

### Linux ###
Under Linux, wxWidgets should be available through your package manager. You need the _developer_ package using GTK:
```
sudo apt-get install libwxgtk3.0-dev wx-common wx3.0-i18n
```


## Installing Indi and Nova ##
On Linux, the [Indi](http://www.indilib.org/) and [Nova](http://libnova.sourceforge.net/) library packages should be available for the project.
Simply run the following command to install the development version of these packages, as follows:

```
sudo apt-get install libindi-dev libnova-dev
```

The `cmake` configuration should find these packages. It is required to have libindi-dev >= 0.9.

## Building and installing OpenCV ##
OpenCV is needed on Win32 platforms only. Nothing needs to be built since the OpenCV
zip file contains libraries already built for Windows. Just download and install OpenCV 2.4.10 from [the official website](http://www.opencv.org)
or [SourceForge](http://sourceforge.net/projects/opencvlibrary/).

We assume `$OPENCV_DIR` being a variable that points to the location where OpenCV was extracted.

# Configuring the OpenPhdGuiding project #
Once the dependencies available on your system, you are able to build the PHD2. For that, you will give the location of the dependencies as options to cmake.
The basic scheme is
  1. open a terminal. Under Windows, the terminal should be the one provided by the Visual Studio command line tool, which has the proper environment variables defining the compiler and the SDKs
  1. go to the source directory **`$PHD2_SRC`** and create a subdirectory where the project will be built
  1. type the `cmake` command line with the proper options
  1. open the generated project or build the project in place

## Creating the Visual Studio project ##
We suppose you have
  * CMake
  * Visual Studio 12 installed (see [here](#Visual_Studio_versions.md) for more details)
  * [wxWidgets](#Building_and_installing_wxWidgets.md) and [OpenCV](#Building_and_installing_OpenCV.md) built and installed

```
cd %PHD2_SRC%
mkdir tmp
cd tmp
cmake -G "Visual Studio 12" -DwxWidgets_PREFIX_DIRECTORY=%WXWIN% -DOpenCVRoot=%OPENCV_DIR% ..
PHD2.sln
```

The last line opens the solution in Visual Studio.

## Creating the XCode project on OSX ##
We suppose you have
  * CMake
  * XCode installed
  * [wxWidgets](#Building_and_installing_wxWidgets.md) built and installed

To construct the XCode project, type the commands below:
```
cd $PHD2_SRC
mkdir tmp
cd tmp
cmake -G Xcode -DwxWidgets_PREFIX_DIRECTORY=$WXWIN ..
open PhD2.xcodeproj
```

The last line opens the project in XCode. Note that the project is built only for 32 bit architecture,
as some dependencies are not available for 64 bits architecture.


## Creating the Makefiles on Linux ##
We suppose you have
  * CMake
  * a compilation toolchain (eg. `gcc`, `ld`, `make`...)
  * [wxWidgets](#Building_and_installing_wxWidgets.md) installed
  * [Indi and Nova](#Installing_Indi_and_Nova.md) library packages installed

```
cd $PHD2_SRC
mkdir tmp
cd tmp
cmake ..
make -j 4
```

As for the other platforms, it is possible to specify another wxWidgets distribution by providing the location of the installation prefix on the `cmake` command line:

```
cmake -DwxWidgets_PREFIX_DIRECTORY=$WXWIN ..
```

However if the wxWidgets package has been installed properly, the default `cmake` command should just run fine.




# Developing with `cmake` #

We provide here some tips for being able to develop with `cmake`.

As mentioned earlier, `cmake` is a program that generates for you a Visual Studio solution, an XCode workspace or a bunch of Makefiles.
One of the major benefit is that there is only one build configuration to maintain, contained in one or more text files.

## Avoid building the `cmake` project in the sources tree ##
To keep the source tree clean from the files generated by the `cmake` step, it is a good habit to separate the /cmake binary tree/ from the sources
that are under version control. Also, putting `cmake` generated files under version control is discouraged, which means that Visual Studio solutions
or XCode workspaces should not be added to the repository (as they can be generated).

## Avoid editing the generated solutions/workspaces/makefiles ##
The principal source for generating the build configuration lies inside the `cmake` configuration files (CMakeLists.txt, ...). This means that
any regeneration of the Visual Studio solutions or XCode workspace will be based on these `cmake` files, and any modification to those will be lost
during the regeneration.

## Regeneration of the Visual solutions / XCode workspaces ##
If the `cmake` files are modified, the Visual solution or XCode workspace need to be regenerated. In a solution/workspace, all targets depend on
a `cmake` specific target called `ZERO_CHECK`. This target checks if the solution/workspace needs to be regenerated before continuing the build of the
desired target.

Most of the time, building the `ZERO_CHECK` target directly from the solution explorer will bring the `cmake` changes to the solution/workspace.
In Visual Studio, each project of the solution that is modified should be unloaded and then reloaded into the solution: these changes are automatically detected by Visual Studio,
so there is no requirement for manual intervention. However, when the number of changed project is high, this unload/reload process can sometimes be time consuming.
To address that issue, it is often more efficient to close the solution, regenerate the `cmake` configuration from the command line directly, and reopen the solution.

Note that the changes on the `cmake` configuration may have very minimal impact on the changes on the projects, if written that way.
To have minimal impacts on the project,
  * avoid changing definitions or include paths that are seen by all targets. For instance, the following line
```
include_directories(${wxWidgets_INCLUDE_DIRS})
```
> has an incidence on all targets that are defined in the CMakeLists.txt and below by the `add_subdirectory` directive. Instead, once the
> !PHD2 target is defined, one can do the following:
```
target_include_directories(PHD2 PRIVATE ${wxWidgets_INCLUDE_DIRS})
```
> which makes the include of the wxWidgets path local to the !PHD2 target. `PUBLIC` visibility (instead of `PRIVATE` as shown here)
> propagates to the dependent targets.
  * Same goes for definitions.

## Avoid using "globs" ##
GLOBS in the `cmake` files take everything according to a pattern from a directory. This functionality is fine when those files should be included
in every platform the same way. This is however often not the case, and a file is likely to be included when it should not be.
The ["Zen of Python"](https://www.python.org/dev/peps/pep-0020/) to this regard, in particular "Explicit is better than implicit", is a good principle
to follow.
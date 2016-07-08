# Introduction to Cerbero

Cerbero is a Python-based packaging system for downloading, compiling, installing, and packaging GStreamer and all the libraries it uses. Instructions on how to do this for each component are codified in `recipe` files which are shipped with Cerbero itself. Cerbero can be used on Linux, OS X, and Windows, and can cross-compile for Linux, Windows, Android, and iOS. See the full [list of architectures and platforms](#supported-platforms).

The first step is to fetch the latest Cerbero from git:

    $ git clone http://anongit.freedesktop.org/git/gstreamer/sdk/cerbero.git cerbero.git
    $ cd cerbero.git

By default, this will build the latest git version of GStreamer. If you want to, for example, build the 1.8 stable release, you can checkout the `1.8` branch like so:

    $ git checkout 1.8

If you have multiple remotes, you will need to specify the pathspec and the branch name explicitly:

    $ git checkout -b 1.8 origin/1.8
    
You can switch back by doing:

    $ git checkout master

And so on.

# Table of Contents
 * [Directory Structure](#dir-structure)
 * [Command-Line Interface](#cli-usage)
 * [Supported Architectures and Platforms](#supported-platforms)

## Directory Structure <a id="dir-structure"></a>

Out of all the files and directories inside the Cerbero git directory, the following are the important ones:

    cerbero.git/
      cerbero/
      cerbero-uninstalled
      config/
      packages/
      recipes/

`cerbero-uninstalled` is the [interface to Cerbero](#cli-usage).

`cerbero` contains the Python code that constitutes Cerbero.

`config` has the various "configurations" (combinations of platforms and architectures) on and for which Cerbero can be run.

`recipes` is where you'll find the `.recipe` files that fetch, build, and install the sources for a piece of software. These are in the form of a Python derived class that overrides methods for fetching, building, and installing.

`packages` has `.package` files which list the files that each package will contain. Packages can be `.rpm`, `.deb`, `.framework`, `.msi` and `.msm`, just `.tar.bz2`, and so on. Each package file is supposed to list all the files necessary for making a particular set of libraries work. This will create two sets of packages: one contains runtime libraries and executables and the other will have development libraries and headers.

## Command-Line Interface <a id="cli-usage"></a>

Although Cerbero can be installed on your system using the bundled `Makefile`, the most common (and recommended) way to use it is by directly running it from the git repository as shown in the following commands.

Let's start with building GStreamer 1.x for the native platform and architecture. First, we must bootstrap the Cerbero build environment.

    $ python2 cerbero-uninstalled bootstrap

Next, we can build a specific recipe. Let's say, we only want the core GStreamer libraries and GStreamer base plugins

    $ python2 cerbero-uninstalled build gst-plugins-base-1.0

This will build all the recipes that are needed by `gst-plugins-base-1.0.recipe` (found in the `recipes` directory) and then build the recipe itself.

We can also build an entire package in one go. Such as the set of core GStreamer libraries and plugins:

    $ python2 cerbero-uninstalled package gstreamer-1.0-core

Or the entire set of GStreamer libraries and plugins:

    $ python2 cerbero-uninstalled package gstreamer-1.0

While on Linux, to cross-compile for Windows, you just need to add a `-c` argument specifying the appropriate configuration file to use:

    $ python2 cerbero-uninstalled -c config/cross-win32.cbc package gstreamer-1.0

While on Windows, to build for 32-bit x86 using MinGW and the MSVC toolchain (wherever possible), the config file to use is:

    $ python2 cerbero-uninstalled -c config/win32-mixed-msvc.cbc package gstreamer-1.0

## Supported Architectures and Platforms <a id="supported-platforms"></a>

Currently Cerbero supports the following architecture, platform, and toolchain combinations:

<table>
  <tr>
    <th>Platforms</th>
    <th>Toolchain</th>
    <th>Architecture</th>
    <th>Configuration</th>
  </tr>
  <tr>
    <td>Linux</td>
    <td>GCC</td>
    <td>any</td>
    <td>(native, none)</td>
  </tr>
  <tr>
    <td rowspan="4">Windows</td>
    <td rowspan="2">MinGW/GCC</td>
    <td>x86</td>
    <td>win32.cbc</td>
  </tr>
  <tr>
    <td>x86_64</td>
    <td>win64.cbc</td>
  </tr>
  <tr>
    <td rowspan="2">MSVC</td>
    <td>x86</td>
    <td>win32-mixed-msvc</td>
  </tr>
  <tr>
    <td>x86_64</td>
    <td>win64-mixed-msvc</td>
  </tr>
  <tr>
    <td rowspan="2">OS X</td>
    <td rowspan="2">Clang</td>
    <td>x86_64</td>
    <td>osx-x86-64.cbc</td>
  </tr>
  <tr>
    <td>universal</td>
    <td>osx-universal.cbc</td>
  </tr>
  <tr>
    <td rowspan="4">iOS (cross)</td>
    <td rowspan="4">Clang</td>
    <td>armv7</td>
    <td>cross-ios-armv7.cbc</td>
  </tr>
  <tr>
    <td>arm64</td>
    <td>cross-ios-arm64.cbc</td>
  </tr>
  <tr>
    <td>x86_64</td>
    <td>cross-ios-x86-64.cbc</td>
  </tr>
  <tr>
    <td>universal</td>
    <td>cross-ios-universal.cbc</td>
  </tr>
  <tr>
    <td rowspan="4">Android (cross)</td>
    <td rowspan="4">GCC</td>
    <td>armv7</td>
    <td>cross-android-armv7.cbc</td>
  </tr>
  <tr>
    <td>arm64</td>
    <td>cross-android-arm64.cbc</td>
  </tr>
  <tr>
    <td>x86</td>
    <td>cross-android-x86.cbc</td>
  </tr>
  <tr>
    <td>x86_64</td>
    <td>cross-android-x86-64.cbc</td>
  </tr>
  <tr>
    <td rowspan="2">Windows (cross)</td>
    <td rowspan="2">MinGW/GCC</td>
    <td>x86</td>
    <td>cross-win32.cbc</td>
  </tr>
  <tr>
    <td>x86_64</td>
    <td>cross-win64.cbc</td>
  </tr>
  <tr>
    <td rowspan="2">Linux (cross)</td>
    <td rowspan="2">GCC</td>
    <td>x86</td>
    <td>cross-lin-x86.cbc</td>
  </tr>
  <tr>
    <td>arm</td>
    <td>cross-lin-arm.cbc</td>
  </tr>
</table>


Support for LLVM/Clang on Linux and for GCC on OS X is easy to add if needed.

Cross-compilation targetting Windows is currently only available on Linux.

Of particular note is that while building natively on Windows with the use of Meson build files, Cerbero is able to build GStreamer and its core dependencies entirely with the Microsoft Visual C++ compiler. The full list of recipes that can use it are:

| Recipes that can use MSVC |
|---------------------------|
| bzip2.recipe |
| orc.recipe |
| libffi.recipe (only 32-bit) |
| glib.recipe |
| gstreamer-1.0.recipe |
| gst-plugins-base-1.0.recipe |
| gst-plugins-good-1.0.recipe |
| gst-plugins-bad-1.0.recipe |
| gst-plugins-ugly-1.0.recipe |

This feature is enabled by the use of `config/win32-mixed-msvc.cbc` as demonstrated in the previous section. By default if no configuration is specified the Meson build files use MinGW on Windows.

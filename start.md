# Introduction to Cerbero

Cerbero is a Python-based packaging system for downloading, compiling, installing, and packaging GStreamer and all the libraries it uses. Instructions on how to do this for each component are codified in `recipe` files which are shipped with Cerbero itself. Cerbero can be used on Linux, OS X, and Windows, and can cross-compile for Linux, Windows, Android, and iOS. See the full [list of architectures and platforms](#supported-platforms).

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

`packages` has `.package` files which list the files that each package will contain (`.rpm`, `.deb`, `.framework`, or just `.tar.bz2`). Each package file is supposed to list all the files necessary for making a particular set of libraries work.

## Command-Line Interface <a id="cli-usage"></a>

## Supported Architectures and Platforms <a id="supported-platforms"></a>

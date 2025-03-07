# Welcome to HyperHaskell,

… the strongly hyped Haskell interpreter.

[![Hackage](https://img.shields.io/hackage/v/hyper-haskell-server.svg)](https://hackage.haskell.org/package/hyper-haskell-server)
![Build Status](https://github.com/HeinrichApfelmus/hyper-haskell/actions/workflows/build-haskell.yml/badge.svg?branch=master)
![Hype level](https://img.shields.io/badge/hype-level_%CE%B1-ee40bb.svg)


*HyperHaskell* is a graphical interpreter for the programming language [Haskell][]. You use worksheets to enter expressions and evaluate them. Results are displayed graphically using HTML.

*HyperHaskell* is intended to be *easy to install*. It is cross-platform and should run on Linux, Mac and Windows. Internally, it uses the [GHC][] API to interpret Haskell programs, and the graphical front-end is built on the cross-platform [Electron][] framework.

*HyperHaskell*'s main attraction is a [`Display`][display] class that supersedes the good old [`Show`][show] class. The result looks like this:

  <img src="docs/screenshots/worksheet-diagrams.png" height="500">

  [haskell]: https://haskell.org
  [show]: http://hackage.haskell.org/package/base/docs/Prelude.html#t:Show
  [display]: https://hackage.haskell.org/package/hyper/docs/Hyper.html#t:Display
  [ghc]: https://www.haskell.org/ghc/
  [electron]: http://electron.atom.io/
  [stack]: https://www.haskellstack.org
  [release]: ../../releases

# Installation
## Overview

*HyperHaskell* is intended to be easy to install. The easiest way to install it is to download the binary distribution. This is explained in the next subsection. However, there is a pitfall which you have to know about, and which requires knowledge of the installation structure.

A HyperHaskell installation consists of two parts:

1. The graphical front-end.

    Currently written in HTML and JavaScript, packaged with the [Electron][] framework.

2. The interpreter back-end.

    Consists of an executable `hyper-haskell-server`,
    written in Haskell using the [GHC API][ghc],
    and a library (module) `Hyper` for visualizing and pretty printing Haskell values.

    Both parts depend on several different Haskell packages.
    Unfortunately, the versions of the packages used to compile the executable
    and to compile the library have to be exactly the same.

    This is why, at the moment,
    the front-end does *not* come with the back-end executable included.
    Instead, the user is asked to install the `hyper-haskell-server` back-end
    into his or her own database of Haskell packages,
    and then tell the front-end about it.
    This way, the user is free to use different package or compiler versions.

## Installation of the binary distribution

Installation from the binary distribution follows the structure explained above.

1. [Download the graphical front-end from the latest release][release] and unpack it.

    ![App](docs/screenshots/app-osx.png)

    At the moment, binaries for macOS and Windows (thanks to Nicholas Silvestrin) are provided. Since Heinrich Apfelmus only has access to a macOS machine, help is appreciated!

2. Install the back-end server

    1. Make sure that you have a working installation of the [GHC][] Haskell compiler.

    2. Install the back-end with Cabal by executing

            cabal install hyper hyper-haskell-server

        It is also recommended (but not necessary) that you install the additional support for other popular Haskell packages, e.g. the [Diagrams][] library by additionally executing

            cabal install hyper-extra

    3. Now you can start the front-end application and create a new worksheet, or open an existing one. Make sure that the "Interpreter Back-end" in the "Settings" section of the worksheet is set to "cabal". (The path field does not matter in this case.)

        ![Settings](docs/screenshots/settings-back-end-cabal.png)

        It is also possible to use [Stack][] by using `stack install`, but that is not fully explained here, only to some extent below.

That's it! Happy hyper!

  [diagrams]: https://github.com/diagrams

## Run from source

When developing HyperHaskell itself, it is also possible to run it from source. Follow these steps:

1. [Download and install Electron](http://electron.atom.io/releases/)

    The whole thing is currently developed and tested with Electron version 10.1.5.
    
    (If you use the [npm][] package manager, you can install it in your home directory with `cd ~ && npm install electron@10.5.1`.
    On Debian-based Linux distributions, Electron [currently](https://github.com/electron-userland/electron-prebuilt/issues/70#issuecomment-192520913) requires the `nodejs-legacy` package.)

2. Make sure that you have a working installation of
    * the [GHC][] Haskell compiler
    * the [`cabal`][cabal] utility

    (See the [Haskell homepage][haskell] for more on how to obtain these.)

3. You also need the `just` utility, which should be available for any UN*X platform. Edit the file named [`justfile`](justfile) and tell it where to find the Electron executable

    On macOS: Typically,

        ELECTRON := "/Applications/Electron.app/Contents/MacOS/Electron"

    On Linux: Typically,
    
        ELECTRON := "/usr/local/bin/electron"

    On Windows: You can locate `electron.exe` and double-click it. Then simply drop the `hyper-haskell\app` folder onto the lower pane of the window. Alternatively, from the terminal invoke what is suggested in the upper portion of the `Electron` window, i.e.
    
        <path-to-electron>\electron.exe hyper-haskell\app

4. Go into the root directory of this repository and type `just run`.

        $ cd hyper-haskell
        $ just run

    This will call the `cabal` utility to build the server back-end,
    and finally run the front-end.

5. Use the *File* menu to open one of the example worksheets from the [worksheets](worksheets/) folder. Voilà!

    You can also create a new worksheet, but note that you have to set the back-end path in the "Settings" section. The path is relative to the directory where the worksheet was saved. For instance, if you run a worksheet from the [worksheets](worksheets/) directory, the path `../haskell/stack.yaml` will point to the right `hyper-haskell-server` executable. Screenshot:

    ![Settings](docs/screenshots/settings-back-end-stack.png)

    Note that for this setting, the `stack` utility has to be in your path. You can also set an explicit path for this utility in the "Preferences…" menu item.


## Packaging

The normal way to obtain HyperHaskell is to download the application bundle in binary form. This section describes how to generate this from source.

We use the [`electron-packager`][pkg] utility. To install it, you need to use the [npm][] package manager and execute the following commands:

    cd ~
    npm install electron-packager
    npm install electron@10.1.5 --save-dev

To create an application bundle and compress it in a zip-file, use the following commands:

  * On macOS:

        make pkg-darwin
        make zip-darwin

  * On Windows: You need the [7zip](https://www.7-zip.org) utility in your path.

        make pkg-win32
        nake zip-win32

  * On Linux: not implemented yet

  [npm]: https://www.npmjs.com/
  [pkg]: https://github.com/electron-userland/electron-packager



# Contributors

Many thanks to everyone who contributed, provided feedback or simply found a nice application for HyperHaskell! In particular, many thanks to:

Moritz Angermann, Simon Jakobi, Rodney Lorrimar, Karshan Sharma, Nicholas Silvestrin, [*and many others*](CONTRIBUTORS).

The project was started by *Heinrich Apfelmus*.

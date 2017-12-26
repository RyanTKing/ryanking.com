+++
type = "itemized"
author = "Ryan King"
title = "Chip-8 Emulator"
description = "A fully-functional Chip-8 emulator with strong type-checking."
date = "2017-05-03"
linktitle = "Github Repository"
format = "ATS"
link = "https://github.com/RyanTKing/ats-chip8"
+++

The long term goal of the project is to use the same implementation with several graphics wrappers such as SDL, X11, and Javascript. Right now, it only supports SDL2.

# Building

The entire emulator is built as the default *GNU Make* routine. The following routines are available along with

Build the emulator (done by default):

    $ make
    $ make build

Install NPM dependencies:

    $ make npm-install

Update NPM dependencies:

    $ make npm-update

# Running

The executable located in the BUILD folder can be run with a path to a game file (many are included in the GAMES) folder. A simple script is provided to simplify it, that can be used as such:

    $ ./run-chip8 [GAME_NAME]

The game name is the name of one of the provided games. If no game name is specified, the list of available games is provided.

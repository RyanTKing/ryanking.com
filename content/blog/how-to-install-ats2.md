+++
author = "Ryan King"
categories = ["ATS"]
date = "2016-09-25T3:29:33-04:00"
description = "A quick guide on how to install ATS on *nix operating systems."
featured = "ats.jpg"
featuredalt = "A simpe ATS program"
featuredpath = "date"
linktitle = "How to Install ATS"
title = "How to Install ATS"
type = "post"
draft = "false"

+++

**Update (2/14/2017):** This tutorial is now out dated! Please see the updated one [here](http://ryanking.com/blog/Joy-of-ATS-1-Installing-ATS/ "Updated ATS Tutorial")!

# What is ATS?

From the ATS2 website, [ats-lang.org](http://www.ats-lang.org "ATS Website"):

> ATS is a statically typed programming language that unifies implementation with formal specification. It is equipped with a highly expressive type system rooted in the framework Applied Type System, which gives the language its name. In particular, both dependent types and linear types are available in ATS.

In essence, ATS is a language with a very in-depth type system and strict set of rules for designing programs in it. The natural consequence of that design is that once code written in ATS compiles, it is "correct" in the sense that finding runtime errors is much less likely than other languages since the sophisticated type checker can sniff out most errors before compilation.

Another huge advantage to programming in ATS is that it actually transpiles, which means that the code is turned into another programming language. What that means is that you can use all the features and libraries that already exist in another language (C, Python, Clojure, etc.) in ATS without having to rewrite or port them.

# Dependencies

Before we get to ATS itself, we need to grab a few programs/utilities that ATS requires to be built:

* **GCC:** GNU Compiler Collection is a set of compilers for common programming languages, mainly used to build C code.
* **Git:** Popular distributed version control system which will be used to retrieve ATS and keep it updated.
* **GMP:** GNU Multiple Precisiion Arithmetic Library is a correct and efficient math library.
* **BDWGC:** A powerful garbage collector for C/C++.
* **JSON-C:** A tool for creating and reading JSON objects in C.
* **Erlang:** A functional programming language used in certain ATS extensions.
* **Z3:** A high performance theorem prover.

## Installing on Linux

All the dependencies can be installed using your Linux package manager, for the sake of the example I will be using apt-get, but simply replace the apt-get command with the one corresponding to your package manager.

    $ sudo apt-get update
    $ sudo apt-get install -y gcc
    $ sudo apt-get install -y git
    $ sudo apt-get install -y build-essential
    $ sudo apt-get install -y libgmp-dev
    $ sudo apt-get install -y libgc-dev
    $ sudo apt-get install -y libjson-c-dev
    $ sudo apt-get install -y erlang
    $ sudo apt-get install -y z3

## Installing on macOS

Apple provides a package for macOS that includes a few essential development tools such as gcc and git. In order to install, simply execute the following command in terminal:

    $ xcode-select --install

Linux distributions usually include a package manager (apt-get, pacman, yum, etc.), but on macOS you'll need to install a third party one, Homebrew is usually the package manager of choice, which can be installed by running the following command in terminal:

    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

You can read more about Homebrew on its website, [brew.sh](http://brew.sh/ "Homebrew Website"). Other package managers are available such as fink ([finkproject.org](http://finkproject.org/ "Fink Website")) or MacPorts ([macports.org](http://macports.org/ "MacPorts Website")), but as of now Homebrew has become the de facto standard so has the most packages, is the most actively developed, etc.

Here are the sequence of homebrew commands to install the remaining dependencies:

    $ brew doctor
    $ brew install git
    $ brew install gmp
    $ brew install bdw-gc
    $ brew install json-c
    $ brew install erlang
    $ brew install z3
    $ brew cleanup

# Setting up for the Install

Now that all the dependencies are installed, we can grab a fresh copy of ATS and ATS contributions, which include a bunch of programs to compile ATS into various languages and do other tasks. You can switch to any directory you want, but I recommend just putting them in your home directory since its only two folders. Once again from terminal:

    $ git clone git://git.code.sf.net/p/ats2-lang/code ATS2
    $ git clone https://github.com/githwxi/ATS-Postiats-contrib.git ATS2-contrib

To build/install/use ATS, a few changes to the environment variables must be made. Environment variables are basically like variables in a programming language only for Unix-like operating systems, for example, $HOME holds the location of the current user's home directory. The following set of commands will create the variables, which will only last until the current terminal session is closed:

    $ export PATSHOME=${PWD}/ATS2
    $ export PATSHOMERELOC=${PWD}/ATS2-contrib
    $ export PATH=${PATSHOME}/bin:${PATH}

The ${PWD} portion of the command is another command that will be called first that will get the full path to the current directory where you cloned the two folders to. The first two variables are used by ATS, but the last one, PATH is a special variable that basically tells your terminal where on your computer file system you store executable programs, for example, when you type "git" into the prompt, your computer searches all the folders stored in the PATH variable for an executable with the name "git" then runs it. Since we will be storing ATS executables in our ATS2 folder, we will add it to the path.

Now we will make the variables permanent by adding them to a file called .bashrc. That file basically stores configurations for your terminal, so if we add those variables there, everytime you open a terminal, the variables will be set. If you use another prompt such as zsh, you can add them to your .zshenv or similar file, but generally speaking, the following commands should take care of it:

    $ echo "export PATSHOME=${PWD}/ATS2" >> ${HOME}/.bashrc
    $ echo "export PATSHOMERELOC=${PWD}/ATS2-contrib" >> ${HOME}/.bashrc
    $ echo "export PATSHOME_contrib=${PWD}/ATS2-contrib" >> ${HOME}/.bashrc
    $ echo "export PATH=\${PATSHOME}/bin:\${PATH}" >> ${HOME}/.bashrc

Echo is a fancy way of saying "print" and the ">> ${HOME}/.bashrc" will make it so instead of printing out to the terminal, the quoted command will be added to the .bashrc file.

# Building ATS (finally)

Now that all the setup is done, we can finally build ATS!

    $ cd ATS2
    $ ./configure
    $ make all

That's it! If you've done everything correct up until this point, you should be able to execute the following command get similar output:

    $ which patsopt
    /Users/RyanKing/ATS2/bin/patsopt

## Extensions

The following commands will install the extension for interfacing with z3 for theorem proving:

    $ cd ${PATSHOMERELOC}/Projects/MEDIUM/ATS-extsolve
    $ make DATS_C
    $ cd ../ATS-extsolve-z3
    $ make build
    $ mv -f patsolve_z3 ${PATSHOME}/bin

Now we will add support for compiling into other languages, the following is needed for any language we want to compile to:

    $ cd ${PATSHOMERELOC}/Projects/MEDIUM/CATS-parsemit
    $ make DATS_C

First, we'll add Javascript support:

    $ cd ${PATSHOMERELOC}/Projects/MEDIUM/CATS-atsccomp/CATS-atscc2js
    $ make build
    $ mv -f atscc2js ${PATSOME}/bin
    $ cd ${PATSHOMERELOC}/contrib/libatscc/libatscc2js
    $ make all
    $ make all_in_one

Now Python:

    $ cd ${PATSHOMERELOC}/Projects/MEDIUM/CATS-atsccomp/CATS-atscc2py3
    $ make build
    $ mv -f atscc2py3 ${PATSHOME}/bin
    $ cd ${PATSHOMERELOC}/contrib/libatscc/libatscc2py3
    $ make all
    $ make all_in_one

And Clojure:

    $ cd ${PATSHOMERELOC}/projects/MEDIUM/CATS-atsccomp/CATS-atscc2clj
    $ make build
    $ mv -f atscc2clj ${PATSHOME}/bin
    $ cd ${PATSHOMERELOC}/contrib/libatscc/libatscc2clj
    $ make all
    $ make all_in_one

And finally, erlang:

    $ cd ${PATSHOMERELOC}/projects/MEDIUM/CATS-atsccomp/CATS-atscc2erl
    $ make build
    $ mv -f atscc2erl ${PATSHOME}/bin
    $ cd ${PATSHOMERELOC}/contrib/libatscc/libatscc2erl
    $ make all
    $ make all_in_one
    $ cd Session
    $ make all
    $ make all_in_one

That's it for this guide, hopefully it gets everything up and running for you, let me know in the comments of any issues and I'll try my best to fix them.

Happy type-checking!

+++
title = " The Joy of ATS 2: A Simple Game"
date = "2017-12-25T20:00:00"
author = "Ryan King"
categories = ["ATS", "Tutorials"]
description = "Writing your first lines of code in ATS"
linktitle = "The Joy of ATS 2: A Simple Game"
featured = "question-mark.jpg"
featuredpath = "date"
featuredalt = "A question mark"
type = "post"
+++

# Hello, World

Now that we have an ATS install up and running on our system, it's time to put it to use. We sill start off by writing the standard "hello world" program because the Computer Science Tutorial Gods might smite me if we do not. Start by making a new file in your favorite text editor, (I use Emacs with the ats2-mode plugin), and enter the following text:

~~~ats
implement main0() = () where {
  val _ = print("Hello, World!\n")
}
~~~

Now that we some code written, save it as "hello.dats" and compile with:

~~~bash
$ patscc -o hello hello.dats
~~~

And then if you run it, you should see the following output:

~~~bash
$ ./hello
Hello, World!
~~~

Let's break down this program. The first word you see is "implement," which, in plain English, means "we have already defined this function so our program knows what it is and can call it, and now we are writing what it actually does." The reason we use "implement" for main0 without previously defining it ourselves is because, like in C, the main function is the entry-point for our program, so the definition already exists in every ATS program, we are merely just describing what it does in our particular program.

Now, let's look at our main function in detail. We set main0 equal to (), which means void, but the keyword after the parenthesis, "where," is what allows us to print our phrase even though the function returns void. The "where" keyword means that we have a value which uses the following "sub-values" or "sub-functions." We can see a more clear example of its use here, note that the "fun" keyword allows us to create a new function:

~~~ats
fun foo(): a + b where {
  val a = 5
  val b = -3
}
~~~

Predictably, "foo" will return 2, because it is returning the sum of a and b *where* a is equal to 5 and b is equal to -3; outside of the where statement, a and b will be undefined, much like how if we define variables in an if block in C, the values are not accessible outside of the if block.

The last line we need to look at in our hello program is the one value assignment within the where block. We are assigning the returned value of the function "print" to the label "_", which might make sense if you are familiar with Haskell. The underscore means that we do not care about the return value of the function so we do not need to store its return value in a variable. What we care about is the print function's "side effect," which in functional programming terms, means something that happens as the result of calling a function that does not impact the return value, e.g., printing to STDIN, drawing something to the screen, playing a sound, updating a global variable, etc.. So, the whole line means "we are calling the print function, but only care about the side effect of it printing to STDIN so do not need to store its return value."

The where statement is one way to create a list of value assignments that can be used to contribute to one value, but the alternative is a "let" statement. We will rewrite our sample function using that:

~~~ats
fun foo():
  let
    val a = 5
    val b = -3
  in
    a + b
  end
~~~

Whether you use "let" or "where" is a style choice, I typically use a where statement if the final value is something simple such as one function call or variable and a let statement when I have something more complicated. Since ATS does not have any real "strict" style guidelines and I care very much about code style, I have my own guidelines that I try to apply to all of my code, and should probably write out formally. ATS is a somewhat complex language syntactically so I like to do what I can to make my code as readable as possible.

# Values All the Way Down

ATS's primary programming paradigm is that of functional programming (not to mean its ONLY paradigm), which Wikipedia defines as follows:

>In computer science, functional programming is a programming paradigm—a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. It is a declarative programming paradigm, which means programming is done with expressions[1] or declarations[2] instead of statements. In functional code, the output value of a function depends only on the arguments that are passed to the function, so calling a function f twice with the same value for an argument x produces the same result f(x) each time; this is in contrast to procedures depending on a local or global state, which may produce different results at different times when called with the same arguments but a different program state.

In layman's terms, we can think of a program written in a functional style as a list of values and function. Let's look at the following bit of ATS code:

~~~ats
val a = 5
val b = 6
fun myadd(x: int, y: int): int = x + y
val c = myadd(a, b)
~~~

The first two lines define two integer values, the third line creates a function that takes two integer parameters and returns an integer via adding its parameters, and the third line creates a third value that is the return value of our function with the first two values passed as parameters.

# Guess the Number

The main project for this entry is going to be a simple game (surprising, I know) called "Guess the Number," which lets the user think of a number then guesses it (again, surprising). The game runs as follows:

1. The program outputs a welcome message.
2. The program tells the user to think of a number between the starting lower bound and starting upper bound.
3. The program outputs a guess that is halfway between the low and high value.
4. The user tells the program whether the value is lower than its guess, higher than its guess, or the guess is correct.
5. The program does the following based on the user input:
    * If the guess is too low (the user inputed ">"), it sets the upper bound to the guess and goes back to step 3.
    * If the guess is too high (the user inputed "<"), it sets the lower bound to the guess and goes back to step 3.
    * If the guess is correct (the user inputed "="), it continues.
6. The program asks the user if it wants to play again and does the following based on the user input:
    * If the user says yes (the user inputed "y"), it goes back to step 2.
    * If the user says no (the user inputed "n"), the game ends.

As you can see, the guess the number program does not really guess as much as it does a binary search with the help of user input. Also note that in this implementation, we use user inputs of ">", "<", and "=" to represent a guess that is less than the target number, greater than the target number, and equal to the target number respectively.

## The Statics

The first bit of code we are going to look at is contained in a file we will create called **guess.sats**:

~~~ats
fun start_game(): void
fun game_loop(low: int, high: int): void
fun get_response(): string
fun play_again(): void
~~~

This is what we call a **static file**. In ATS, "statics" refer to the formal definition of functions and values, but not actually describing what the value holds or what the function does, just its name and the type associated with it. On the first line, we are saying that we have a function called "start_game" that takes no parameters and returns type void. This allows our type-checker to know that any calls to the function called "start\_game" should not take any parameters and will return a value of type void. Similarly, on the second line, we define the "game\_loop" function that takes two integers as parameters and also returns type void. Defining functions in a separate file for a relatively simple program may seem superfluous, but the formality in the code will allow our type-checker to sniff out any potential errors in the writing of our code instead of when we run it. For example, the following code will not type-check:

~~~ats
val foo = start_game()
val bar = game_loop(3, foo)
~~~

Our type-checker knows that "foo" will be of type of void due to the definition of "start\_game" and "game\_loop" takes integer parameters so it will not let us compile our faulty code and tell us that we are passing a parameter of the wrong type.

## SATS in the HATS

We will quickly author one special file that is a part of most ATS programs: **staloadall.hats**. The filename is conventional name and simply means "statically load all." All we put in this file is our includes and static loads so that way in our code files, we don't need to manually load what can be a long list of libraries and static files. Ours is relatively short, and since we will only have one code file, it is a bit unnecessary, but it is a good habit to get in for when we get into large, multi-file ATS projects that have long lists of dependencies. Here is the one for this project:

~~~ats
#include "share/atspre_staload.hats"
#include "share/HATS/atspre_staload_libats_ML.hats"

staload "./guess.sats"
~~~

All this files does is include two built-in ATS hats files that load a lot of common libraries as well as statically load, via the "staload" keyword, the static file that we just wrote.

## What's in a (file)name (extension)?

So far, we've seen a few different types of file extensions. The extensions do not change what the files mean (generally), rather, they are merely conventions since every extension represents a different "part" in an ATS program. I will briefly now describe what the four main ones are:

* **.dats:** This is where your **dynamics** go, which is actual implementations of functions, values, etc.. Think of these files as corresponding to ".c" files in a C program.
* **.sats:** As we saw, these files are were your statics go. Think of these files as corresponding to ".h" files in a C program.
* **.hats:** These files are simply a list of static loads and includes. If you are of the habit of having one header file in C program that just includes all the external library need, this is basically that file.
* **.cats:** We won't have to deal with these for a little bit, but these ARE treated differently by the compiler to the ones listed above because these files hold C code instead of ATS code. ATS transpiles to C code, and sometimes, especially when dealing with C libraries, we need to put in some raw C code to make it work properly. Don't worry too much about these yet.

## Preparing for Compilation

Before we get to the real meat and potatoes of the game, let's set up a Makefile because we are rarely going to be writing an ATS program where spitting off compilation commands straight from the terminal is practical or good practice. I am going to assume a basic knowledge of GNU Make and just point out a few things.

~~~Makefile
PATSCC = ${PATSHOME}/bin/patscc
PATSOPT = ${PATSHOME}/bin/pastopt

INCLUDE = -DATS_MEMALLOC_LIBC -latslib

hello: hello.dats
	$(PATSCC) -o $@ $<

guess: guess.dats
	$(PATSCC) $(INCLUDE) -o $@ $<

clean:: ; @rm -f *~
clean:: ; @rm -f *_?ats.c
clean:: ; @rm -f *_?ats.o
clean:: ; @rm -f hello
clean:: ; @rm -f guess
~~~

All we do in this Makefile is alias our compiler and type-checker (we don't need it, but I put it in every ATS Makefile by habit), set our includes, and create routines for both our programs as well as some clean routines to remove the intermediate C files and our compiled programs. The first include, "-DATS\_MEMALLOC\_LIBC" is required anytime you are doing anything that might need to allocate memory, such as read a string from STDIN, and the second include, "-latslib", adds in the function objects for a variety of functions defined in the ATS distribution. Both are usually good to include since a majority of programs will require them.

## The Meat and Potatoes

Now that we have our statics defined it's time to get to the actual implementation of our game. We will break it down function by function starting at the entry point and working from there.

~~~ats
implement main0() = start_game() where {
  val _ = print("Welcome to Guess the Number!\n")
}
~~~

All we do in main0 is print out a nice, friendly welcome message, and return the value of our "start\_game" function, which, by the statics, we can see also returns void. This is another instance where we do not care about the return value, but want to call the function because its side effects are what allow the game to run. Now let's look at the start\_game function:

~~~ats
local
  val low = 0
  val high = 100
in
  implement start_game() = game_loop(low, high) where {
    val _ = println!("Please think of a number between ", low, " and ", high)
    val _ = print("Press enter when you have it.")
    val _ = fileref_get_line_string(stdin_ref)
  }
end
~~~

The first thing that will probably stick out is the keyword "local". We can think of the progression of "local", "in", and "end" similar to that of "let", "in", and "end", only outside of the function in the global scope of the program. Within the first section (between "local" and "in"), we define two variables, "low" and "high", which is the starting range for our guesses. We choose to define them outside of the function instead of hard-coding them in so that way we can easily change those two values and have them be reflected in both instances where we reference them. In this function, we tell the user to think of a number, and retrieve a line from STDIN. We don't care what the line is, we just want to halt program execution until the user hits enter (sending in a line). The "println!" function is similar to "print", which we used early, except that it takes a comma-separated list of values which are then concatenated along with a newline character at the end. After the two print statements have been sent to STDOUT and the user sends a line to STDIN, the return value is set equal to "game\_loop" with the default low and high rangers passed as parameters.

~~~ats
implement game_loop(low, high) =
  let
    val guess = (high - low) / 2 + low
    val _ = print!("Is your number ", guess, "?\n")
    val line = get_guess()
  in
    ifcase
    | line = "=" => play_again() where {
        val _ = println!("I got it!")
      }
    | line = ">" => game_loop(guess, high)
    | line = "<" => game_loop(low, guess)
  end
~~~

This function calculates the guess by finding the midpoint between the high and low value, prints out the guess, then asks the user if the guess is higher, lower, or correct. The keyword "ifcase" is a clean way to do a series of if/else statements; each condition begins with a pipe, the statement to the left of the "=>" symbol is the condition the check, and the statement to the right is what to execute if the condition evaluates to true. Note that the last condition, "\_" is the default, think of it as the final "else" statement. Also note that the statements are checked in the order that they are written. If the user says the guess is correct, we go into our "play\_again" function, and if its lower/higher we call the game\_loop function recursively with the high or low bound set to the guess.

Lastly, let's look at our two helper functions:

~~~ats
implement play_again() =
  let
    val _ = print("Play again? (y/n) ")
    val line = fileref_get_line_string(stdin_ref)
  in
    ifcase
    | line = "y" => start_game()
    | line = "n" => println!("Goodbye!")
    | _ => play_again() where {
        val _ = println!("Please enter 'y' or 'n'.")
      }
  end

implement get_response() =
  let val line = fileref_get_line_string(stdin_ref) in
    if line = ">" || line = "<" || line = "=" then line
    else get_response() where {
      val _ = println!("Please enter '<', '>', or '='.")
    }
  end
~~~

The first function, "play\_again", asks the user whether they want to play, and if the user sends in "y", it runs the game from the beginning by calling the function that starts the game, if the user inputs "n", it says goodbye and allows the program to finish execution, otherwise, it tells the user that its input is not valid and recursively calls itself.

The second function, "get\_response", is similar. It gets user input, if the input is one of the three valid options, it returns it, otherwise, it yells at the user and recursively calls itself.

So now, we should have three ATS files and a Makefile. If we call make and the executable, it should be able to guess our number (say 69):

~~~bash
$ make guess
/Users/ryanking/Projects/ATS2/bin/patscc -DATS_MEMALLOC_LIBC -latslib -o guess guess.dats
$ ./guess
Welcome to Guess the Number!
Please think of a number between 0 and 100
Press enter when you have it.
Is your number 50?
>
Is your number 75?
<
Is your number 62?
>
Is your number 68?
>
Is your number 71?
<
Is your number 69?
=
I got it!
Play again? (y/n) n
Goodbye!
~~~

It works! You can find all the source code for this post (and future posts) in my [Github repository](https://github.com/RyanTKing/Joy-of-ATS).

One final note, you'll notice that we don't specify return types or parameter types when implementing our functions because the statics already do that for us!

# In Summary

All right, since this might have been a lot to bite off for your first dive into ATS, I'll try to summarize a few key points:

* Statics are where you define functions and values, usually in a file ending with ".sats" or in your main ATS file with the "extern" keyword before it. Defining functions and values associates a name with a type, but does not handle the implementation.
* The dynamics, which is where functions are given implementations and variables are given values are usually in a file ending with ".dats". This is what turns into machine code when compiled.
* Values have scopes, and those scopes are defined with "where", "let"/"in"/"end", "local"/"in"/"end", etc..
* Every function has a return value, but sometimes we don't care about it due to the function's side effects.
* A function takes in zero or more values and returns something, we can use "let" or "where" to simulate statements.

I hope you learned something and understand a good chunk of the code that I just threw at you, as always, leave any questions or criticism in the comments below!

Happy type-checking!

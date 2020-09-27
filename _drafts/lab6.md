---
title: week 7 lab
description: Intro to shells
layout: default
due: Monday, October 5th 5pm Chicago time
date: 2020-10-05T17:00:00
# notes get passed through markdownify
github_link: TODO
pairings: TODO
---

# Introduction to Homework 3

In this lab, we will be writing a shell program.  A shell is the program
that all of you use when you ssh to a linux machine, or hit ``ctrl+```
to open a terminal in vscode.
It's the program which prints out a prompt, reads in what you type, and
then executes whatever instruction you told it to do.  It is different
from ssh (which is the program you use to connect to the server), or
linux (which is the OS on which your shell is running). The assignment
is not yet posted, but the basic loop for our (and any) shell, will be:

* Prompt the user
* Read and parse the command the user types in
* Create a new process to run the command the user typed
* Wait for the new process to end and print out its exit status
* Start over with the prompt


## The high level structure of a shell

[This week's lab]({{page.github_link }})'s skeleton code is a stripped down version of the basic
shell that is presented in the book in Chapter 8. In homework 3, we will be switching it over to
using `spawn` instead of `fork`, and then we will be adding the ability to handle signals (the topic
of last week's lecture) and file redirection (the topic of this week's lecture). This week's lab
will help you do the first step.

If you'd like to go over the absolute basics of creating new processes and running different
programs, you can take a look at the code that Prof. Kanich used for the fork video
[here](https://github.com/csatuic/vscode-lectures/tree/master/lecture7).

To get started, clone the repository and get it started - as usual, you can either do this on your
laptop via a devcontainer or a systemsX machine on campus.

Take a close look at forkshell.c, and give yourself some time to understand what each function does.

* `eval` takes as input one string written by the user.
    * `parseline` separates that one long string into an `argv` style collection of pointers to the
      individual "words" of the command.
    * `builtin_command` checks to see whether the user entered a special command, like `quit`. If
      there's a special command, you don't need to fork a new process, because it's something the
      shell should take care of.

## How a fork based shell works

In `forkshell.c`, after `eval` has determined that it is _not_ running a builtin command, it goes
through the process of forking a new process. Take a look at that code in `eval` and make sure you
understand what it's doing.

To dig into how those functions work, take a look at the man pages for `fork`, `waitpid`, and
`execve`. (To view a man page, type `man fork` or `man waitpid` into terminal.)

## How a spawn based shell works

Now, you'll be changing your code over from a fork-based shell to a spawn-based shell. Accept
Homework 3 if you haven't already, and open it up. Your task for the remainder of the lab is to
modify your version of `spawnshell.c` so that it uses `posix_spawnp` rather than `fork`. You can take
a look at `posix_spawn` man page, the fork video, and the `vscode-lectures` example code to get an
idea of how to modify fork-based child process creation into a spawn-based child process creation.
Using `posix_spawnp` makes life a little easier because it uses the `$PATH` enviornment variable to
find executables.

Once you have modified your code, the testing script in the lab directory should work.
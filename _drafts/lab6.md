---
title: week 7 lab
description: Intro to shells
layout: default
due: Monday, October 5th 5pm Chicago time
date: 2020-10-05T17:00:00
# notes get passed through markdownify
github_link: https://classroom.github.com/a/R7pM_QQW
pairings: https://docs.google.com/spreadsheets/d/1aDhmwnsBxdncsRgVD9pffNOm4_yM3I0dvijhO0Cs0wo/
---

# Introduction to Homework 3

In this lab, we will be writing a shell program.  A shell is the program that all of you use when
you ssh to a linux machine, or hit ``ctrl+``` to open a terminal in vscode. It's the program which
prints out a prompt, reads in what you type, and then executes whatever instruction you told it to
do.  It is different from ssh (which is the program you use to connect to the server), or linux
(which is the OS on which your shell is running). The assignment is not yet posted, but the basic
loop for our (and any) shell, will be:

* Prompt the user
* Read and parse the command the user types in
* Create a new process to run the command the user typed
* Wait for the new process to end print the previous command's exit status if the user types the command `?`
* Start over with the prompt


## The high level structure of a shell

To start this week's lab, [accept Homework 3]({{page.github_link }}) via GitHub. This skeleton code
is a stripped down version of the basic shell that is presented in the book in Chapter 8. In
homework 3, we will be switching it over to using `spawn` instead of `fork`, and then we will be
adding the ability to handle signals (the topic of this week's lecture) and file redirection (the
topic of next week's lecture). This week's lab will help you do the first step.

If you'd like to go over the absolute basics of creating new processes and running different
programs, you can take a look at the code that Prof. Kanich used for the fork video
[here](https://github.com/csatuic/vscode-lectures/tree/master/lecture7).

To get started, clone the repository and get it started - as usual, you can either do this on your
laptop via a devcontainer or a systemsX machine on campus.

Take a close look at `forkshell.c`, and give yourself some time to understand what each function does.

* `eval` takes as input one string written by the user.
    * `parseline` separates that one long string into an `argv` style collection of pointers to the
      individual "words" of the command.
    * `builtin_command` checks to see whether the user entered a special command, like `quit`. If
      there's a special command, you don't need to fork a new process, because it's something the
      shell should take care of.

> Suggested activity: convince yourself that you understand how `parseline` is manipulating `buf`
> and `argv`.

## How a fork based shell works

In `forkshell.c`, after `eval` has determined that it is _not_ running a builtin command, it goes
through the process of forking a new process. Take a look at that code in `eval` and make sure you
understand what it's doing.

To dig into how those functions work, take a look at the man pages for `fork`, `waitpid`, and
`execve`. (To view a man page, type `man fork` or `man waitpid` into terminal.)

## How a spawn based shell works

A spawn based shell is nearly identical to a fork based shell, however spawn only returns once, and
it returns success or failure, rather than returning the process id. You will have to create a
variable to store the process id, and pass it by reference to the `posix_spawn` function.

## Getting started on Homework 3

Accept Homework 3 if you haven't already, and open it up. Your task for the remainder of the lab is
to modify your version of `spawnshell.c` so that it uses `posix_spawnp` rather than `fork`. You can
take a look at the `posix_spawn` man page, the fork video, and the `vscode-lectures` example code to
get an idea of how to modify fork-based child process creation into a spawn-based child process
creation. Note that using `posix_spawnp` makes life a little easier because it uses the `$PATH`
environment variable to find executables.

## Testing

One very important component of all software engineering is testing. Rather than doing our testing
for this assignment via the autograder right away, we will be showing you how you can test the full
functionality of your shell. You're encouraged to write your own test cases to determine whether your
code is working correctly.

## Using input redirection to test a shell

One important concept we'll go over this week is file redirection. In short, file redirection allows
you to read your program's input from a file, rather than from the keyboard.

For instance, if you have a file `input` that has the lines:

```
pwd
ls
date +%Y%m%d
```

You can feed it to a new instance of a shell program by redirecting the input from that file by
adding the `<` character as an argument, then the name of the file:

```
ckanich@home-desktop:~/repos/shell-skel$ sh < input 
```

> Suggested activity: Ask yourself what you expect will happen when you run `sh < input`, then
> create this file on your machine and feed it to the shell `sh` and see what happens.

In addition to using `sh`, you can also run the skeleton code for the assignment in the same way,
and see that it works properly.

> Ask yourself: how can you convince yourself that it is working properly? Hint: read the output of
> `man diff` and try to think of a way to use that tool to determine whether your shell is running
> properly (note: not that it's using posix_spawn, just that it is working as you expect it to).

## Receiving redirected input vs. redirecting input ourselves

Redirected input is a tricky topic. Every process has its own input and output files, and it doesn't
care whether they are connected to terminals, files, or other cool things like network connections.
By testing our shell _using_ input redirection, our shell doesn't need to _understand_ input
redirection. However, over the course of the next two weeks, you'll need to add input redirection to
your shell, so that, for instance, you could run a set of commands like this:

```
pwd
ls > list_of_files
cat < list of files
```

...and it would work the way that `sh` works when given that set of inputs.

## What you need to do for this lab

You can find your pairings for the lab in [this sheet]({{page.pairings}}).

The peer explanations for this lab are:

<!-- 1. Draw a before and after diagram of the contents of `buf`, `argc`, and `argv` when the user runs -->
   <!-- "echo hello world." -->
<!-- 2. Show your evaluator what you did to switch `fork` with `posix_spawnp` to run a child process. -->

| Session | Task | Points |
|---|---|---|
| Session A | Draw a before and after diagram of the contents of `buf`, `argc`, and `argv` when the user runs "echo hello world." | 1 point |
| Session B | Show your evaluator what you did to switch `fork` with `posix_spawnp` to run a child process. | 1 point |
{: class="table table-striped"}

Please grade your peers out of **1 point** using [this form]({{site.eval_link}}).
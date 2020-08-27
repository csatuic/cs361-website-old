---
title: homework 1
description: linux executable format
# liquid shortcoming: can't manipulate objects in templates that well,
# so you need to duplicate relevant info in the 'due' object for rendering in the entire 
# class schedule.
due:
  type: due
  title: homework 1
  date: 2020-09-02T23:59:00-5:00
  description: linux executable format
date: 2020-08-24
github_link: TODO
skeleton_repo: TODO
---

# Homework 1: The ELF format

This homework is meant to get you familiarized with the ELF
file format and the process of linking executables.

We covered using git in the lab, but if you're still not that familiar
with it, this assignment is a second chance to get up to speed. Remember: git
gives you warnings and errors _for good reason_, if it complains at the command
line when you run a command, don't just assume it's completed correctly!

The skeleton code for this assignment is available at [this
link]({{github_link}}). You must use GitHub classroom to
write your code and keep a commit log on GitHub. You can submit the code via
[Gradescope]({{site.gradescope}}).

Now that you have the skeleton code, you can start coding. You should commit
early and often, and push to your remote repository whenever is convenient to
back up your work!

## Programming environment

This assignment was created in an Ubuntu 18.04 based environment. The
skeleton code comes with a `devcontainer` setup script so that running
it in a remote container through Visual Studio Code will put you in the
same environment. This assignment is simple enough that any Linux
environment with an up to date `gcc` should be sufficient, including
`systems1.cs.uic.edu`. If you aren't/can't/don't want to complete this
assignment using vscode remote containers, feel free to complete this on
a lab machine, a local Linux VM, or elsewhere.

## The Programming Part!

This part will give you a quick introduction to using `readelf` to better
understand the linking process.

In this assignment, you must fill `hw1.c` and `hw1.h` with code which will:

1. cause your UIC netID (and nothing else) to be printed on the first line of output when the program is run.
3. cause `gcc -Wall hw1.c` to issue zero warnings (and zero errors, duh).
2. cause the output of `readelf -s hw1.o` to have an identical number of symbol table entries as shown below.
2. cause the output of `readelf -s hw1.o` to have identical values in the red+bold sections of the output below:

You can perform the symbol table check by running `make test` within the `hw1` directory. A full credit solution will have output that looks like:

Symbol table '.symtab' contains 25 entries:

|Num: |  Value  |       Size|Type |  Bind | Vis   |  Ndx|Name|
|---|---|---|---|---|---|---|---|
|     0: |0000000000000000|     0| NOTYPE|  LOCAL|  DEFAULT|  UND| |
|     1:| 0000000000000000|     0| FILE  |  LOCAL|  DEFAULT|  ABS| hw1.c|
|     2:| 0000000000000000|     0| SECTION| LOCAL|  DEFAULT|    1| |
|     3:| 0000000000000000|     0| SECTION| LOCAL|  DEFAULT|    3| |
|     4:| 0000000000000000|     0| SECTION | LOCAL |  DEFAULT|    4| |
|     5:| 0000000000000000|    11| **FUNC** |   **LOCAL** | DEFAULT    |**1**| **I_have_written**|
|     6:| 0000000000000000|    **12**| **OBJECT** |  **LOCAL** |  DEFAULT    |**3**| **the_code**|
|     7:| 000000000000000b|    26| **FUNC**|    **LOCAL** | DEFAULT    |**1**| **that_you_needed**|
|     8:| 0000000000000018|     **4**| **OBJECT** |  **LOCAL** | DEFAULT    |**3**| **to_compile.**2213|
|     9:| 0000000000000000|     0| SECTION | LOCAL | DEFAULT|    5|
|    10:| 0000000000000058|    78| **FUNC**|    **LOCAL** |  DEFAULT    |**1**| **and_which**|
|    11:| 000000000000001c|     **4**| **OBJECT** |  **LOCAL** | DEFAULT    |**3**| **has_a_bunch_of.**2220|
|    12:| 0000000000000020|     **4**| **OBJECT** |  **LOCAL** | DEFAULT    |**3**| **ridiculous.**2221|
|    13:| 0000000000000000|     **8**| **OBJECT** |  **LOCAL** | DEFAULT    |**4**| **symbols.**2222|
|    14:| 0000000000000000|     0| SECTION | LOCAL |  DEFAULT|    7|
|    15:| 0000000000000000|     0| SECTION | LOCAL  |DEFAULT|    8|
|    16:| 0000000000000000|     0| SECTION|  LOCAL|  DEFAULT|    6|
|	17:| 0000000000000000|     7| **FUNC**|    **GLOBAL**| DEFAULT|    **1**| **sides_and**|
|    18:| 0000000000000025|    61| **FUNC**|    **GLOBAL** |DEFAULT   | **1**| **main**|
|    19:| 0000000000000000|     **0** |**NOTYPE**  |**GLOBAL** |DEFAULT  |**UND**| **__GLOBAL__OFFSET__TABLE__**|
|    20:| 0000000000000000|     **0** |**NOTYPE**  |**GLOBAL** |DEFAULT  |**UND**| **printf**|
|    21:| 00000000000000a6|    47| **FUNC**|    **GLOBAL** |DEFAULT    |**1**| **Forgive_me**|
|    22:| 0000000000000004|     **4**| **OBJECT** |  **GLOBAL** |DEFAULT  |**COM**| **they_are_arbitrary**|
|    23:| 0000000000000010|     **8**| **OBJECT** |  **GLOBAL** |DEFAULT|    **3**| **so_random**|
|    24:| 00000000000000d5|    41| **FUNC**|    **GLOBAL** |DEFAULT|    **1**| **and_so_varied**|
{: class="table table-striped well texttt"}

Hints:

* Are you seeing `puts` instead of `printf`? Check the man pages for what the
  difference is, and make sure that the compiler can't optimize away your
  call to printf.  Remember, requirement #1 **only** mentions the **first**
  line of output.
* The four digit numbers do not need to made identical, but you do need to make
  them show up. Keep experimenting with different variable types until you find
  how to create variables with periods and numbers on them.
* Function lengths are very difficult to reproduce - note that for every FUNC,
  you do not have to duplicate the length (the length is the number of bytes of
  assembly code chosen by the compiler to execute the body of each function).


## Template 

There is a very bare bones skeleton and a Makefile provided for this assignment. The skeleton file is worth one point because it compiles with no warnings and no errors.

## Grading
Grading will be done automatically using Gradescope. Submitting to
GitHub is not sufficient - your code must be submitted to Gradescope. If
you have issues with the autograder, please contact us via Piazza ASAP.
**Technical issues with submitting your assignment is not an acceptable
excuse for submitting the assignment improperly or late.**

## Due Date
This assignment is due {{ page.due.date | date_to_rfc822 }}. See the
[syllabus](syllabus.html) for the late turnin policy. This assignment is
worth just as much as every other homework, but is easier than all the
rest (in our opinion) so getting as much credit on it as possible is
important!

## helpful documents

Chapter 7 in the book
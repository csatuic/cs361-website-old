---
title: homework 5
due: 
    type: due
    title: homework 5
    date: 2020-11-23T23:59:00-5:00
    text: Monday, November 23rd 11:59pm Chicago time
    description: 'Assignment #5 due'
date: 2020-11-13
github_link: https://classroom.github.com/a/lMNAawtQ
---

# {{page.title}}: A Home-Brew Web Server

## This homework is be due on {{page.due.text}}

This homework has the following learning objectives:
* Learn how to follow a network protocol (in this case, http)
* Learn network/socket programming

[Assignment checkout]({{page.github_link}})

In this homework, we'll look at the HTTP protocol as a Web server. After accepting the assignment, you will find that the `hw5` directory contains:

* `homework5.c`: Skeleton code for the server side of a TCP application. This will be the primary file for this assignment, but feel free to modularize and create other files if you prefer to do so.
* `WWW/`: A directory containing example files for your Web server to distribute. 
<!-- * `thread_example.c`: Example code that illustrates a very simple threaded programming scenario. You are not required to use or make any changes to this file, but you should understand what it does. -->
<!-- * `format_string.c`: A simple program that takes two arguments and prints a formatted string as the standard output. -->
* A `Makefile`. If you modularize your code into different files, make sure those changes are reflected in the Makefile (and don't forget to `git add` those files to your personal git repo!

Your server program will receive two arguments: 1) the port number it should listen on for incoming connections, and 2) the directory out of which it will serve files (often called the document root in production Web servers). For example:

```bash
$ ./homework5 5000 WWW
```

This command will tell your Web server to listen for connections on port 5000 and serve files out of the `WWW` directory. That is, the `WWW` directory is considered `/` when responding to requests. For example, if you're asked for `/index.html`, you should respond with the file that resides in `WWW/index.html`. If you're asked for `/dir1/dir2/file.ext`, you should respond with the file `WWW/dir1/dir2/file.ext`.

## Requirements

Your server should handle the following cases:

1. Complete the code inside the `main()` function to *create* a socket, *bind* it to an address and port, *listen* for client connections, and *accept* waiting connections.
1. Serve requested files out of the directory specified in the run-time arguments. For example, if the specified directory is `WWW` and the requested file is `/example.jpg`, you should respond with the file `WWW/example.jpg` if it exists. You need to return the correct HTTP Content-Type header for `jpeg` and `html` files so they can be rendered in the browser.
2. If the requested file does not exist, you should respond with a **404** error code and a readable error page containing some basic HTML. 
<!-- 3. If the path requested by the client is a directory, you should handle the request as if it was for the file `index.html` inside that directory. You do not need to handle the case where the directory does not contain `index.html` file. Hint: use the `stat()` system call to determine if a path is a directory or a file. The `st_mode` field in the stat struct has what you need. -->
3. **Common Gateway Interface (CGI)** is a protocol for a web server to serve dynamic content using command-line interface programs. As a part of the protocol, you should handle requests like `cgi-bin/hello-world.py?first_name=<str1>&last_name=<str2>` by executing the file `hello-world.py` with `QUERY_STRING` as the environment variable in a new child process and return the response produced by the file by duplicating the file descriptor.
<!-- and return the string produced by `hello-world.py` inside `WWW/cgi-bin` directory. -->
<!-- Besides returning the static content, you should also handle requests like `format_string?<str1>&<str2>` and return the formatted string to the client. For instance, if the request is `/format_string?test1&test2`, your program should run the `format_string` binary in the `WWW` directory with arguments `test1` and `test2`, and return the output to the client. -->

## Bonus Point

- Handling HTTP Content-Type headers for additional file types provided in the `WWW` directory, like `txt`, `gif`, `png`, and `pdf` will earn you 2 bonus points.
<!-- If your server handles the proper HTTP Content-Type header in the response based on the file ending, you will receive a bonus point. We will test `txt`, `gif`, `png`, and `pdf` file extensions. -->

## Important Note
Always, always, always **check the return value of any system calls you make!**. This is especially important for `send`, `recv`, `read`, and `write` calls that tell you how many bytes were read or written. Failing to do so can crash autograder giving you zero points.

When testing, you should be able to retrieve byte-for-byte copies of files from your server. Use `wget` or `curl` to fetch files and `md5sum` or `diff` to compare the fetched file with the original. We will grade using this method. *For full credit, the files need to be exact replicas of the original.*

## Grading Rubric

This assignment is worth **8 points** in total and **2 additional bonus points**:

* **2 points** for serving exact copies of HTML and JPEG files in the browser and using command line clients like `wget` or `curl`. The MD5 sums should match!   
 <!-- to command line clients like wget or curl.  -->
 <!-- The MD5 sums should match! -->
* **2 points** for correctly returning a 404 error code and HTML message when a request asks for a file that does not exist.
<!-- * **2 points** for serving `index.html`, if it exists, when asked for a file that is a directory. -->
* **4 points** for returning the plain formatted string given the request arguments.
* **Bonus: 2 points** for serving other files (that can be rendered correctly -- set Content-Type) to a standard Web browser (e.g., Firefox).

When submitting, you must provide a `Makefile` along with your program and ensure that when your program compiles, the executable's name is `homework5`. If you do not add any files to your source tree (and only update `homework5.c`), you should not need to change anything.

Grading will be done automatically using a script. We will publish this script after grading has completed; you are responsible for writing your own test cases. If you wish, you can share test cases you have written with the class.

## Tips

* Take compiler warnings seriously. Unless it's an unused variable, you should address the warning as soon as you see it. Dealing with a pile of warnings just makes things more difficult later.
* Test your code in small increments. It's much easier to localize a bug when you've only changed a few lines.
* If you need to copy a specific number of bytes from one buffer to another, and you're not 100% sure that the data will be entirely text, use `memcpy()` rather than `strncpy()`. The latter terminates early if it finds a null terminator ('\0').
* If you're trying to do some sort of specific string or memory manipulation, feel free to ask if there's a better/recommended way to do it rather than brute force. Often there may be a standard library function that will make things easier.
* Read chapter 11 (especially 11.5 and 11.6) from the textbook to learn more about the network programming in C.

Roughly, your server should follow this sequence:

1. Read the arguments, bind to the specified port, and find your document root (you might find the `chdir()` system call helpful).
2. Accept a connection, and hand it off to a new thread for concurrent processing.
3. Receive and parse a request from the client.
4. Look for the path that was requested, starting from your document root (the second argument to your program). One of four things should happen:
   1. If the path exists and it's a file, formulate a response and send it back to the client (bonus point for handling Content-Type header happens here).
   2. If the path exists and it's a directory that contains an `index.html` file, respond with that file.
   3. If the path does not exist, respond with a 404 code with a basic error page.
5. If the requested path contains `cgi-bin`, retrieve the URL parameters and create a new child process. Modify the file descriptor of the child process and return the formatted string as plain text (Hint: You can use `posix_spawn` to accomplish this).
6. Close the connection, and continue serving other clients.


## How to run your program
There are two ways to run and test your code:
1. We would share a spreadsheet assigning unique port numbers to each student to avoid collisions and you can run your code on `systemsX`.
2. The `devcontainer.json` has settings to forward a port from the container to your local machine. If you wish to change the forwaded port, you can change the port number in line `"forwardPorts": [<port-number>]`.

If you have any questions about the homework requirements or specification, please post on Piazza.

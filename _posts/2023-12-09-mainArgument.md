---
layout: single

title: "[Linux] Function arguments of main()"
excerpt: "argument count, argument vector"

categories:
  - Linux server

tag: [C/C++, linux, system developement] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


# Main function argument: ***argc, argv***

argv and argv are special function parameters used for pssing those into main() when we type something and executed on commend line.


## Argument count - argc

This variable indicates the number of command-line arguments when the program is executed. argc is an integer variable, and it should be at least 1 when the program is run. This is because the program's name itself is passed as the first argument.

## Argument vector - argv

This variable is a pointer array where each pointer points to a string representing a command-line argument. The first element of argv points to the program's name, and the subsequent elements point to additional command-line arguments passed during execution.

I will explain with a simple example. The following is the basic structure of a C program:

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    //programm codes
    ...
    return 0;
}
```

Here, the main function takes argc and argv as parameters. For example, let's assume the program is executed as follows:

```bash
./my_program arg1 arg2 arg3
```

- argc becomes 4 (including the program's name).

argv looks like this:
- argv[0]: "./my_program"
- argv[1]: "arg1"
- argv[2]: "arg2"
- argv[3]: "arg3"

>By the way -> *argc[] == **argc

In this way, the program can read the arguments passed on the command line.

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

# Creating Makefile

There are 3 files wating to be built as follows 

```console
ubuntu@ubuntu-vm:~/Work/matrix_example$ ls
main.c matrix.c  matrix.h
```
to generate .exe file without make utility, The procedure below is a bit cumbersome

## Building Makefile in primitive way

```console
all: main

main: main.o matrix.o
    gcc -o main main.o matrix.o

main.o: main.c
    gcc -c main.c

matrix.o: matrix.c
    gcc -c matrix.c

clean:
    rm -rf main.o matrix.o main
```

The given code allows us to generate the desired main.exe file by entering "make" in the command prompt. However, if changes occur in the files, we would have to tediously modify the makefile. To address this, we can use macros.

## Adding macro in Makefile for easy correction

```console
TARGET: main
OBJ: main.o matrix.o
CFLAGS = -c
CC: gcc

all: $(TARGET)

main: $(OBJ)
    $(CC) -o $(TARGET) $(OBJ)

main.o: main.c
    $(CC) $(CFLAGS) $<

matrix.o: matrix.c
    $(CC) $(CFLAGS) $<

clean:
    rm -rf *.c $(TARGET)
```

macro has been created. Now, let's try to reduce duplicated parts

## Reducing duplicates

```console
TARGET: main
OBJ: main.o matrix.o
CFLAGS = -c
CC: gcc

all: $(TARGET)

.o.c:
    $(CC) $(CFLAGS) $<

clean:
    rm -rf *.c $(TARGET)
```

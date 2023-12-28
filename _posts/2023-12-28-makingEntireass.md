---
layout: single

title: "[Linux] Generate preprocess files to inspect"
excerpt: "Let's create .s.i files for detail information"

categories:
  - Linux server

tag: [C/C++, linux, kernel] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

## Edit Makefile in linux dir

```bash 
root@raspberrypi:/project/linuxSrc/linux # vi Makefile
```
Find KBUILD_CFLAGS by typing below on vim

```bash 
:/KBUILD_CFLAGS
```
### Add -save-temps=obj to build .s.i files 

```cpp 
KBUILD_CFLAGS   :=  -Wall -Wundef -Werror=strict-prototypes -Wno-trigraphs \
                    /* ... skip ... */
                    -save-temps=obj \  /* ### ADD THIS CODE ### */
                    -std=gnu11
```

### build.sh 

```bash
root@raspberrypi:/project/linuxSrc # ./build.sh
```

Now you can see all files with extension .s and .i to inspect and analyze how kernel is most of which is made of assembly code.

### How to preprocess only 1 file to inspect

```bash
root@raspberrypi:/project/linuxSrc # vi build_preprocess.sh
```
type below code

```cpp
#!/bin/bash

echo "configure build output path"

KERNEL_TOP_PATH="$( cd "$(dirname "$0")" ; pwd -P)"
OUTPUT="$KERNEL_TOP_PATH/out"
echo "$OUTPUT"

KERNEL=kernel8
BUILD_LOG="$KERNEL_TOP_PATH/rpi_preprocess_build_log.txt"

PREPROCESS_FILE=$1

echo "build preprocess file: $PREPROCESS_FILE"

echo "move kernel source"
cd linux

echo "make defconfig"
make O=$OUTPUT bcm2711_defconfig

echo "kernel build"
make $PREPROCESS_FILE O=$OUTPUT -j4 2>&1 | tee $BUILD_LOG
```
Changing authorization and try to generate ***core.c*** file

```bash
root@raspberrypi:/project/linuxSrc # chmod 755 build_preprocess.sh
root@raspberrypi:/project/linuxSrc # ./build_preprocess.sh /kernel/sched/core.i
```

Now core.i can be seen made in subject directory in short time

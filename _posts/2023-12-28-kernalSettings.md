---
layout: single

title: "[Linux] First Steps for the Kernel"
excerpt: "Kernel Configuration Guide"

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


## Accessing root

```bash
pi@raspberrypi:~ $ passwd root
passwd: You may not view or modify password information for root.
pi@raspberrypi:~ $ sudo passwd root
New password:
Retype new password:
passwd: password updated successfully
pi@raspberrypi:~ $ su
Password:
root@raspberrypi:/home/pi#
```

changed to root dir, you don't need to sudo keyword everytime to install or access something. escape can be done by typing "exit" on shell.

## Install required pakages 

```bash 
root@raspberrypi:/# apt-get install git bc bison flex libssl-dev
```

## Making dir

```bash
root@raspberrypi:/# ls
bin   home            lost+found  proc  srv  var
boot  initrd.img      media       root  sys  vmlinuz
dev   initrd.img.old  mnt         run   tmp  vmlinuz.old
etc   lib             opt         sbin  usr
root@raspberrypi:/# mkdir project
root@raspberrypi:/# cd project
root@raspberrypi:/project# mkdir linuxSrc
root@raspberrypi:/project# cd linuxSrc
root@raspberrypi:/project/linuxSrc# 
```

## install raspberryPi kernel source code clone

```bash
root@raspberrypi:/project/linuxSrc# git clone --depth=1 http://github.com/raspberrypi/linux
```

>--depth=1 is a commit concept. If it is not used, it fetches the entire source code (all commit history). If used, it fetches only the content of the latest commit. Once this is done, a file named "Linux" is created.

## set colour on root@user

```bash
pi@raspberrypi:~ $ cd ~
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ sudo cp ./.bashrc /root/
```
```bash
root@raspberrypi:~# sourche .bashrc
```
>Change directory to home using "cd ~", then copy .bashrc to /root/ directory. This file is opened once when the "pi" user boots. By copying this file to root and opening it using the "source" command, it can also be utilized in the root environment.

## Set variable on Bash

```bash
root@raspberrypi:/project/linuxSrc/linux # KERNEL=kernel8
root@raspberrypi:/project/linuxSrc/linux # echo $KERNEL
kernel8
```

## Chipset config make file 

```bash
root@raspberrypi:/project/linuxSrc/linux # make bcm2711_defconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/confdata.o
  HOSTCC  scripts/kconfig/expr.o
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/menu.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTCC  scripts/kconfig/util.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
```

>After entering the above commands, running "make" tailors the build to our hardware SoC (System on Chip).


## making build.sh file

```bash
root@raspberrypi:/project/linuxSrc # vi build.sh
root@raspberrypi:/project/linuxSrc # ls
build.sh  linux
```

```cpp
#!/bin/bash

echo "configure build output path"

KERNEL_TOP_PATH="$( cd "$(dirname "$0")" ; pwd -P)"
OUTPUT="$KERNEL_TOP_PATH/out"
echo "$OUTPUT"

KERNEL=kernel8
BUILD_LOG="$KERNEL_TOP_PATH/rpi_build_log.txt"

echo "move kernel source"
cd linux

echo "make defconfig"
make O=$OUTPUT bcm2711_defconfig

echo "kernel build"
make O=$OUTPUT Image modules dtbs -j4 2>&1
```
 write down this onto build.sh file


 ## changing authorization using chmod

 ```bash
root@raspberrypi:/project/linuxSrc # ls
build.sh  linux
root@raspberrypi:/project/linuxSrc # chmod 755 build.sh
root@raspberrypi:/project/linuxSrc # ls
build.sh  linux
root@raspberrypi:/project/linuxSrc # ls -l
-rwxr-xr-x  1 root root  365 Dec 28 12:03 build.sh
 ```
 
 ## make mrproper before ./build.sh 

 ```bash
 
pi@raspberrypi:/project/linuxSrc/linux $ su
Password:
root@raspberrypi:/project/linuxSrc/linux # make mrproper
  CLEAN   scripts/basic
  CLEAN   scripts/kconfig
  CLEAN   include/config include/generated .config
root@raspberrypi:/project/linuxSrc/linux #
 ```

 ## ./build.sh

```bash
root@raspberrypi:/project/linuxSrc # ./build.sh
```

it takes long time to build 

## Kernel Install

```bash
root@raspberrypi:/project/linuxSrc # vi install.sh
root@raspberrypi:/project/linuxSrc # ls
build.sh  install.sh  linux  out
```
open install.sh, and type below code on it

```cpp
#!/bin/bash

echo "configure build output path"

KERNEL_TOP_PATH="$( cd "$(dirname "$0")" ; pwd -P)"
OUTPUT="$KERNEL_TOP_PATH/out"
echo "$OUTPUT"

KERNEL=kernel8

cd out

make 0=$OUTPUT modules_install
cp $OUTPUT/arch/arm64/boot/dts/broadcom/*.dtb /boot/
cp $OUTPUT/arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
#cp $OUTPUT/arch/arm64/boot/dtts/overlays/README /boot/overlays/
cp $OUTPUT/arch/arm64/boot/Image /boot/$KERNEL.img

```
and change authorization

```bash
root@raspberrypi:/project/linuxSrc # chmod 755 install.sh
root@raspberrypi:/project/linuxSrc # ./install.sh
```

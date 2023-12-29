---
layout: single

title: "[Linux] Structure of Linux kernel source"
excerpt: "Exploring kernel directories"

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

# Strcture of Kernel source


<p align="center"><img src="/assets/images/2023-12-29-linuxStructure/linux_structure.png"></p>



## arch

The "arch" directory contains architecture-specific code. For example, it includes code tailored for various architectures such as x86, ARM, MIPS, and others.

```bash
root@raspberrypi:/project/linuxSrc/linux # cd arch
root@raspberrypi:/project/linuxSrc/linux/arch # ls
alpha  csky     loongarch   nios2     riscv  um
arc    hexagon  m68k        openrisc  s390   x86
arm    ia64     microblaze  parisc    sh     xtensa
arm64  Kconfig  mips        powerpc   sparc
root@raspberrypi:/project/linuxSrc/linux/arch # cd arm64
root@raspberrypi:/project/linuxSrc/linux/arch/arm64 # ls
boot     include        Kconfig.platforms  Makefile  xen
configs  Kbuild         kernel             mm
crypto   Kconfig        kvm                net
hyperv   Kconfig.debug  lib                tools
root@raspberrypi:/project/linuxSrc/linux/arch/arm64 #
```
The compilation is specific to a particular architecture; for example, Raspberry Pi 4B is compiled using arm64 files for the bcm2711 SoC. As architectures vary between different SoCs, and assembly languages differ, it's crucial to identify one's hardware accurately and compile accordingly.

```bash
root@raspberrypi:/project/linuxSrc/linux # make bcm2711_defconfig
```
>running “make” tailors the build to our hardware SoC (System on Chip).

## other dirs

|dir|description|
|---|---|
|include|The necessary header files are located. |
|Doncumentation|The documentations for description are located.|
|mm|Memory-related, Virtual Memory, Paging-related|
|drivers|Sources related to drivers, essential knowledge for embedded developers. Source|
|fs|Code related to file systems, determining how files will be stored and managed in physical memory. Examples include FAT, NTFS.|
|lib|Kernel-level supported libraries are located here.|
|***kernel***|Here, essential codes are situated, and these codes are universally used regardless of hardware. Kernel-specific codes, which vary for different System-on-Chips (SoCs), are organized separately in the "arch" directory.|

## **kernel dirs**

|dir|description|
|---|---|
|irq|The codes related to interrupts are located here.|
|sched|The codes related to scheduling are located here.|
|power|This is the directory managing the kernel's power management.|
|locking|The codes for synchronization, mutex, semaphore, and similar constructs are located here.|
|printk|The codes used for outputting kernel logs are located here.|
|trace|Here are the codes related to ftrace.|


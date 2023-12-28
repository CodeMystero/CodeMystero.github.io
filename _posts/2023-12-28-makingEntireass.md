---
layout: single

title: "[Linux] Generate assembly files to analize and inspect"
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

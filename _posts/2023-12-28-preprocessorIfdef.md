---
layout: single

title: "[Linux] Preprocessor for selective compile"
excerpt: "#ifdef, #elif, #else, and #endif"

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


# Preprocessor for compiling with define -D

Let's create a simple example using ***#ifdef, #elif, #else, and #endif.*** Then, compile it using each definition and observe the results.


```cpp
#include <stdio.h>

#ifdef SUM
int func(int a, int b){
   return a+b;
}

#elif MUL
int func(int a, int b){
   return a*b;
}

#else
int func(int a, int b){
   return a-b;
}

#endif


int main(){

    int a = 10;
    int b = 20;

    printf("return >> %d\n",func(a,b));

    return 0;
}
```

#### The results for conditional compilation are different as follows:


```bash
root@raspberrypi: # gcc main.c -o main
root@raspberrypi: # ./main
return >> -10
```

```bash
root@raspberrypi: # gcc main.c -o main -DSUM
root@raspberrypi: # ./main
return >> 30
```

```bash
root@raspberrypi: # gcc main.c -o main -DMUL
root@raspberrypi: # ./main
return >> 200
```

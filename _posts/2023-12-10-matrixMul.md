---
layout: single

title: "[POS] Matrix multiplcation"
excerpt: "let's code matrix multiplication on Linux server"

categories:
  - Prosedure oriented programming

tag: [C/C++, linux, malloc, pointer] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

# main.c

The code to find the product of the matrix is divided into three files, **main.c matrix.c matrix.h**. those will be use to create a **Makefile** that we will look into later.

## Declaration of matrixes

Let's write a code that multiplies two matrices, A and B, and assigns the result to matrix C. We will use a double pointer to create a 2x2 matrix.

```cpp
int** a;
int** b;
int** c;
```


## Displaying time taken for the processing

Let's add a function to the main.c file that consols processing time. It allows us to see intuitively the advenced performance of the thread to be added later.

```cpp
#include <time.h>

clock_t start, finish;

start = clock();
...
finish = clock();

printf("processing time taken: %f(s)\n",((double)finish-start)/CLOCKS_PER_SEC);
```

```consol
processing time take: 0.000006(s)
```

# matrix.c

## matrix multiplication

The equation for the multiplication of the matrix is shown in Google. To create a function for it, double pointers to three matrices were received as factors, and length was also received as factors.

```cpp
int matrix_mul(int **src1, int **src2, int **dest, int len){

    for(int i = 0;i<len;i++){
        for(int j = 0;j<len;j++){
            dest[i][j] = 0;
            for(int k = 0;k<len;k++){
                dest[i][j] += src1[i][k]*src2[k][j];
            }
        }
    }
    return 0;
}
```
> Adding **return 0;** to all functions are to put error detection

## Output the value of the matrix randomly created

```cpp
int printf_matrix(int **src, char* name, int len){
    printf("=== %s matrix ===\n",name);
    for(int i = 0;i<len;i++){
        for(int j = 0;j<len:j++){
            printf("%d ",src[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```
## Initialization of matrixes

In the next step, code will be written to initialize matrices A, B, and C for calculating C. However, the purpose of writing this code is to use threads. To achieve that, the matrix sizes will exceed 1000x1000. Since the stack size won't be able to handle even one matrix, causing a core dump, matrices A, B, and C will be dynamically allocated.

Additionally, the matrix sizes will be obtained as command line arguments in the main function by adding the argc and argv parameters.

```cpp
int main(int argc,char** argv)
    if (argc == 2){
        len = atoi(argv[1]);
    }else{
        len = 10; //intialzing as 10 if no argument declared
    }

    matrix_Init(&a, &b, &c, len);
    ...
```
>***atoi()***: convert an ascii to an integer<br>#include <stdlib.h><br>int atoi(const char* nptr)


```cpp
void matrix_Init(int*** aa, int*** bb, int*** cc, int len){

    int** a;
    int** b;
    int** c;

    a = (int**)malloc(len*sizeof(int*));
    b = (int**)malloc(len*sizeof(int*));
    c = (int**)malloc(len*sizeof(int*));

    for(int i = 0;i<len;i++){
        a[i] = (int*)malloc(len*sizeof(int));
        b[i] = (int*)malloc(len*sizeof(int));
        c[i] = (int*)malloc(len*sizeof(int));
        for(int j = 0;j<len:j++){
            a[i][j] = random()%10;
            b[i][j] = random()%10;
        }
    }

    *aa = a;
    *bb = b;
    *cc = c;
}
```

Add a random seed using ***srandom()*** to add the code below so that an integer value of a different value is generated over time.

```cpp
#include <time.h>

int main(...){
    srandom(time(NULL))
    ...
}
```


# matrix.h


```cpp
int printf_matrix(int**,char*,int);
int matrix_Init(int***,int***,int***,int);
int matrix_mul(int**, int**, int**, int);
```








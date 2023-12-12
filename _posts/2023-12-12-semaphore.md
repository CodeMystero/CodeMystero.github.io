---
layout: single

title: "[Linux] Mutual exclusion with semaphore"
excerpt: "thread, syncronization"

categories:
  - Linux server

tag: [C/C++, linux, thread] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


# Asyncronization with semaphore

>Mutual Exclusion with Semaphores

#include &lt;semaphore.h&gt;

int sem_init(sem_t *sem, int pshared, unsigned int value);

int sem_wait(sem_t *sem);

int sem_post(sem_t *sem);

int sem_destroy(sem_t *sem);

```cpp
#include <semaphore.h>
int count;
sem_t bin_sem; // global variable

int main(){
    sem_init(&bin_sem, 0, 1);
    pthread_t a_thread, b_thread;

    pthread_create(&a_thread, NULL, func1, NULL);
    pthread_create(&b_thread, NULL, func2, NULL);  

    sem_destroy(&bin_sem);  
}

void *func1(void* arg){
    for(int i = 0;i<10000;i++){
        sem_wait(&bin_sem);
        count++;//Critical section !!!!!!
        sem_post(&bin_sem);
    }
}

void *func2(void* arg){
    for(int i = 0;i<10000;i++){
        sem_wait(&bin_sem);
        count--;//Critical section !!!!!!
        sem_post(&bin_sem);
    }
}
```

The following function is a very simple example that uses semaphores to eliminate collisions between two threads. The key point here is that the two threads perform operations independently of each other, without caring about the order. If the two threads need to proceed in a specific order, the code should be written using two flags, allowing each thread to raise the other's flag in a coordinated manner.

# Syncronization with semaphore

```cpp
#include <semaphore.h>
int count;
sem_t bin_sem; // global variable
sem_t bin_sem_th1;
sem_t bin_sem_th2;
int main(){
    sem_init(&bin_sem, 0, 1);
    sem_init(&bin_sem_th1, 0, 1);
    sem_init(&bin_sem_th2, 0, 0);
    pthread_t a_thread, b_thread;

    pthread_create(&a_thread, NULL, func1, NULL);
    pthread_create(&b_thread, NULL, func2, NULL); 

    sem_destroy(&bin_sem);    
}

void *func1(void* arg){
    for(int i = 0;i<10000;i++){
        sem_wait(&bin_sem_th1);
        sem_wait(&bin_sem);
        count++; //Critical section !!!!!!
        sem_post(&bin_sem);
        sem_post(&bin_sem_th2);
    }
}

void *func2(void* arg){
    for(int i = 0;i<10000;i++){
        sem_wait(&bin_sem_th2);
        sem_wait(&bin_sem);
        count--;//Critical section !!!!!!
        sem_post(&bin_sem);
        sem_post(&bin_sem_th1);
    }
}
```
Now, the two threads, func1 and func2, are invoked and processed sequentially. The speed is slower compared to asynchronous semaphores.

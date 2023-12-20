---
layout: single

title: "[Linux] Shared memory"
excerpt: "Shared memory to be accessed by multi-processing"

categories:
  - Linux server

tag: [linux, ubuntu] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


## Declare an integer variable for sharing among processes.

```cpp
int shmid;
int *sharedVariable;
```

shmid is used to be allocated shared memory id from shmget() function. sharedVariable is the variable that will be shared from multi processing.

## Get shared memory space 

```cpp
if((shmid = shmget(IPC_PRIVATE, sizeof(int), IPC_CREAT|0666)) == -1){
    perror("shmget");
    exit(1);
}
```

>int shmget(key_t <u>key</u>, size_t <u>size</u>, int <u>shmflg</u>);

- key: A unique key used to identify the shared memory segment, typically generated using the ftok function associated with a file.

- size: The size of the shared memory to be created or retrieved.

- shmflg: Flags that control various options for the shared memory. It is commonly used with IPC_CREAT to create a new segment or with IPC_EXCL to ensure that the segment does not already exist.

The shmget function returns the identifier of the shared memory segment upon success and -1 on failure.


## Attaching process onto memory segment 

```cpp
sharedVariable = shmat(shmid, NULL, 0);
```

>void *shmat(int <u>shmid</u>, const void <u>*shmaddr</u>, int <u>shmflg</u>);

- shmid: Identifier of the shared memory segment created by the shmget function.

- shmaddr: Used to specify the desired memory address. Typically, NULL is used, allowing the kernel to choose a suitable address.

- shmflg: Flags, usually set to 0.

The shmat function returns the starting address of the attached memory segment upon success and (void *)-1 on failure.



> ***NOW, sharedVariable is shared on multi processes***

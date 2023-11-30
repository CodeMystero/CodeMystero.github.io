---
layout: single

title: "[OOP] Smart pointer"
excerpt: "unique_ptr, shared_ptr, weak_ptr"

categories:
  - Object oriented programming

tag: [C/C++, OOP, pointer, memory] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---


![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

## unique_ptr

We could easily allocate space on heap memory by ***new*** keyword. the drawback of the keyword possibly triggers memory leak by overlooking ***delete*** keyword to eliminate allocated space on the momey. this issue developer describing tends to become more prominent as the code becomes more complex. available sicne C++ 11, 2011.
```cpp
/*pseudo code for new keyword*/
datatype* ptr = new datatype(initialization)

/*now we can use smart pointer for audo delete*/
#include <memory>

int main(){
    unique_ptr<int> ptr(new(initialization))
}
/*if main is done, ptr cleared too(delete)*/
```
if wants to make class memory using smart pointer that can be described as follows
```cpp
#include <memory>
#include <iostream>
using namespace std;

class test {
public:
    test() {}
    ~test() {}
};

int main() {
    unique_ptr<test> ptr(new test());
}
```

### unique_ptr, once it made, *doesn not allow* another pointer points to the location

#### .relesaee(), move()

Unique_ptr," as indicated by the presence of the word "unique," means that multiple pointers cannot point to the same memory. If we want to transfer the ownership of the memory held by ptr_1 to another pointer ptr_2, we can use the *.release()* keyword.

```cpp
unique_ptr<test> ptr_1(new test());
unique_ptr<test> ptr_2(ptr_1.release());
//alternative
ptr_2 = move(ptr_1);
```

>When ownership is transferred for both ways, ptr_1 no longer points to that memory. If you don't use the .release() keyword, there is no way to transfer ownership (shallow copy) without it.

#### .reset()

If desired, you can also change what object ptr_1 is pointing to by assigning it to a different object

```cpp
unique_ptr<test> ptr_1(new test());
ptr_1.recet(new int(5));
// another object dynamic allocation, can be with class object as well
```

## shared_ptr

shared_ptr enables multiple pointers to point to the same object. It tracks the reference count of the shared memory, ensuring how many pointers are currently sharing it. When the reference count drops to zero, indicating that the memory is no longer referenced, shared_ptr safely releases the memory. This feature helps prevent memory leaks and efficiently manages resources

```cpp
shared_ptr<test> ptr_1 = make_shared<test>();
//or
shared_ptr<test> ptr_1(new test());
```

## weak_ptr
A *weak_ptr* allows pointing to an arbitrary object without ownership. *weak_ptr* is commonly used to point to an object owned by a *shared_ptr*. It does not increase the reference count of *shared_ptr* and simply serves the role of referencing the object that the shared_ptr is pointing to. Thus, even when the reference count of a shared_ptr becomes 0, the object doesn't disappear


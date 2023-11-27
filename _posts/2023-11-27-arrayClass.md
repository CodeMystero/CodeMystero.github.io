---
layout: single

title: "[OOP] []operator, using array"
excerpt: "adding idx error detection, append"

categories:
  - Object oriented programming

tag: [C/C++, OOP, array, error_detection, class, operator] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


## Definition of ArrTest class 
The following class is a class that constructs an array. The member variables include the address and size of the array.

```cpp
class ArrTest {
private:
    int *arr;
    int len;
public: 
    ArrTest(int size) :len(size) {
        arr = new int[size]; 
    }
    ~ArrTest() {
        delete arr;  // deleted
    }
    int length();
    int& operator[](int idx);
    ArrTest& append(int size);
};
```

## Error detection for calling out of boundary index of arr
The following function is code designed for detecting errors. When an index outside the declared size of the array is called, it triggers an error message
```cpp
int& ArrTest::operator[](int idx) {
    if (idx < 0 || idx >= len) {
        // exit(1) can terminate program if there is error
        std::cout << "Out of bound" << std::endl; //exit(1);
        //return -1; can return any number '-1'
    }
    //return *(arr + idx); also can be used
    return arr[idx];
}
```
## Return length of array 

```cpp
int ArrTest::length(){
    return len;
}
```


## Adding arr.append() function
### array.append(size)
below code does not append materials of array, ***different to Python's .append()***, but append additional heap memory only so that subject array is ready to get additional contents to be included. appending contents to empty index of array can be done using *for* or *while* loop. ***Or there is another solution for this which is overloading***

```cpp
ArrTest& ArrTest::append(int size) {
    int* arr1 = new int[len+size];
    memcpy(arr1, arr, len * sizeof(int));
    delete arr;
    arr = arr1;
    len += size;
    return *this;
}
```

### array.append(array) // overloading
The following code initializes the array with the desired values without the need for another value assignment.

```cpp
ArrTest& ArrTest::append(ArrTest& brr) {
    int *arr1 = new int[this->len + brr.length()];
    memcpy(arr1, arr, len * sizeof(int));
    memcpy(arr1 + len, brr.arr, brr.length() * sizeof(int));
    /*Can be replaced as below if no want memcpy()
    for (int i = 0; i < this->len; i++) { arr2[i] = arr[i]; }
    for (int i = this->len; i < new_length; i++) { 
        arr2[i] = arr1[i-(this->len)]; 
    }*/
    delete arr; 
    this->arr = arr1;
    len += brr.length();
    return *this;
}
```
#### MEMCPY()
>memcpy is a function in the C programming language (and also in C++) that stands for "memory copy." It is used to copy a block of memory from one location to another. if size is the number of index, size × sizeof(data type of array) has to be done to input to n of memcpy()

```cpp
void *memcpy(void *dest, const void *src, size_t n);
```

 The function takes three parameters:
 
|||
|---|---|
|dest|A pointer to the destination array where the content is to be copied|
|src|A pointer to the source of data to be copied|
|n|The number of bytes to copy|



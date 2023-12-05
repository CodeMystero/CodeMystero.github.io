---
layout: single

title: "[OOP] STL Iterator"
excerpt: "access to the elements of container"

categories:
  - Object oriented programming

tag: [C/C++, OOP, template, container, iterator] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


# Iterator for container
- It allows abstract access to the container, eliminating the need to know the type or implementation details of the container.
- Can be used similar to pointers.
- Most containers can be traversed through using iterators.
- Iterator needs to be made as object since it is class. 

## Begin and End of iterator

![begineendIterator](https://www.programiz.com/sites/tutorial2program/files/vector-iterator.png)

### Example of iterator in code

```cpp
#include <iostream>
#include <vector>
#include <set>

int main(){
    std::vector<int> vec{1,2,3};

    for (std::vector<int>::iterator it = vec.begin(); it != vector.end(); it++){
        std::cout << *it << std::endl;
    }
}
```
```console
1
2
3
```
### Auto keyword

- auto keyword is used for automatic type inference.
- Excessive use may compromise readability.

```cpp
int add(int a, int b){
    return a+b;
}

int main(){
    int a = 5;
    auto b = a; // b -> int
    auto c = add(a,b); // c -> int 
}
```

Now we can use auto to reduce declaration of iterator for the container as below

```cpp
    for (auto it = vec.begin(); it != vector.end(); it++){
        std::cout << *it << std::endl;
    }
```

## Operator of iterator 

| Operator              | Description                                       |
|-----------------------|---------------------------------------------------|
| `*it`                 | Dereference the iterator, obtaining the value at the current position |
| `it->member`          | Access a member of the object at the current position |
| `it++` or `++it`      | Move to the next position and return the iterator |
| `it--` or `--it`      | Move to the previous position and return the iterator |
| `it + n`              | Move n positions ahead from the current position and return the iterator |
| `it - n`              | Move n positions back from the current position and return the iterator |
| `it1 == it2`          | Return true if two iterators point to the same position |
| `it1 != it2`          | Return true if two iterators point to different positions |
| `it1 < it2`           | Return true if the position pointed to by it1 is before the position pointed to by it2 |
| `it1 > it2`           | Return true if the position pointed to by it1 is after the position pointed to by it2 |
| `it1 <= it2`          | Return true if the position pointed to by it1 is on or before the position pointed to by it2 |
| `it1 >= it2`          | Return true if the position pointed to by it1 is on or after the position pointed to by it2 |

### Example of iterator operator

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Declaration of iterator
    std::vector<int>::iterator it = numbers.begin();

    // element print using iterator
    std::cout << "Vector elements: ";
    while (it != numbers.end()) {
        std::cout << *it << " ";
        ++it;  // iterator move to next element of vector
    }
    std::cout << std::endl;

    // == operator for iterator
    std::vector<int>::iterator it1 = numbers.begin();
    std::vector<int>::iterator it2 = numbers.end();

    if (it1 == it2) {
        std::cout << "The iterators point to the same position." << std::endl;
    } else {
        std::cout << "The iterators point to different positions." << std::endl;
    }

    return 0;
}
```




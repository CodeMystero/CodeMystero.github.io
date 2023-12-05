---
layout: single

title: "[OOP] STL containers"
excerpt: "characteristics of container class"

categories:
  - Object oriented programming

tag: [C/C++, OOP, template, container] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

# Standard template library

- A collection of containers implemented using templates
- Object for storing data
- Commonly used data structures and algorithms are implemented

## Componenets

|Key word|Description|
|---|---|
|***Container***|- a collection of objects or primitive data types.<br> - array, vector, deque, stack, set, map, etc.<br> - Each container requires an associated header file.<br> - #include &lt;container&gt;|
|***Algorithm***|- Algorithm to handle the elements of container<br> - find, max, count, accumulate, sort, etc.|
|***Iterator***|- Iteration or accessment for the elements of container<br> - forward, reverse, by value, by reference, contant, etc.|

## Types of containers

|Container|Description|
|---|---|
|***Sequential***|- Container that maintains insertion order<br> - array, vector, list, forward_list, deque|
|***Associative***|- Container that stores elements without a specific order or according to a predetermined rule.<br> - set, multi set, map, multi map|
|*Adaptor*|- "Variations or applications of containers.<br> - stack, queue, priority queue|

### Member functions of containers

| Container            |Member Functions                   |
|----------------------|-------------------------------------------|
| `std::vector`        | `push_back`, `pop_back`, `size`, `empty`, `front`, `back`, `begin`, `end`, `insert`, `erase` |
| `std::list`          | `push_back`, `push_front`, `pop_back`, `pop_front`, `size`, `empty`, `front`, `back`, `begin`, `end`, `insert`, `erase` |
| `std::deque`         | `push_back`, `push_front`, `pop_back`, `pop_front`, `size`, `empty`, `front`, `back`, `begin`, `end`, `insert`, `erase` |
| `std::queue`         | `push`, `pop`, `front`, `back`             |
| `std::stack`         | `push`, `pop`, `top`                       |
| `std::set`           | `insert`, `erase`, `find`, `size`, `empty`, `begin`, `end` |
| `std::map`           | `insert`, `erase`, `find`, `operator[]`, `size`, `empty`, `begin`, `end` |
| `std::unordered_set` | `insert`, `erase`, `find`, `size`, `empty`, `begin`, `end` |
| `std::unordered_map` | `insert`, `erase`, `find`, `operator[]`, `size`, `empty`, `begin`, `end` |

>Those are member function of each container class

### Characteristics of the container
- The elements are copied and stored in the container.
- "If you want to store user-defined data types in a container, the ***copy constructor*** must be available for copying and assignment. If the member variables include pointers, a ***Assignment constructor*** is essential.
- Associative containers, operator>, operator<, and operator== are required.


#### Sequential container

##### std::array

std::array is a fixed-size sequential container that stores elements of the same type. It provides a convenient and safer alternative to traditional arrays. The size of a std::array is determined at compile-time.

```cpp
#include <iostream>
#include <array>

int main() {
    // Declare and initialize a std::array of integers with size 5.
    std::array<int, 5> myArray = {1, 2, 3, 4, 5};

    // Access and print elements.
    for (const auto& element : myArray) {
        std::cout << element << " ";
    }

    std::cout << std::endl;

    return 0;
}

```

```cpp
1 2 3 4 5
```

##### std::vector

std::vector is a dynamic-size array-like container. It automatically manages memory and allows for dynamic resizing. Elements can be efficiently added or removed from the end.

```cpp
#include <iostream>
#include <vector>

int main() {
    // Declare and initialize a std::vector of integers.
    std::vector<int> myVector = {1, 2, 3, 4, 5};

    // Add an element to the end.
    myVector.push_back(6);

    // Access and print elements.
    for (const auto& element : myVector) {
        std::cout << element << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
1 2 3 4 5 6
```

##### std::list

std::list is a doubly-linked list container. It allows for efficient insertion and removal of elements at any position. However, it may have slightly higher memory overhead compared to std::array and std::vector.

```cpp
#include <iostream>
#include <list>

int main() {
    // Declare and initialize a std::list of integers.
    std::list<int> myList = {1, 2, 3, 4, 5};

    // Insert an element at the beginning.
    myList.push_front(0);

    // Access and print elements.
    for (const auto& element : myList) {
        std::cout << element << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
0 1 2 3 4 5
```

#### Associative container

##### std::set

std::set is an ordered container that stores unique elements. The elements in a std::set are always sorted in ascending order, and each element must be unique.

```cpp
#include <iostream>
#include <set>

int main() {
    // Declare and initialize a std::set of integers.
    std::set<int> mySet = {5, 2, 1, 3, 4};

    // Insert an element into the set.
    mySet.insert(6);

    // Access and print elements.
    for (const auto& element : mySet) {
        std::cout << element << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
1 2 3 4 5 6
```

##### std::map

std::map is an associative container that stores key-value pairs. It allows efficient lookups and retrieval of values based on their corresponding keys.

```cpp
#include <iostream>
#include <map>

int main() {
    // Declare and initialize a std::map of string keys and integer values.
    std::map<std::string, int> myMap = {{"one", 1}, {"two", 2}, {"three", 3}};

    // Insert a new key-value pair into the map.
    myMap["four"] = 4;

    // Access and print key-value pairs.
    for (const auto& pair : myMap) {
        std::cout << pair.first << ": " << pair.second << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
four: 4 one: 1 three: 3 two: 2
```


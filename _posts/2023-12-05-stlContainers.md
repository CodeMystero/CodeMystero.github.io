---
layout: single

title: "[OOP] STL Container"
excerpt: "examples of priority containers"

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

# Sequential container

## std::array

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

## std::vector

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

## std::list

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

# Associative container

## std::set

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

## std::map

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

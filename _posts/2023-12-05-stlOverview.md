---
layout: single

title: "[OOP] Standard Template Library"
excerpt: "container, iterator, algorithm"

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

# Algorithm of STL

- An algorithm that operates on a set of elements specified by iterators such as find, max, accumulate, etc.
- Link for algorithm in template [Link for algorithms](https://en.cppreference.com/w/cpp/algorithm).
- In situations where passing a function as an argument to an algorithm can occur.
-> *Functor*, *Function pointer*, *Lambda*

## Iterator and algorithm

- #include &lt;algorithm&gt;
- Unlike iterators, the algorithms applicable to each container can vary.
- All algorithms require iterators as arguments.

> Validity of iterator: subject element if deleted, iterator is not validated

## Examples of algorithm

### find()

find does not require function argument.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    // Create a vector of integers
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Define a value to search for
    int target = 3;

    // Use std::find to search for the value in the vector
    std::vector<int>::iterator it = std::find(numbers.begin(), numbers.end(), target);

    // Check if the value was found
    if (it != numbers.end()) {
        std::cout << "Value " << target << " found at index " << std::distance(numbers.begin(), it) << std::endl;
    } else {
        std::cout << "Value " << target << " not found in the vector" << std::endl;
    }

    return 0;
}
```

```console
Value 3 found at index 2
```

### for_each()

- std::for_each function calls a given function for each element within the container as an argument.
- method to pass a function as argument.
-> *Functor*, *Function pointer*, *Lambda expression*

#### Functor

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Functor class definition.
class SquareFunctor {
public:
    // Overloading () operator to define the code executed when the functor is called.
    void operator()(int& element) const {
        // Squaring each element.
        element = element * element;
    }
};

int main() {
    // Creating a vector of integers.
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Creating an instance of the SquareFunctor.
    SquareFunctor squareFunctor;

    // Using std::for_each to apply the functor to each element.
    std::for_each(numbers.begin(), numbers.end(), squareFunctor);

    // Printing the transformed result.
    std::cout << "Transformed numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
Transformed numbers: 1 4 9 16 25
```

#### Function pointer

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Definition of the function to be passed as a function pointer.
void squareElement(int& element) {
    element = element * element;
}

int main() {
    // Create a vector of integers.
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Declare a function pointer and initialize it to point to the squareElement function.
    void (*functionPointer)(int&) = squareElement;

    // Use std::for_each to apply the function pointer to each element.
    std::for_each(numbers.begin(), numbers.end(), functionPointer);

    // Print the transformed result.
    std::cout << "Transformed numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
Transformed numbers: 1 4 9 16 25
```

- `void`: Indicates that the return type of the function pointed to by the function pointer is `void`.
- `(*functionPointer)`: Indicates that the name of the function pointer is `functionPointer`. The `*` denotes the declaration of a pointer.
- `(int&)`: Indicates that the function pointed to by the function pointer takes an `int` type reference as an argument.
- `= squareElement;`: Initializes the function pointer to point to the `squareElement` function.


#### Lambda expression (anonymous expression)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    // Create a vector of integers.
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Lambda expression to square each element.
    std::for_each(numbers.begin(), numbers.end(), [](int& element) {
        element = element * element;
    });

    // Print the squared result.
    std::cout << "Squared numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }

    std::cout << std::endl;

    return 0;
}
```

```cpp
Squared numbers: 1 4 9 16 25
```

- `[](int& element)`: This is a lambda expression, denoted by `[]`, which captures no external variables. Inside the parentheses, it declares a lambda function taking an `int&` parameter named `element`.
- `{ element = element * element; }`: The body of the lambda function, enclosed in curly braces `{}`, performs the operation of squaring the `element` by multiplying it with itself.


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

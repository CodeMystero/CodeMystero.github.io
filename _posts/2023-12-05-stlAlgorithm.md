---
layout: single

title: "[OOP] STL Algorithm"
excerpt: "access to the elements of container"

categories:
  - Object oriented programming

tag: [C/C++, OOP, template, container, iterator, lambda] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

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


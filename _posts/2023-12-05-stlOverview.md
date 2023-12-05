---
layout: single

title: "[OOP] Standard template library"
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






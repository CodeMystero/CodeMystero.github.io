---
layout: single

title: "[OOP] Generic programming"
excerpt: "template, generic class T"

categories:
  - Object oriented programming

tag: [C/C++, OOP, template] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


## Generic programming

> Methodology for writing code that works regardless of data types

Depending on the language, using generic programming allows writing generic code that operates on a variety of data types. Generics enable the determination of types at *compile time*, enhancing code reusability.

### Macro generic programming (#define)

Let's create a MAX function that takes two values, an integer and a floating-point number, and returns the larger of the two.

```cpp
int MAX(int a, int b){
    return a>b?a:b;
}

double MAX(double a, double b){
    return a>b?a:b;
}
```

However, if a function for comparing strings is needed, we would have to add that function as well. By using the define statement, we can elegantly address this without the need for excessive function overriding

```cpp
#define MAX(a,b) a>b?a:b
```
Using preprocessor macros is indeed very convenient. However, since they are replaced by the compiler within the code, issues may arise depending on the context. To minimize confusion, it's advisable to use parentheses liberally, especially considering the potential complexities in pre-processor substitutions.

Additionally, preprocessor macros have two main issues.
- Debugging preprocessor macros is challenging
- preprocessor macros are declared globally since they have no namespaces, leading strange error

## Generic programming using template

 Templates provide a blueprint for creating generic classes and functions, allowing the compiler to generate specific implementations based on the provided data types


### Function template


Let's write the MAX function using templates.

```cpp
template <typename T>
T MAX(T a, T b){
    return a>b?a:b;
}
```

Now, the above code will generate appropriate code during compilation for any data type we input, returning the desired value. at the line of ***typename T***, It does not need to be T, but can be any character or string such as ***"ABC"***. normally it is T, a few prefers U instead of it.

this is how to call the function with tymename T
```cpp
MAX<int>(a,b);
MAX<double>(a,b);
MAX<char>(a,b);
MAX<point>(a,b);
...
```

The compiler generates the actual function code based on the data type specified within the angle brackets. If the data type can be inferred, it is also possible to omit the data type inside the angle brackets. <br><br>
When only the template code exists, the compiler does not generate the actual function code during compilation. It is instantiated only when actually used in the program.

#### Multiple parameters in templates

```cpp
template <typedate T, typedata U>
void show(T a, U b){
  return a>b?a:b;
}

int main(){
  show<int,char>(1, 'a');
  show<int,float>(1, 1.12);
  ...
}
```

 If the data type can be inferred, it is also possible to omit both data type inside the angle brackets.

```cpp
show(1,'a');
show(1,2.1);
...
```

#### Specialization of template

Let's compate two strings using max function to compute longer word.

```cpp
#inclue <string>

template <typename T>
T MAX(T a, T b){
    return a>b?a:b;
}

int main(){
  std::string s1 = "star";
  std::string s2 = "apple";
  std::cout << MAX<std::string>(s1,s2); << std::endl;
}
```

The code will return 

```console
star
```

The developer wanted to return the longer word "apple," but the compiler returned the word "star" because it starts with a letter that comes later in the alphabet.

we can specialize the code to compute length of string. 

```cpp
#inclue <string>

template <typename T>
T MAX(T a, T b){
    return a>b?a:b;
}

template <>
std::string MAX(std::string a, std::string b){
  return a.length()>b.length()?a:b;
}

int main(){
  std::string s1 = "star";
  std::string s2 = "apple";
  std::cout << MAX<std::string>(s1,s2); << std::endl;
}
```

now we can have the result below

```consol
apple
```

in this case, we must be cafule

```cpp
MAX(s1,s2); // returns star
MAX<std::string>(s1,s2) // returns apple
```

### Class template

The compiler generates appropriate classes based on the type. The fundamental principle is similar to the use of function templates

this is example how to use class template

```cpp
#include <iostream>
#include <string>

template <typename T>
class employee{
private:
  std::string name;
  T age; 
public:
  employee(std::string name, T age)
    :name(name), age(age){}
  
  void get_name () const{
    return name;
  }

  T get_age() const{
    return age;
  }
};

int main(){
  employee<int> jack("jack", 21); 
  employee<std::string> wild("wild", "fourty-four");
}
```

>type declaration <data type> ***used to be inevitable*** for class template. In recent versions of C++, if omitted, ***the compiler can infer the types***.


multiple parameter of generic template also possible on class

```cpp
template <typename T, typename U>
class employee{
private:
  T name;
  U age; 
...}
```

specialization of parameter can be described as below if we want rounding float type of age made

```cpp
template <typename T, typename U>
class employee{
private:
  T name;
  U age; 
...}

template <typename T>
class employee<T,double>{
private:
  T name;
  U age;
public:
  employee(T name, double age)
    :name(name){
      this->age = round(age);
    } 
...}
```

#### Class template parameters

```cpp
template<typedata T, int N>
class array{
public:
  int size = N;
  T value[N];
}

int main(){
  array<char, 10> arr;
}
```

The created array (arr) is as follows:

```cpp
char arr[10];
```

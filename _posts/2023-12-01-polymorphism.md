---
layout: single

title: "[OOP] Polymorphism - Dynamic binding"
excerpt: "interface, virtual and interface"

categories:
  - Object oriented programming

tag: [C/C++, OOP, inheritance, override, pointer, virtual, interface] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

# Dynamic binding (Run Time Polymorphism)
>***Static binding*** relies on the type declared at ***compile time*** to determine the function call, whereas ***dynamic binding*** relies on the data type allocated in memory at ***run time*** to determine the function call.

### 3 conditions for dynamic binding
- Inheritance 
- Pointer or reference of base class
- Vitual function (override in derived function)

<br>

>***Override*** is the object-oriented programming concept where a child class redefines a method that is already defined in its parent class, using the same method signature.

<br>

## Dynamic binding in inheritance

### Example of dynamic binding

>Example UML

<div class="mermaid"> 
classDiagram
      SmartPhone <|-- Samsung
      SmartPhone <|-- Apple
      Apple <|-- iPhone_15_Max
      SmartPhone: -int price
      SmartPhone: ...
      SmartPhone: +Take_a_picture()
      class Samsung{
          -string camera
          ...
          +Take_a_picture()
      }
      class Apple{
          -string camera
          ...
          +Take_a_picture()
      }
      class iPhone_15_Max{
          -string advanced_camera
          ...
          +Take_a_picture()
      }
</div>

The UML described above can be organized into code as follows.

```cpp
#include <iostream>
#include <string>
#include <pointer>

class SmartPhone{
private:
  int price;
public:
  virtual void take_a_picture();
}

class Samsung : public SmartPhone{
private:
  string camera;
public:
  virtual void take_a_picture() override;
}

class Apple : public SmartPhone{
private:
  string camera;
public:
  virtual void take_a_picture() override;
}

class iPhone_15_Max : public Apple{
  private:
  string camera;
public:
  virtual void take_a_picture() override;
}

int main(){
  SmartPhone iphone15Max = new iPhone_15_Max();
  shared_ptr<SmartPhone> iphoneSe4 (new Apple());
  shared_ptr<SmartPhone> galaxy_s23 = make_shared<Samsung>();
  //The three methods mentioned above all involve creating memory for three different classes using the data type of SmartPhone. All three approaches use pointers in a somewhat similar manner. please refer to Smartpointer on this blog

  iphone15Max.take_a_picture() 
  // iPhone_15_Max::take_a_picture()

  iphoneSe4.take_a_picture() 
  // Apple::take_a_picture()

  galaxy_s23.take_a_picture() 
  // Samsung::take_a_picture()
}
```
The *virtual* or *override* keyword can be omitted when overriding a function in an derived class, especially when the base class already declares the function as virtual. However, for the sake of readability and clarity for the reader, it is advisable to retain the *virtual* and *override* keyword.<br><br>

### Virtual for destructor
Let's see example first

```cpp
class base{
public:
  ~base(){
    cout<< "base destructor"<<endl;
  }
}

class derived : public base{
public:
  ~derived(){
    cout<< "derived destructor"<<endl;
  }
}

int main(){
  base ptr = new derived(); 
  delete ptr;
}
```

If we write code like the above to use dynamic binding, the console will display the following:

> base detructor 

That implies that the memory of the derived class still remains in memory, leading to a memory leak. we can correct the code as below

```cpp
class base{
public:
  virtual ~base(){
    cout<< "base destructor"<<endl;
  }
}

class derived : public base{
public:
  virtual ~derived(){
    cout<< "derived destructor"<<endl;
  }
}

int main(){
  base ptr = new derived(); 
  delete ptr;
}
```
consol out:
>base dectructor<br>derived destructor

If a class is intended for inheritance and dynamic binding, it is advisable to declare a virtual destructor for safety.


## Pointer or reference for class in Dynamic binding

### Example of base pointer 

```cpp
SmartPhone ptr_1 = new SmartPhone();
SmartPhone ptr_2 = new Samsung();
SmartPhone ptr_3 = new Apple();
SmartPhone ptr_4 = new iPhone_15_Max();

// if all take_a_pinture under override
ptr_1.take_a_picture(); //SmartPhone::take_a_picture();
ptr_2.take_a_picture(); //Samsung::take_a_picture();
ptr_3.take_a_picture(); //Apple::take_a_picture();
ptr_4.take_a_picture(); //iPhone_15_Max::take_a_picture();

// now we can control or initialize take_a_picture() by array 
SmartPhone *array[] = {ptr_1, ptr_2, ptr_3, ptr_4};

for (int i = 0; i<4; i++){
  array[i]->take_a_picture();
}
```
### Example of base reference

```cpp
SmartPhone smartphone;
SmartPhone& ref_1 = smartphone;
ref_1.take_a_picture(); //SmartPhone::take_a_picture();

Samsung samsung;
SmartPhone& ref_2 = samsung;
ref_2.take_a_picture(); //Samsung::take_a_picture();

Apple apple;
SmartPhone& ref_3 = apple;
ref_3.take_a_picture(); //Apple::take_a_picture();

iPhone_15_Max iPhone15Max;
SmartPhone& ref_4 = iPhone15Max;
ref_4.take_a_picture(); //iPhone_15_Max::take_a_picture();

// product = {smartphone, samsung, apple, iPhone15Max}
void take_a_picture(SmartPhone& product){
  product.take_apicture()
}
```
<br>

## Override/final specifier

### *Override* specifier

The "override specifier" is a keyword in C++ that is used to explicitly indicate that a member function in a derived class is intended to override a virtual function in its base class. It is a part of C++11 and later standards.

By using the override specifier, you make it clear to the compiler and other developers that you intend for the function to override a virtual function in a base class. If the function does not match a base class virtual function signature, the compiler will generate an error, helping to catch potential mistakes.

```cpp
class Base {
public:
    virtual void myFunction() const {
        // some implementation
    }
};

class Derived : public Base {
public:
    void myFunction() const override {
        // overridden implementation
    }
};
```

In the above example, the override specifier is used in the Derived class to indicate that myFunction is intended to override the virtual function in the Base class. If there is a mismatch in function signatures, the compiler will raise an error.

### *Final* specifier

The final specifier is a C++ keyword introduced in C++11 and is used to prevent further overriding of a virtual function in derived classes or to indicate that a class should not be used as a base class for further inheritance.

#### Function Overriding

When applied to a virtual function in a base class, final ensures that the function cannot be overridden in any derived class. If a programmer attempts to override a final function, the compiler will generate an error.

```cpp
class Base {
public:
    virtual void myFunction() const final {
        // some implementation
    }
};

class Derived : public Base {
public:
    // Error: 'void Derived::myFunction() const' marked 'final', but is not overriding
    void myFunction() const override {
        // overridden implementation
    }
};
```

#### Class Inheritance

When applied to a class declaration, final indicates that the class cannot be used as a base class for further inheritance. This means it cannot be extended by other classes.

```cpp
class Base final {
    // class implementation
};

// Error: cannot derive from 'final' base 'Base'
class Derived : public Base {
    // class implementation
};
```

The use of final can provide clarity in design, indicating that certain functions or classes are intended to be terminal points in an inheritance hierarchy.

## Pure virtual function and Abstract class

### Pure virtual function

A pure virtual function in C++ is a virtual function that is declared in a base class but has no implementation in that class. It is marked with the = 0 syntax, indicating that any derived class must provide its own implementation for the pure virtual function. A class containing at least one pure virtual function becomes an abstract class, and instances of such classes cannot be created.

```cpp
class Shape {
public:
    // Pure virtual function
    virtual void draw() const = 0;

    // Non-pure virtual function
    void print() const {
        // some implementation
    }
};

class Circle : public Shape {
public:
    // Must provide an implementation for the pure virtual function
    void draw() const override {
        // implementation specific to Circle
    }
};

class Square : public Shape {
public:
    // Must provide an implementation for the pure virtual function
    void draw() const override {
        // implementation specific to Square
    }
};
```

In the above example, the draw function in the Shape class is a pure virtual function, meaning that any class inheriting from Shape must provide its own implementation for draw. As a result, Shape becomes an abstract class, and objects of type Shape cannot be instantiated. The derived classes Circle and Square must provide their own implementations for the draw function to be considered complete classes.

### Abstract class

An abstract class in C++ is like a blueprint for other classes. It cannot be used on its own; instead, it's meant to be inherited by other classes.

>Cannot Be Instantiated:<br> - ***You cannot create objects directly from an abstract class***.

```cpp
#include <iostream>

// Abstract class
class Shape {
public:
    // Pure virtual function
    virtual void draw() const = 0;
    // Concrete function
    void print() const {
        std::cout << "Printing a shape." << std::endl;
    }
};

// Concrete derived class
class Circle : public Shape {
public:
    // Implementation of the pure virtual function
    void draw() const override {
        std::cout << "Drawing a circle." << std::endl;
    }
};

int main() {
    // Create an object of the derived class
    Circle circle;

    // Use the overridden draw function
    circle.draw();  // Drawing a circle.
    // Use the print function from the base class
    circle.print();  // Printing a shape.

    return 0;
}
```
>In the example above, making object of 'Shape' class is prohibitted since it is abstract class

<br>

```cpp
Shape shape; // Compile Error !!!
Shape ptr = new Shape(); // Compile Error !!!
/* BUT BELOW IS STILL POSSIBLE */

Shape ptr = new Circle();
ptr->draw();
ptr->print();
```
<br>

## Abstract class and interface

An abstract class that ***only contains pure virtual functions*** is referred to as an *interface*. Interface is used to specify the functionalities that derived classes should have, so it is essentially a ***shell or a blueprint*** for classes.

>The keyword "interface" exists in Java and C# but is not present in C++.

```cpp
class IShape{
public:
  virtual void draw() = 0;
  virtual void rotate() = 0;
  virtual ~Shape();
};

class Circle : public IShape{
public:
virtual void draw() override{
  //...
}
virtual void rotate() override{
  //...
}
virtual ~Circle();
};
```

>By convention, an interface class name is prefixed with the uppercase letter 'I'.

---
layout: single

title: "[OOP] Operator overloading"
excerpt: "function, stream I/O, '=' and subscript"

categories:
  - Object oriented programming

tag: [C/C++, OOP, overloading, operator] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

# Operator overloading

Operator overloading is a technique in C++ where users redefine or extend the functionality of existing operators for their custom classes or user-defined data types. This allows users to apply standard operators to instances of their own classes, providing a more intuitive and effective way to work with user-defined types.

```cpp
class apple{
private:
    double weight;
public:
    apple(double weight)
        : weight(weight){}
}

main (){
    apple apple_1(52.5);
    apple apple_2(49.23);

    // if we want to sum above apples 
    double weight_sum = apple_1 + apple_2 
    //COMPILE ERROR !!!
}
```

operator + is not defined to sum the data type 'apple'. that should be redefiend, and overriden to be available for user made datatype.

### How we did to solve above issue without oeprator overloading

the problem aboce can be tackled as follows using defining member or global function to get weight without operator. 


```cpp
//Duplicated codes are skipped 
// can define weight_sum function inside apple class
double weight_sum(apple apple_2){
    doulbe weight_sum;
    sum_weight = this->weight + apple_2.get_weight();
    return sum_weight;
}
// Or, define weight_sum functin as global function
// having 2 parameters of apple
double weight_sum(apple apple_1, apple apple_2){
    doulbe weight_sum;
    sum_weight = apple_1.get_weight() + apple_2.get_weight();
    return sum_weight;
}

main (){
    //Both functions can be used as follows
    double weightSum;
    
    weightSum = appl_1.weight_sum(apple_2);
    weightSum = weight_sum(apple_1, apple_2);
    // TOO COMPLEX WITHOUT OPERATOR !
}
```
Operator rules:

Operator overloading is implemented in the form of functions. For example, operator+ represents a function that overloads the addition operator (+).

- **Parameters**: most operator overloading functions have parameters. These parameters represent the two operands involved in the operation. When defined as a member function of a class, there is typically one parameter.

- **Return Type**: operator overloading functions, like regular functions, have a return type and return the result of the operation. The result can be returned by reference or by value.

- **Number of Operands**: most binary operators have two operands, while unary operators have one operand.

- **Operator Precedence and Associativity**: operator overloading does not alter the precedence and associativity of the corresponding operators. It follows the rules of the original operators.

- **Member Function vs Global Function**: for binary operators, overloading can be done as a member function of a class, and for unary operators, overloading can be done as a member function or a global function.

- **Default Operator Overloading Not Possible**: some operators cannot be overloaded by default. For example, operators like . (member access operator) and :: (scope resolution operator) cannot be overloaded.


## Member function operator overloading

### Declaration of a binary operator as a member function

>+,-,==,!=,>,<, etc.

- Example of declaration and how it useds in *'Point'* class

```cpp
class point {
public:
    int x;
    int y;
}

//Example of declaration
point operator+(const point& rhs) const;
point operator-(const point& rhs) const;
bool operator==(const point& rhs) const;
bool operator!=(const point& rhs) const;
//const in (const point& rhs) means p2 value doens't change
//const at the end of the declaration means p1 values doen't chage 

//Example of how it used
point p3;
p3 = p1 + p2 // p1.operator+(p2); made by compiler
if(p1 == p2){  //p1.operator==(p2); made by compiler
    ...
}
```

> ***Compiler interprets "p3 = p1 + p2" into "p1.operator+(p2)"***

let's look at a more concrete example when we have point class as follows

<div class="mermaid"> 
classDiagram
      class point{
        -int x;
        -int y;
        +point(int x, int y): x(X), y(y)
        +get_x()
        +get_y()
        +operator+()
      }
</div>

The operator+ can be coded below

```cpp
point operator+(const point& rhs) const{
    point result(this->get_x()+rhs.get_x,this->get_y()+rhs.get_y());
    return retul
}

int main(){
    point p1(1,2);
    point p2(2,3);
    point p3 = p1+p2;
    // p3 = (3,5)
}
```
### Declaration of a unary operator as a member function

>++,--,-,!

- Example of declaration and how it useds in *'Point'* class

```cpp
//Example of declaration
point operator-()const;
point& operator++(); // pre-increment
point operator++(int); //post-increment
bool operator!() const;

//Example of how it used
point p1 = (10,20);
point p2 = -p1; //p1.operator-();
p2 = p1++; //p1.operator++(int);
p2 = ++p1; //p1.operator++();
```

> ***Compiler interprets "-p1" into "p1.operator-()"***, unary operator doesn't have parameter unlike binary operator

### limitation of member function in operator

Making the commutative property hold may not be possible

```cpp
point p2 = p1*3 // p1.operator*(3) -> compile OK !!!
point p2 = 3*p1 // 3.operator*(p1) -> compile ERROR !!!
```
This issue can be addressed by declaring the function as a global function.

## Global fuunction operator overloading

>+,-,==,!=,>,<, etc.

- Example of declaration and how it useds in *'Point'* class

```cpp
class point {
public:
    int x;
    int y;
}

//Example of declaration
point operator+(const point& lhs, const point& rhs);
point operator-(const point& lhs, const point& rhs);
bool operator==(const point& lhs, const point& rhs);
bool operator!=(const point& lhs, const point& rhs);

//Example of how it used
point p3;
p3 = p1 + p2 // operator+(p1, p2); made by compiler
if(p1 == p2){  //operator==([p1, p2); made by compiler
    ...
}
```

> ***Compiler interprets "p3 = p1 + p2" into "operator+(p1, p2)"***

Creating the operator function as a global function allows convenient access to private member variables using the friend keyword.

```cpp
friend point operator+(const point& lhs, const point& rhs);
```

## stream insertion(<<), extraction(>>) operator overloading

Let's recall point class. 

<div class="mermaid"> 
classDiagram
      class point{
        -int x;
        -int y;
        +point(int x, int y): x(X), y(y)
      }
</div>

If we want to print value of x and y to the console, we can write down code as below

```cpp
int main(){
    cout << point.x << point.y << endl;
}

//this will show values of x and y
```

what should we do if we want to make code as below, and make same result to the code above?

```cpp
    cout << point << endl;
//this will compile error 
```

we can use operator<< overriding.

> let's reproduce the usage of std::cout and examine how the << operator is utilized

```cpp
class MyOstream{
public:
    void operator<<(int value){
        printf("%d",value);
    }
};

int main(){
    MyOstream mycout;
    mycout << point.x // display point.x value
}
```

In this manner, we can arbitrarily define stream insertion (<<) and extraction (>>) operator overloading to return the desired values
It is highly recommended to create global objects for the purpose of using these operators. This is because modifying the iostream to tamper with the cout object is technically risky and involves potentially dangerous operations.

>cout.operator<<(value)(X)<br>operator<<(cout,value)(O), this is global function

If we make operator<< as class member function, the structure will be 

>> p1 << std::cout; // p1.operator<<(std::cout)

this is how we make it clearly

```cpp
//ostream's copy is prohibited thus & is essential 
friend std::ostream& operator<<(str::ostream& os, const point& rhs){
    os<<'['<<rhs.xpos<<','<<rhs.ypos<<']'<<endl;
    return os; // chain insertion
}

int main()
{
    point p1(1,2);
    cout<<p1<<endl;
}
```

Let's see extraction(>>) operator

```cpp
friend std::istream& oprator>>(str::istream& is, const point& rhs){
    int x = 0; y = 0;
    is>>x>>y
    rhs = point(x,y);
    return is;
}

int main(){
    point p1;
    std::cin>>p1;
}
```

## Assignment(=) operator overloading

The assignment operator for "shallow copy" automatically occurs when assignment takes place (note that when declaration and initialization happen simultaneously, it is copying, not assignment). The implementation of this function would look like the following:

```cpp
point& operator=(const point& rhs){
    
    if (this == &rhs){
        return *this;
    }
    
    x = rhs.x;
    y = rhs.y;

    return *this;
}

```
When a class has a member variable of pointer type, especially when it points to dynamically allocated memory, it is necessary to define copy constructor and assignment operator to perform 'deep copy.' Deep copy involves creating a new copy of the pointed data rather than copying the address of the memory. This ensures that the objects do not share the same memory, preventing unintended side effects.

```cpp
class Array{
private:
    int *ptr;
    int size;
public:
    Array(int val, int size)
        : size(size)
    {
        ptr = new int[size];
        for (int i = 0;i<size;i++>){
            ptr[i] = val+i;
        }
    }
    int GetSzie() const{
        return size;
    }
    int GetValue(int index) const{
        if (index<size && index >=0){
            return ptr[index];
        }else{
            cout<<"Out of range"<<endl;
        }
    }
    ~Array(){
        delate[] ptr;
    }
}

int main(){
    Array a_1(5,10);
    Array a_2(3,5);

    a_2 = a_1;
}

```

If you write the code as above, a problem arises because the data in the heap memory that a_1 is pointing to is duplicated and disappears, and the data in a_2 remains unchanged. To address this issue and perform a deep copy, let's add the following code

```cpp
Array& operator=(const Array& rhs){ //a_2.operator=(a_1)
    if (this == &rhs) return *this;

    delete[] ptr;
    ptr = new int[rhs.size];
    size = rhs.size;
    for (int i = 0;i<size;i++){
        ptr[i] = rhs.ptr[i];
    } 
    return *this;
}
```

## Subscript([]) operator overloading

Let's use the array class above and try using the getvalue() function.

```cpp
Array arr(5);
cout<< arr.GetValue(-1)<<endl; //compile okay !!! but returns rubbish value
```

The given code is expected to compile without issues because the GetValue() function takes an integer parameter, and using -1 as an argument is valid. however, it will return garbage value. thus ***the necessity of boundary checks*** is arouse here

we can add boundary checker
```cpp
int& operater[](int rhs){
    if (rhs < 0 || rhs >= size){
        cout << "Index is out of boundary" << endl;
        exit(1);
    }
    return arr[rhs];
}

int main(){
    Array arr(5);
    arr[3] = 15; // compile okay because operator
                 // returns int& 
}
```





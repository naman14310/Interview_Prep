# Class and Objects

**Classes** are user-defined data types that act as the blueprint for individual objects, attributes and methods. Whereas, **Objects** are instances of a class created with specifically defined data. Objects can correspond to real-world objects or an abstract entity. 

<br>

## Structure Vs Class

#### Similarities:
1. Both are user defined data types.
2. Access specifiers, such as public, private, and protected, are identically used in structures and classes to restrict the access of their data and methods outside their body.
3. Both can have constructors, methods, properties, fields, constants, enumerations etc.
4. Both structures and classes can implement interfaces to use multiple-inheritance in code.


#### Differences:
1. Class provides more security then structures.
2. Members of a class are private by default and members of a structure are public by default. 
3. When deriving a struct, the default access-specifier of base struct is public. While deriving a class, the default access specifier for base class is private. 

<br>

#### When Structures should be used ?
When we only want a user-defined datatypes with small memory footprint. It will simply act like a group or multiple primitive datatypes. Use a struct when we want value-type semantics instead of reference-type. Structs are copy-by-value.

<br>

#### When Classes should be used ?
When we need to map real world entities with our program. Or when we need polymorphic behaviour. Use classes to build complex programs in which we want to perform multiple functions on objects.

<br>

## Access Modifiers
Access Modifiers or Access Specifiers in a class are used to assign the accessibility to the class members (By default, they are private). Following are 3 access modifiers:
1. Public
2. Private
3. Protected

<br>

![img](https://www.sitesbay.com/cpp/images/access-specifier-in-cpp.png)

<br>

#### Public
The public members of a class can be accessed from anywhere in the program using the direct member access operator (.) with the object of that class. 

```cpp
class Circle{
public:
    double radius;

    double compute_area(){
        return 3.14*radius*radius;
    }
};
 
int main(){
    Circle obj;
    obj.radius = 5.5;       // --> accessing public datamember outside class
     
    cout << "Radius is: " << obj.radius << "\n";
    cout << "Area is: " << obj.compute_area();
    return 0;
}
```

<br>

#### Private
The class members declared as private are not allowed to be accessed directly by any object or function outside the class. Only the member functions or the friend functions are allowed to access the private data members of a class. We can access the private data members of a class indirectly using the public member functions of the class. 

```cpp
class Circle{  

private:
    double radius;      // --> private data member

public:   
    double  compute_area()  {   
        return 3.14*radius*radius;      // --> public member function can access private data member radius
    }

};
 
int main(){  
    Circle obj;
     
    /* trying to access private data member directly outside the class */
    
    obj.radius = 1.5;   // --> COMPILE ERROR
     
    cout << "Area is:" << obj.compute_area();       // --> Use private data members using public member functions
    return 0;
}
```

<br>

#### Protected
Only difference of Protected with Private is that it can be accessed by any subclass (derived class) of that class as well. 

```cpp

/* base class */
class Parent{  
    protected:
        int id_protected;       // --> protected data members
};
 
/* derived class from public base class */

class Child : public Parent{
    public:
        void setId(int id){

            /* Child class is able to access the inherited
            protected data members of base class */

            id_protected = id;
        }
     
    void displayId(){
        cout << "id_protected is: " << id_protected << endl;
    }
};
 

int main() {
    Child obj1;
     
    /* member function of the derived class can
    access the protected data members of the base class */
     
    obj1.setId(81);
    obj1.displayId();
    return 0;
}
```


<br>

## Member Functions
Member functions are the functions, which have their declaration inside the class. Inside class definition it can be defined directly, but outside the class, we have to use the scope resolution `::` operator along with class name along with function name.

```cpp
class Cube{
public:
    int side;
    int getVolume();        // --> Member function declared here, but defined outside the class
    
    int getArea(){          // --> Member function declared and defind inside the class
        return side*side;
    }
}

// --> member function defined outside class definition
int Cube :: getVolume(){
    return side*side*side;
}

```

<br>

### Types of Member Functions
1. Simple functions
2. Static functions
3. Const functions
4. Inline functions
5. Friend functions


<br>

## Inline Functions
These functions are copied everywhere during compilation, like preprocessor macro, so the overhead of function calling is reduced. All the functions defined inside class definition are by default inline. We can also make any non-class function inline by using keyword inline.

#### Points to Note (IMP):
1. For an inline function, declaration and definition must be done together. 
2. Inline functions do increase efficiency, but we should not make all the functions inline. Because if we make large functions inline, it may lead to code bloat, and might affect the speed too.
3. Hence, it is adviced to define large functions outside the class definition using scope resolution :: operator, because if we define such functions inside class definition, then they become inline automatically.
4. Inline functions doesn't get storage, they are kept in Symbol table.

<br>

Note: Inline expansion makes a program run faster because the overhead of a function call and return is eliminated. However, it makes the program to take up more storage because the statements that define inline functions are reproduced at each point where the function is called. So a trade-off becomes necessary.

<br>

### Forward References
All the inline functions are evaluated by the compiler, at the end of class declaration. 

```cpp
class ForwardReference{
    int i;
public:

    /*  This code will work because no inline function in a class is evaluated 
        until the closing braces of class declaration. */
    
    int f() {
        return g() + 10;      // --> call to undeclared function
    }
    
    int g() {
        return i;
    }
};

int main(){
    ForwardReference fr;
    fr.f();
}
```

<br>

## Friend Class and Friend Functions
A **Friend class** can access private and protected members of other class in which it is declared as friend.

Note: Unlike member functions, it cannot access the member names directly and has to use an object name and dot membership operator.

```cpp
#include <iostream>
class A {
private:
    int a;
 
public:
    A() { a = 0; }
    friend class B;         // --> Friend Class
};
 
class B {
private:
    int b;
 
public:
    void showA(A &x){
        // --> Since B is friend of A, it can access private members of A
        cout << "A::a=" << x.a;
    }
};
 
int main(){
    A a;
    B b;
    b.showA(a);
    return 0;
}
```
 
 <br>

**Friend Function** can be given a special grant to access private and protected members. A friend function can be: 
1. A member of another class 
2. A global function 

```cpp
#include <iostream>
 
class B;
 
class A {
public:
    void showB(B&);
};
 
class B {
private:
    int b;
 
public:
    B() { b = 0; }
    friend void A::showB(B& x);     // --> Friend function
};
 
void A::showB(B& x){
    // --> Since showB() is friend of B, it can access private members of B
    std::cout << "B::b = " << x.b;
}
 
int main(){
    A a;
    B x;
    a.showB(x);
    return 0;
}
```

<br>

Following are some important points about friend functions and classes: 
1. Friendship is not mutual. If class A is a friend of B, then B doesn’t become a friend of A automatically.
2. Friendship is not inherited.
3. When we make a class as friend, all its member functions automatically become friend functions.
4. A function can be declared friend with any number of classes.
5. A friend function is not in the scope of class to which it has been declared as friend hence it cannot be called using object of that class.
6. It can be declared either in public or private part of class without affecting its meaning.

<br>

#### NOTE: Friend Functions is a reason, why C++ is not called as a pure Object Oriented language. Because it violates the concept of Encapsulation.

<br>



## Memory Allocation of Objects
Usually, C program has four memory sections: heap, stack, data, code.

1. **Heap:** This memory is unused and can be used to dynamically allocate the memory at runtime. It is applied and freed by programmer.
2. **Stack:** Store function’s parameter and local variant in a function. It is system’s responsibility to apply and free the space.
3. **Data:** Store global variant and static variant and constant. It can be divided into three small parts in detail. First initialized global variant and static variant, then uninitialized global variant and uninitialized static variant, last constant variant.
4. **Code:** Store your code. (only can be read, not allowed to be written)

<br>


## The Static Keyword
We can use static keyword with:
1. Variable
2. Objects
3. Functions

<br>

### Static Variables
1. When a variable is declared as static, space for it gets allocated for the lifetime of the program. They have a property of preserving their value even after they are out of their scope.
2. Even if the function is called multiple times, space for the static variable is allocated only once and the value of variable in the previous call gets carried through the next function call. 
3. Static variables are allocated memory in data segment, not stack segment.
4. Static variables (like global variables) are initialized as 0 if not initialized explicitly. 
5. There can not be multiple copies of same static variables for different objects.
6. Static variables can not be initialized using constructors.
7. In C, static variables can only be initialized using constant literals. (C++ removed this property)

```cpp

#include<iostream>
using namespace std;
  
class GfG{
public:
    static int i;
      
    GfG(){
        // Do nothing
    };
};

/*  static variable inside a class must be defined or initialized explicitly by the user 
    using the class name and scope resolution operator outside the class.   */

int GfG::i = 1;  
  
int main()
{
    GfG obj;
    cout << obj.i; 
}
```

<br>

### Static Objects
Just like variables, objects also when declared as static have a scope till the lifetime of program.

**Program 1: Normal Object**

```cpp
class GfG{
    int i;
    public:
        GfG(){
            i = 0;
            cout << "Inside Constructor, ";
        }
        ~GfG(){
            cout << "Inside Destructor, ";
        }
};
  
int main(){
    int x = 0;
    if (x==0){
        GfG obj;
    }
    cout << "End of main ";
}
```

Output: Inside Constructor, Inside Destructor, End of main

When the object is created, the constructor is invoked and soon as the control of if block gets over the destructor is invoked as the scope of object is inside the if block only where it is declared.

<br>

**Program 2: Static Object**

```cpp
#include<iostream>
using namespace std;
  
class GfG{
    int i = 0;
      
    public:
    GfG(){
        i = 0;
        cout << "Inside Constructor, ";
    }
      
    ~GfG(){
        cout << "Inside Destructor";
    }
};
  
int main(){
    int x = 0;
    if (x==0){
        static GfG obj;
    }
    cout << "End of main, ";
}
```

Output: Inside Constructor, End of main, Inside Destructor

Now the destructor is invoked after the end of main. This happened because the scope of static object is through out the life time of program.


<br>

### Static Functions
1. These functions work for the class as whole rather than for a particular object of a class. 
2. Static member function can be called using class name and scope resolution :: operator.
3. They can only access static data members and static member functions. 
4. Static member functions do not have this pointer. 
5. A static member function cannot be virtual.
6. A static member function cannot be const and volatile.

<br>


 

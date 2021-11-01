# Class and Objects

**Classes** are user-defined data types that act as the blueprint for individual objects, attributes and methods. Whereas, **Objects** are instances of a class created with specifically defined data. Objects can correspond to real-world objects or an abstract entity. 

<br>

## @ Structure Vs Class

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

## @ Access Modifiers
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

## @ Member Functions
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

#### Types of Member Functions
1. Simple functions
2. Static functions
3. Const functions
4. Inline functions
5. Friend functions


<br>

## @ Inline Functions
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

## @ Friend Class and Friend Functions
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



## @ Memory Allocation of Objects
Usually, C/C++ program has four memory sections: heap, stack, data, code.

1. **Heap:** This memory is unused and can be used to dynamically allocate the memory at runtime. It is applied and freed by programmer.
2. **Stack:** Store function’s parameter and local variant in a function. It is system’s responsibility to apply and free the space.
3. **Data:** Store global variant and static variant and constant. It can be divided into three small parts in detail. First initialized global variant and static variant, then uninitialized global variant and uninitialized static variant, last constant variant.
4. **Code:** Store your code. (only can be read, not allowed to be written)

<br>


## @ The Static Keyword
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


## @ Constructors in C++
A constructor is a special type of member function of a class which initializes objects of a class. 


#### How constructors are different from a normal member function?
1. Constructor has same name as the class itself
2. Constructors don’t have return type
3. A constructor is automatically called when an object is created.
4. It must be placed in public section of class.
5. If we do not specify a constructor, C++ compiler generates a default constructor for object.

Note: In C++, the constructor cannot be virtual, because when a constructor of a class is executed there is no virtual table in the memory, means no virtual pointer defined yet. So, the constructor should always be non-virtual.

Note: Constructors are not inherited. They are called implicitly or explicitly by the child constructor.

<br>

![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20191128195435/CPP-Constructors.png)

<br>

**Real Life example:**

Suppose you went to a shop to buy a marker. When you want to buy a marker, what are the options? 
1. The first one you go to a shop and say give me a marker. So just saying give me a marker mean that you did not set which brand name and which color. So, whatever the frequently sold marker is there in the market or in his shop he will simply hand over that. And this is what a default constructor is.
2. The second method you go to a shop and say I want a marker a red in color and XYZ brand. So you are mentioning this and he will give you that marker. So in this case you have given the parameters. And this is what a parameterized constructor is.
3. Then the third one you go to a shop and say I want a marker like this(a physical marker on your hand). So the shopkeeper will see that marker, and give a new marker for you. So copy of that marker. And that’s what copy constructor is.

<br>

### Default Constructor
Default constructor is the constructor which doesn’t take any argument. Even if we do not define any constructor explicitly, the compiler will automatically provide a default constructor implicitly.

```cpp
class construct{
public:
    int a, b;
 
    /* Default Constructor */
    
    construct(){
        a = 10; b = 20;
    }
};
 
int main(){
    construct c;        // --> Default constructor called automatically when the object is created
    
    cout << "a: " << c.a << endl << "b: " << c.b;
    return 0;
}
```

<br>

### Parameterized Constructors
These are constructors with parameters. Typically, these arguments help initialize an object when it is created.

```
Example e = Example(0, 50); // --> Explicit call

Example e(0, 50);           // --> Implicit call
```

Note: Whenever we define one or more non-default constructors (with parameters) for a class, a default constructor should also be explicitly defined as the compiler will not provide a default constructor in this case.

```cpp
class Point{
private:
    int x, y;
 
public:
    /* Parameterized Constructor */
    Point(int x1, int y1){
        x = x1; y = y1;
    }
 
    int getX(){
        return x;
    }
    
    int getY(){
        return y;
    }
};
 
int main(){
    Point p1(10, 15);       // --> Constructor called
 
    // --> Access values assigned by constructor
    cout << "p1.x = " << p1.getX() << ", p1.y = " << p1.getY();
    return 0;
}

```

Uses of Parameterized constructor: 
1. It is used to initialize objects with different values when they are created.
2. It is used to overload constructors.

<br>

### Copy Constructor
It initializes an object using another object of the same class.

```cpp
class Point{
private:
    int x, y;
public:
    Point(int x1, int y1) { x = x1; y = y1; }
 
    /* Copy constructor */
    
    Point(const Point &p1) {
        x = p1.x; y = p1.y; 
    }
 
    int getX()  {  return x; }
    int getY()  {  return y; }
};
 
int main(){

    Point p1(10, 15);   // --> Normal constructor is called here
    Point p2 = p1;      // --> Copy constructor is called here
 
    // --> Let us access values assigned by constructors
    cout << "p1.x = " << p1.getX() << ", p1.y = " << p1.getY();
    cout << "\np2.x = " << p2.getX() << ", p2.y = " << p2.getY();
    return 0;
}
```

<br>

#### When is copy constructor called ? 
1. When an object of the class is returned by value. 
2. When an object of the class is passed (to a function) by value as an argument. 
3. When an object is constructed based on another object of the same class. 
4. When the compiler generates a temporary object.

<br>

### Shallow Copy Vs Deep Copy

![img](https://i.stack.imgur.com/AWKJa.jpg)

<br>

If we don’t define our own copy constructor, the C++ compiler creates a default copy constructor for each class which does a member-wise copy between objects. It works fine in general, But if there is any pointer variable defined in the object, then pointer in copied object will also points to the same location. This is known as **Shallow copy**. 

<br>

If we manually write a copy constructor and assign pointer values then that will be the case of **Deep copy**. It is possible only with user defined copy constructor. 

```cpp
class box {
private:
    int a;
    int* p;
  
public:
    box(){
        p = new int;
    }
  
    void set(int x, int y){
        a = x;
        *p = y;
    }
  
    // --> Manually written Copy constructor for implementing Deep Copy
    
    box(box &obj){
        a = obj.a;
        p = new int;
        *p = *(obj.p);
    }
};
  
int main(){
    box first;          // --> first object
    first.set(12, 14);
    
    box second = first;     // --> Here deep copy will happen  
    return 0;
}
```

<br>

#### Copy constructor vs Assignment Operator 
Copy constructor is called when a new object is created from an existing object, as a copy of the existing object. Assignment operator is called when an already declared(or initialized) object is assigned a new value from another existing object.

```
MyClass t1, t2;
MyClass t3 = t1;  // --> Copy constructor will be called
t2 = t1;          // --> Overridden assignment operator will be called
```

<br>

#### Can we make copy constructor private ? 
Yes, a copy constructor can be made private. When we make a copy constructor private in a class, objects of that class become non-copyable. This is particularly useful when our class has pointers or dynamically allocated resources. 

<br>

#### Why argument to a copy constructor must be passed as a reference ? 
A copy constructor is called when an object is passed by value. So if we pass an argument by value in a copy constructor, a call to copy constructor would be made to call copy constructor which becomes a non-terminating chain of calls. Therefore compiler doesn’t allow parameters to be passed by value.

<br>

#### [Why copy constructor argument should be const in C++ ?](https://www.geeksforgeeks.org/copy-constructor-argument-const/)

<br>

## @ Destructors in C++
Destructor is an instance member function which is invoked automatically whenever an object is going to be destroyed. It is the last function that is going to be called before an object is destroyed. If the object is created by using 'new' to allocate memory which resides in the heap memory, the destructor should use 'delete' to free the memory.   

<br>

#### Properties of Destructor
1. It cannot be declared static or const.
2. The destructor does not have arguments.
3. It has no return type not even void.
4. An object of a class with a Destructor cannot become a member of the union.
5. A destructor should be declared in the public section of the class.
6. The programmer cannot access the address of destructor.
7. Destructor can be virtual.

<br>

#### When is destructor called ? 
A destructor function is called automatically when the object goes out of scope: 
1. the function ends 
2. the program ends 
3. a block containing local variables ends 
4. a delete operator is called  

<br>

#### When do we need to write a user-defined destructor ? 
If we do not write our own destructor in class, compiler creates a default destructor for us. The default destructor works fine unless we have dynamically allocated memory or pointer in class. When a class contains a pointer to memory allocated in class, we should write a destructor to release memory before the class instance is destroyed. This must be done to avoid memory leak.

<br>

#### Can there be more than one destructor in a class ? 
No, there can only one destructor in a class with classname preceded by ~, no parameters and no return type.

<br>

#### When do we need to write a user defined destructor ?
There are two things that necessitate defining a destructor:
1. When your object gets destructed, you need to perform some action other than destructing all class members.
2. When you need to destruct objects through a base class pointer. When you need to do this, you must define the destructor to be virtual within the base class. Otherwise, your derived destructors won't get called, independent of whether they are defined or not, and whether they are virtual or not.

```cpp
class Foo {
    public:
        ~Foo() {
            std::cerr << "Foo::~Foo()\n";
        };
};

class Bar : public Foo {
    public:
        ~Bar() {
            std::cerr << "Bar::~Bar()\n";
        };
};

int main() {
    Foo* bar = new Bar();
    delete bar;
}
```

<br>

#### [Virtual Destructors](https://www.youtube.com/watch?v=JXHJZJXKP64)  

#### [Private Destructors](https://www.geeksforgeeks.org/private-destructor/)
 

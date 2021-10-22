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

### Public
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

### Private
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

### Protected
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

## Static Member Functions
1. These functions work for the class as whole rather than for a particular object of a class. 
2. Static member function can be called using class name and scope resolution :: operator.
3. These functions cannot access ordinary data members and member functions.
4. They can only access static data members and static member functions.

<br>

## Const Member Functions
Such member functions can never modify the object or its related data members.

<br>

## Friend Class and Friend Functions
A **Friend class** can access private and protected members of other class in which it is declared as friend.

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
 

**Friend Function** can be given a special grant to access private and protected members. A friend function can be: 
a) A member of another class 
b) A global function 

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

 

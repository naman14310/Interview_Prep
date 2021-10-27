# Polymorphism

The term "Polymorphism" is the combination of "poly" + "morphs" which means many forms. When one a function or operator act differently in different situations, then that is known as polymorphism.

<br>

### Real Life Example Of Polymorphism
A lady behaves like a teacher in a classroom, mother or daughter in a home and customer in a market. Here, a single person is behaving differently according to the situations.

<br>

## Types of Polymorphism

<br>

![img](https://static.javatpoint.com/cpp/images/cpp-polymorphism.png)

<br>

## Overloading
If we create two or more members having the same name but different in number or type of parameter, it is known as C++ overloading.

In C++, we can overload:
1. Functions
2. Constructors
3. Operators

<br>

### Function Overloading
In function overloading, the function is redefined by using either different types of arguments or a different number of arguments. Only through these differences compiler can differentiate between the functions.

**The advantage of Function overloading is that it increases the readability of the program because you don't need to use different names for the same action.**

```cpp
class Cal {    
public:    
    int add(int a,int b){      
        return a + b;      
    }      
    
    int add(int a, int b, int c) {      
        return a + b + c;      
    }      
    
    float mul(double x, int y) {  
        return x + y;  
    }  
};     

int main(void) {    
    Cal C;                                                     
    cout<<C.add(10, 20)<<endl;      
    cout<<C.add(12, 20, 23); 
    cout<<C.add(12.5, 2);
    return 0;    
}    
```

<br>

#### [Which Functions cannot be overloaded in C++ ?](https://www.geeksforgeeks.org/function-overloading-in-c/)
1. Function declarations that differ only in the return type fails in compilation.
2. Member function declarations with the same name and the name parameter-type-list cannot be overloaded if any of them is a static member function declaration. 
3. Parameter declarations that differ only in a pointer * versus an array [] are equivalent. That is, the array declaration is adjusted to become a pointer declaration.
4. Parameter declarations that differ only in that one is a function type and the other is a pointer to the same function type are equivalent.
5. Parameter declarations that differ only in the presence or absence of const and/or volatile are equivalent.

<br>

#### Ambiguity in Function Overloading
Ambiguity occurs When the compiler is unable to decide which function is to be invoked among the overloaded function.

<br>

**Case1: Type Conversion Ambiguity**

The fun(1.2) calls the second function according to our prediction. But, this will not happen as in C++, all the floating point constants are treated as double not as a float. If we replace float to double, the program works.

```cpp
void fun(int i) {  
    cout << "Value of i is : " <<i<< endl;  
}  

void fun(float j) {  
    cout << "Value of j is : " <<j<< endl;  
}  

int main()  {  
    fun(12);  
    fun(1.2);  
    return 0;  
}  
```

<br>

**Case2: Ambiguity with Default Arguments**

```cpp
void fun(int i) {  
    cout << "Value of i is : " <<i<< endl;  
}  

void fun(int a,int b=9) {  
    cout << "Value of a is : " <<a<< endl;  
    cout << "Value of b is : " <<b<< endl;  
}  

int main()  {  
    fun(12);  
    return 0;  
}
```

<br>

**Case3: Ambiguity with pass by reference**

```cpp
void fun(int x) {  
    cout << "Value of x is : " <<x<< endl;  
}  

void fun(int &b) {  
    cout << "Value of b is : " <<b<< endl;  
}  
```

<br>

### Operator Overloading
Operator overloading is used to overload or redefines most of the operators available in C++. It is used to perform the operation on the user-defined data type.

**The advantage of Operators overloading is to perform different operations on the same operand.**

<br>

#### List Of Operators that can't be Overloaded
1. Scope operator (::)
2. Sizeof()
3. member selector(.)
4. member pointer selector(*)
5. ternary operator(?:)

<br>

```
Syntax of Operator Overloading
------------------------------

return_type  class_name  :: operator  op(argument_list) {  
     // --> body of the function.  
}  
```

#### Rules for Operator Overloading
1. Existing operators can only be overloaded, but the new operators cannot be overloaded.
2. The overloaded operator contains atleast one operand of the user-defined data type.
3. We cannot use friend function to overload certain operators. However, the member function can be used to overload those operators.
4. When unary operators are overloaded through a member function take no explicit arguments, but, if they are overloaded by a friend function, takes one argument.
5. When binary operators are overloaded through a member function takes one explicit argument, and if they are overloaded through a friend function takes two explicit arguments.

```cpp
class Test {    
private:    
    int num;    
    
public:    
    Test(): num(8){}    
    
    void operator ++ () {     
        num = num+2;     
    }    
    
    void Print() {     
        cout<<"The Count is: "<<num;     
    }    
};    

void Test :: operator + (Test t2) {  
    num += t2.num;   
}  


int main() {    
    Test t1;    
    ++t1;       // --> calling of a function "void operator ++()"    
    
    Test t2;    // --> calling of a function "void operator +()"   
    t1+t2;
    
    t1.Print();    
    return 0;    
}    
```

<br>

## Overriding
If derived class defines same function as defined in its base class, it is known as function overriding in C++. It is used to achieve runtime polymorphism. It enables you to provide specific implementation of the function which is already provided by its base class.

### Need Of Function Overriding
Lets take an example of class car and its derived class sportsCar. Now car has some service (or function) shiftGear which changes gear with default style. Now sportscar wants to shift gear in modern style. Now we can think that, why not we should create new function for this functionality. But, now if we can create a new function, sportscar object can change gear in modern style but it can also access prev function of base class. Not this is wrong because, one car should not have two definations for one service. So that's why we override the base class function itself instead of making new one. 

```cpp
class car{
public:
    void shiftGear(){
        cout<<"change gear in default style";
    }
};

class sportsCar : public car{
public:

    /* Overrided Function */

    void shiftGear() {
        cout<<"change gear in modern style";
    }
}; 

int main(void) {  
    sportsCar c;    
    c.shiftGear();  
    return 0;  
}
```

```
Output: change gear in modern style
```

<br>

## Virtual Function
A C++ virtual function is a member function in the base class that you redefine in a derived class. It is declared using the virtual keyword. It is used to tell the compiler to perform dynamic linkage or late binding on the function. When the function is made virtual, C++ determines which function is to be invoked at the runtime based on the type of the object pointed by the base class pointer.

<br>

### [Need of Virtual Function & Early Binding Vs Late Binding](https://www.youtube.com/watch?v=cUCy2ENJjW8)
As we know that, base class pointer can store address of child class pointer. Now consider following program:

```cpp
class car{
public:
    void shiftGear(){
        cout<<"change gear in default style";
    }
};

class sportsCar : public car{
public:

    /* Overrided Function */
    
    void shiftGear() {
        cout<<"change gear in modern style";
    }
};

int main(){
    car* ptr;
    sportsCar c;
    ptr = &c;       // --> here base class ptr is storing address of child class object
    
    ptr->shiftGear();       // --> POINT OF AMBIGUITY
    return 0;
}

```

```
Output: change gear in default style
```

Now in above example, when we try to call shiftGear function using ptr (which is having base class datatype), the compiler tries to bind that function call at compile time by looking at its datatype (without looking inside it, at what address that ptr is pointing to). Hence, compiler will bind that function call to shiftGear function of base class and execute that function (But child class function should run, as we overridded the function). Hence to overcome this situation, virtual keyword is used in front of functions (which are going to be overrided by derived classes) to tell the compile to bind them at runtime (not at compile time).  

<br>

```cpp
class car{
public:
    virtual void shiftGear(){
        cout<<"change gear in default style";
    }
};

class sportsCar : public car{
public:

    /* Overrided Function */
    
    void shiftGear() {
        cout<<"change gear in modern style";
    }
};

int main(){
    car* ptr;
    sportsCar c;
    ptr = &c;       // --> here base class ptr is storing address of child class object
    
    ptr->shiftGear();       // --> POINT OF AMBIGUITY
    return 0;
}
```

```
Output: change gear in modern style
```

<br>

### [How Virtual Functions Internally Work](https://www.youtube.com/watch?v=Z_FiER8aAqM)

<br>

#### Some Properties of Virtual Functions
1. Virtual functions must be members of some class.
2. Virtual functions cannot be static members.
3. They can be a friend of another class.
4. A virtual function must be defined in the base class, even though it is not used.
5. We cannot have a virtual constructor, but we can have a virtual destructor
6. [In C++, virtual functions can be private and can be overridden by the derived class.](https://www.geeksforgeeks.org/can-virtual-functions-be-private-in-c/)

<br>


## [Abstract class & Pure Virtual Functions](https://www.youtube.com/watch?v=0IR_D1qZy2g)
1. A virtual function is not used for performing any task. It only serves as a placeholder.
2. A pure virtual function is a function declared in the base class that has no definition relative to the base class.
3. A class containing the pure virtual function cannot be used to declare the objects of its own, such classes are known as **abstract base classes**

#### A class containing atleast one pure virtual function is known as abstract class. The main objective of the abstract class and Virtual functions is to provide the traits to the derived classes and to create the base pointer used for achieving the runtime polymorphism.

<br>

Pure virtual function can be defined as:

```
virtual void display() = 0;   
```

Example:

```cpp
class Base {  
public:  
    virtual void show() = 0;  
};  


class Derived : public Base {  
public:  
    void show() {  
        cout << "Derived class is derived from the base class." << endl;  
    }  
};  


int main() {  
    Base *p;  
    Derived d;  
    
    p = &d;  
    p->show();  
    return 0;  
} 
```

```
Output: Derived class is derived from the base class
```

<br>

### Properties Of Abstract Class
1. We cannot create objects of Abstract class.
2. If some class inherits Abstract class, then it is compulsory to provide definitions for all pure virtual functions of abstract base class in derived class. That is Overriding should be compulsion.
3. An abstract class can contain Non-Virtual Function. They can be only called by object of derived class.

#### Note: If we do not want to override pure virtual function of base class in derived class. Then we should again declare it as pure virtual function in derived class, and then derived class will also become another abstract class.

<br>

### Why Abstract Class?
1. It promotes Generalization concept in OOPS. For ex: Let a Person Class, and its two child classes Student and Faculty. Now Person class does not have any existance on its own, but it has some properties which are inherited by both student and faculty. So there is No need to create object of person class, and it will only be used to pass some traits to its child class.
2. By creating abstract class (or pure virtual functions) we can enforce child classes to compulsory provide the definations for pure virtual functions.

<br>



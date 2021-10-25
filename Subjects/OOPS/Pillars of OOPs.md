# 4 Pillars of OOPS

Following are 4 pillars of OOPs:
1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

<br>

## @ Encapsulation
Encapsulation is a process of wrapping code and data together into a single unit, for example, a capsule which is mixed of several medicines. It provides you the control over the data. It is a way to achieve data hiding. The whole idea behind encapsulation is to hide the implementation details from users.

Encapsulation can be achieved by using private members and public member functions (setter-getters).

#### Advantages of encapsulation
1. It improves maintainability and flexibility and re-usability.
2. The fields can be made read-only (If we don’t define setter methods) or write-only (If we don’t define the getter methods).
3. User would not be knowing what is going on behind the scene. They would only be knowing that to update a field call set method and to read a field call get method but what these set and get methods are doing is purely hidden from them.

<br>

#### Real World Example
Capsule will be our class and various medicines filled under the capsule will be our data and member functions.

<br>

## @ Inheritance
Inheritance is a process in which one object acquires all the properties and behaviors of its parent object automatically. In such way, you can reuse, extend or modify the attributes and behaviors which are defined in other class.


#### Advantages of Inheritance
1. Code reusability.
2. One superclass can be used for the number of subclasses in a hierarchy.
3. No changes to be done in all classes; just do changes in base class only.
4. Inheritance avoids duplicity and data redundancy.


#### Real world example
Lets take an example of bank. RBI is the parent of all banks in India. It has some set of rules which needs to be followed by every bank. Hence RBI will be our base class and other banks will be derived class which inherits RBI properties.

<br>

#### Can Object Oriented Programming exists without inheritance ?
Yes, Go doesn’t have inheritance – instead composition, embedding and interfaces support code reuse and polymorphism.

<br>

### Visibility Modes in Inheritance

```
Base class visibility	                        Derived class visibility
--------------------------------------------------------------------------------------------------------
                            Public                      Private                     Protected           
--------------------------------------------------------------------------------------------------------  
Private                     Not Inherited               Not Inherited               Not Inherited       
Protected                   Protected                   Private                     Protected
Public                      Public                      Private                     Protected
```

1. Private members of base class are not inheritable.
2. Public members of base class are inherited in the visiblity mode that we specified during inheritance.
3. Protected members will never be inherited as public.

Note:  The default mode of visibility is private.

<br>

### Types of Inheritance

<br>

![img](https://image.slidesharecdn.com/cpp-programming-notes-150508030807-lva1-app6892/95/c-programming-41-638.jpg?cb=1431061806)

<br>

### Single Inheritance

```
class derived_class_name :: visibility-mode base_class_name  {  
    // --> body of the derived class.  
}  
```

```cpp
class Account {  
public:  
    float salary = 60000;   
};  

class Programmer: public Account {  
public:  
    float bonus = 5000;    
};       

int main(void) {  
    Programmer p1;  
    cout<<"Salary: "<<p1.salary<<endl;    
    cout<<"Bonus: "<<p1.bonus<<endl;    
    return 0;  
}  
```

```
Output:
Salary: 60000
Bonus: 5000
```

<br>

### Multilevel Inheritance
When one class inherits another class which is further inherited by another class, it is known as multi level inheritance. Inheritance is transitive so the last derived class acquires all the members of all its base classes.

**Real world Example : Family Tree**

```cpp
class Animal {  
public:  
    void eat() {   
        cout<<"Eating"<<endl;   
    }    
};  


class Dog: public Animal {    
public:  
    void bark(){  
        cout<<"Barking"<<endl;   
    }    
};  


class BabyDog: public Dog {    
public:  
    void weep() {  
        cout<<"Weeping";   
    }    
};   


int main(void) {  
    BabyDog d1;  
    d1.eat(); d1.bark(); d1.weep();  // --> d1 has access to all its base classes
    return 0;  
}  
```

```
Output:
Eating
Barking
Weeping
```

<br>

### Multiple Inheritance

**Real World Example : Maruti Suzuki creates Ertiga**


```
class D : visibility B-1, visibility B-2, ..  {  
    // --> Body of the class;  
}   
```

```cpp
class A {  
protected:  
    int a;  
public:  
    void get_a(int n) {  
        a = n;  
    }  
};  
  
  
class B {  
protected:  
    int b;  
public:  
    void get_b(int n) {  
        b = n;  
    }  
};  


class C : public A, public B {  
public:  
    void display() { 
        cout << "The value of a is : " << a <<endl;  
        cout << "The value of b is : " << b <<endl;  
        cout << "Addition of a and b is : " << a+b;  
    }  
};  


int main()  {  
   C c;  
   c.get_a(10);  
   c.get_b(20);  
   c.display();  
  
    return 0;  
}  
```

```
Output:
The value of a is : 10
The value of b is : 20
Addition of a and b is : 30
```

<br>

#### Ambiguity Resolution in Multiple Inheritance
Ambiguity can be occurred in using the multiple inheritance when a function with the same name occurs in more than one base class.

```cpp
class A {  
public:  
    void display() {  
        cout << "Class A" << endl;  
    }  
};  

class B {  
public:  
    void display() {  
        cout << "Class B" << endl;  
    }  
};  

class C : public A, public B {  
    void view() {  
        display();  
    }  
};  

int main() {  
    C c;  
    c.view();  
    return 0;  
}  
```

```
Output: reference to 'display' is ambiguous
```

**Solution**
The above issue can be resolved by using the class resolution operator with the function. In the above example, the derived class code can be rewritten as:

```cpp
class C : public A, public B {  
    void view() {  
        A :: display();         // --> Calling the display() function of class A.  
        B :: display();         // --> Calling the display() function of class B.  
    }  
};  
```

<br>

### Hybrid Inheritance

```cpp
class A {  
protected:  
    int a;  
public:  
    void get_a() {  
       cout << "Enter the value of 'a' : " << endl;  
       cin>>a;  
    }  
};  
  
class B : public A {  
protected:  
    int b;  
public:  
    void get_b() {  
        cout << "Enter the value of 'b' : " <<endl;  
        cin>>b;  
    }  
};  

class C {  
protected:  
    int c;  
public:  
    void get_c() {  
        cout << "Enter the value of c is : " << endl;  
        cin>>c;  
    }  
};  
  
class D : public B, public C {  
protected:  
    int d;  
public:  
    void mul() {  
        get_a();  
        get_b();  
        get_c();  
        cout << "Multiplication of a,b,c is : " <<a*b*c<< endl;  
    }  
};  


int main() {  
    D d;  
    d.mul();  
    return 0;  
}  
```

<br>

### [Virtual Inheritance](https://www.youtube.com/watch?v=GsK_4doAmpc)
Virtual inheritance is used when we are dealing with multiple inheritance but want to prevent multiple instances of same class appearing in inheritance hierarchy.

![img](https://miro.medium.com/max/700/1*vR45T1AB59vlEteqE7mHPA.png)

<br>

From above example we can see that “A” is inherited two times in D means an object of class “D” will contain two attributes of “a” (D::C::a and D::B::a). This problem is also known as **diamond problem**.

<br>

If we are inheriting virtually like:

![img](https://miro.medium.com/max/700/1*4nisaqYSLEbZ0kzQZ1nIww.png)

Now class A will act like a direct virtual parent of D (and not through B and C). Hence, only one copy of data members of class A will be created in D, which removes the compiler confusion.

<br>

### Aggregation (HAS-A Relationship)
Aggregation is a process in which one class defines another class as any entity reference. 

```cpp
class Address {  
public:     
    string addressLine, city, state;    
    
    Address(string addressLine, string city, string state) {    
        this->addressLine = addressLine;    
        this->city = city;    
        this->state = state;    
    }    
};  


class Employee {    
private:  
    Address* address;       // --> Employee HAS-A Address   
    
public:  
    int id; string name;    
    
    Employee(int id, string name, Address* address) {    
        this->id = id;    
        this->name = name;    
        this->address = address;    
    }    
    
    void display() {    
        cout<<id <<" "<<name<< " "<< address->addressLine<< " "<< address->city<< " "<<address->state<<endl;    
       }    
};   
   
   
int main(void) {  
    Address a1 = Address("C-146, Sec-15", "Noida", "UP");    
    Employee e1 = Employee(101, "Nakul", &a1);    
    e1.display();   
    return 0;  
}  
```

<br>

### Constructors With Inheritance
If we inherit a class from another class and create an object of the derived class, it is clear that the default constructor of the derived class will be invoked but before that the default constructor of all of the base classes will be invoke

#### Why the base class’s constructor is called on creating an object of derived class?
When a class is inherited from other, The data members and member functions of base class comes automatically in derived class based on the access specifier. When we create an object of derived class, all of the members of derived class must be initialized but the inherited members in derived class can only be initialized by the base class’s constructor as the definition of these members exists in base class only. This is why the constructor of base class is called first to initialize all the inherited members. 

<br>

#### Points to Remember (IMP)
1. If base class constructors do not have any argument, there is no need of any constructor in derived class.
2. If there are one or more arguments in base class constructor, derived class need to pass arguments to base class contructor.
3. If both base class and derived class has constructors, base class contructors will be executed first.
4. In multiple inheritance, base classes are constructed in order in which they appear in the class declaration.
5. In multilevel inheritance, constructors will get executed in order of inheritance.
6. The constructors of virtual base class will get executed before non virtual base class.
7. If there are multiple virtual base classes, they will get executed in the order they declared.

<br>

```cpp
class Base1{
    int data1;
    
public:
    Base1(int i){
        data1 = i;
        cout<<"Base1 class constructor called\n";
    }
};


class Base2{
    int data2;
    
public:
    Base2(int i){
        data2 = i;
        cout<<"Base2 class constructor called\n";
    }
};

class Derived : public Base1, public Base2{
    int derived1, derived2;
    
public:
    
    Derived (a, b, c, d) : Base1 (a), Base2 (b){
        derived1 = c;
        derived2 = d;
        cout<<"Derived class constructor called\n";
    }
}

int main(){
    Derived obj (10, 20, 30, 40);
    return 0;
}
```

```
Output:
Base1 class constructor called
Base2 class constructor called
Derived class constructor called
```

<br>

#### [Few Examples for order of execution of constructors](https://www.youtube.com/watch?v=qHrnTf5DOeI&list=PLu0W_9lII9agpFUAlPFe_VNSlXW5uE0YL&index=48)

```
Case1: 

Class B : public A{
    // --> order of execution of constructors -> first A() then B()
}


Case2:

Class A : public B, public C[
    // --> order of execution of constructors -> first B() then C() then A()
}


Case3:

Class A : public B, virtual public C[
    // --> order of execution of constructors -> first C() then B() then A()
}

```

<br>

**Note: Destructors in C++ are called in the opposite order of that of Constructors.**

<br>

![img](https://media.geeksforgeeks.org/wp-content/uploads/order-of-constructor.png)

<br>





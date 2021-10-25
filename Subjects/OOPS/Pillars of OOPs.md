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


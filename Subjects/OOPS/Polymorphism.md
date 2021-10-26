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

#### Ambiguity in Function Overloading
When the compiler is unable to decide which function is to be invoked among the overloaded function, this situation is known as function overloading.

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

```cpp
class Animal {  
public:  
    void eat(){    
        cout<<"Eating...";    
    }      
};   

class Dog: public Animal {    
public:  
    void eat() {    
        cout<<"Eating bread...";    
    }    
};  

int main(void) {  
    Dog d = Dog();    
    d.eat();  
    return 0;  
}
```

```
Output: Eating bread...
```

<br>

## Virtual Function
A C++ virtual function is a member function in the base class that you redefine in a derived class. It is declared using the virtual keyword. It is used to tell the compiler to perform dynamic linkage or late binding on the function.


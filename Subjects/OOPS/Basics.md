# OOPS Basics

<br>

### What is OOPS ?
OOP (object-oriented programming) is a programming paradigm which involves creation of classes and objects. The main aim of OOP is to bind data together and the functions that operate on them. It organizes software design around data, or objects, rather than functions and logic.

**Note: OOPs follow Bottom-Up Approach**

<br>

#### Real World Example
Lets take an example of mobile phones. Class refers to a category of some entity. Here Mobile will be our class, and its instances (or phones) will become our objects. Now one object is a collection of data and member functions. In our example, phone specs will be the object data and its features which defines the characteristic of any object will be our function such as take_photo(), call(), message(), shutdown() etc.

```
public class Phone{
    public string Name {get; set;}
    public int IMINumber {get; set;}
    public string Type {get; set;}
    
    public void Dial{
    }
    
    public void SendMassage{
    }
}
```

<br>

### Benefits of using OOPS ?
1. Modularity
2. Reusability
3. Productivity
4. Easily upgradable and scalable
5. Security
6. Flexibility due to Polymorphism

<br>

### Limitations of OOPs
1. The length of the programmes developed using OOP language is much larger than the procedural approach.
2. Programs are typically slower than procedure based programs, as they typically require more instructions to be executed.
3. Not suitable for all types of problems.

<br>

### OOPs is well suited for ?
This approach to programming is well-suited for programs that are large, complex and actively updated or maintained.

<br>

### What does keyword `Namespace` do ?
It defines a scope for the identifiers that are used in the program. 

<br>

### Scope Resolution Operator `::`
C and C++ are block structured language. Same variable names can be used to have different meanings in different blocks. The scope of the variable extends from the point of its declaration till the end of the block containing the declaration.

Global version of variable cannot be accessed from within the inner block. The scope resolution operator is used to uncover a hidden variable. 

```cpp
#include <iostream>
using namespace std;

int m = 10;     // --> global 

int main(){

    int m = 20;     // --> local to main()
    
    {
        int k = m;
        int m = 30;     // --> local to inner block
        
        cout<< k <<" "<< m <<" "<< ::m<<endl;
    }
    
    cout<< m << ::m<<endl;
    return 0;
}
```

Output:

20 30 10

20 10

Note: `::m` will always refer to global m

<br> 

### Cohesion Vs Coupling
Cohesion refers to what the class (or module) can do. Low cohesion would mean that the class does a great variety of actions - it is broad, unfocused on what it should do. High cohesion means that the class is focused on what it should be doing, i.e. only methods relating to the intention of the class.

Coupling refers to how related or dependent two classes/modules are toward each other. For low coupled classes, changing something major in one class should not affect the other. High coupling would make it difficult to change and maintain your code; since classes are closely knit together, making a change could require an entire system revamp.

<br>

### Memory management operators in C/C++
We use dynamic allocation techniques when it is not known in advance, how much memory space is needed. C uses `malloc()` and `calloc()` functions to allocate memory dynamically at runtime. Similarily it uses `free()` function to free dynamically allocated memory. 

In C++, an object can be created by `new` and destroyed by `delete`. 

<br>

**Advantages of new over malloc()**
1. It automatically computes size of data object.
2. It automatically returns correct pointer type, so no need to typecast.
3. Like any other operator, new and delete can be overloaded.

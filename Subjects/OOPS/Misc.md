# Misc

<br>

## C Vs C++
C++ can be said a superset of C. 

<br>

#### Similarities
1. The compilation of both the languages is similar.
2. They share the same basic syntax. Nearly all of C’s operators and keywords are also present in C++ and do the same thing.
3. Basic memory model of both is very close to the hardware.
4. Same notions of stack, heap, file-scope and static variables are present in both the languages.
5. Both languages does not support database connectivity.

<br>

#### Differences
1. **Major added features in C++ are Object-Oriented Programming, Exception Handling and rich C++ Library.**
2. C is a function-driven language whereas C++ is an object-driven language
3. Reference variables are not supported by C But supported in C++
4. C provides malloc() and calloc() functions for dynamic memory allocation, and free() for memory de-allocation. Whereas, C++ provides new operator for memory allocation and delete operator for memory de-allocation.
5. C structures don’t have access modifiers, But, C++ structures have access modifiers.
6. C follows top-down approach vs C++ follows bottom approach.


<br>

## [Top Down Vs Bottom Up Approach](https://www.geeksforgeeks.org/difference-between-bottom-up-model-and-top-down-model/)

#### Top-Down Approach
1. In this approach We focus on breaking up the problem into smaller parts.
2. Mainly used by structured programming language such as COBOL, Fortran, C, etc.
3. Contain redundancy.
4. Communications is less among modules.

<br>

#### Bottom-Up Approach
1. In bottom up approach, we solve smaller problems and integrate it as whole and complete the solution.
2. Mainly used by object oriented programming language such as C++, C#, Python.
3. Redundancy is minimized by using data encapsulation and data hiding.
4. More communications among modules.

<br>

## [Procedural Programming Vs OOPS](https://www.geeksforgeeks.org/differences-between-procedural-and-object-oriented-programming/)

<br>

## [What are Reference Variables?](https://www.geeksforgeeks.org/references-in-c/)
When a variable is declared as a reference, it becomes an alternative name for an existing variable. A variable can be declared as a reference by putting ‘&’ in the declaration. 

```cpp
#include<iostream>
using namespace std;
  
int main() {
  int x = 10;
  int& ref = x;             // --> ref is a reference to x.
  
  ref = 20;                 // --> Value of x is now changed to 20
  cout << "x = " << x << endl ;
  
  x = 30;                   // --> Value of x is now changed to 30
  cout << "ref = " << ref << endl ;
  
  return 0;
}
```
```
Output:  
x = 20  
ref = 30
```

<br>

## [Enums](https://www.geeksforgeeks.org/enumeration-enum-c/)
Enumeration (or enum) is a user defined data type in C. It is mainly used to assign names to integral constants, the names make a program easy to read and maintain.

```cpp
#include<iostream.h>
  
enum week{Mon, Tue, Wed, Thur, Fri, Sat, Sun};
  
int main(){
    enum week day;
    day = Wed;
    printf("%d",day);
    return 0;
} 
```

```
Output: 2
```

<br>

### Interesting facts about initialization of enum
1. Two enum names can have same value.
2. If we do not explicitly assign values to enum names, the compiler by default assigns values starting from 0. 
3. We can assign values to some name in any order. All unassigned names get value as value of previous name plus one.
4. The value assigned to enum names must be some integral constant

<br>

### Enum vs Macro
We can also use macros to define names constants. However, There are multiple advantages of using enum over macro when many related named constants have integral values.
1. Enums follow scope rules.
2. Enum variables are automatically assigned values.

<br>

## [This pointer](https://www.geeksforgeeks.org/this-pointer-in-c/)
Each object of a class gets its own copy of the data member. All-access the same function definition as present in the code segment. Then now question is that if only one copy of each member function exists and is used by multiple objects, how are the proper data members are accessed and updated?

The ‘this’ pointer is passed as a hidden argument to all nonstatic member function calls and is available as a local variable within the body of all nonstatic functions. ‘this’ pointer is not available in static member functions as static member functions can be called without any object (with class name). 

**Simply put, This refers to the current instance (object) of the class!**

Following are the situations where ‘this’ pointer is used:

1. When local variable’s name is same as member’s name

```cpp
class Test {
private:
   int x;
public:
   void setX (int x) {
       this->x = x;
   }
   
   void print() { cout << "x = " << x << endl; }
};
```

2. To return reference to the calling object

```cpp
Test& Test::func () {
   // Some processing
   return *this;
} 
```

<br>

## [Const](https://www.geeksforgeeks.org/const-keyword-in-cpp/)
Whenever const keyword is attached with any method(), variable, pointer variable, and with the object of a class it prevents that specific object/method()/variable to modify its data items value. There are a certain set of rules for the declaration and initialization of the constant variables:
1. The const variable cannot be left un-initialized at the time of the assignment.
2. It cannot be assigned value anywhere in the program.
3. Explicit value needed to be provided to the constant variable at the time of declaration of the constant variable.

![img](https://media.geeksforgeeks.org/wp-content/uploads/20201223212505/t1.png)

<br>

#### Const Keyword With Pointer Variables
1. When the pointer variable point to a const value:

```
const data_type* var_name;
```

2. When the const pointer variable point to the value:

```
data_type* const var_name;
```

3. When const pointer pointing to a const variable:

```
const data_type* const var_name;
```

<br>

## [Exception Handling in C++](https://www.youtube.com/watch?v=asekOytwNw4)

#### Difference between Error and Exception
Error refers to an illegal operation which results in the abnormal working of the program. Whereas exceptions refer to an unwanted event, which occurs during the execution of a program i.e at run time, that disrupts the normal flow of the program’s instructions.

<br>

**C++ provides following specialized keywords for this purpose:**
1. **try:** represents a block of code that can throw an exception.
2. **catch:** represents a block of code that is executed when a particular exception is thrown.
3. **throw:** Used to throw an exception. Also used to list the exceptions that a function throws, but doesn’t handle itself.

<br>

**Some Points to Note:**
1. The try and catch keywords come in pairs.
2. There is a special catch block called ‘catch all’ `catch(…)` that can be used to catch all types of exceptions.
3. Implicit type conversion for thrown variable doesn’t happen for primitive types
4. If an exception is thrown and not caught anywhere, the program terminates abnormally.
5. A derived class exception should be caught before a base class exception
6. C++ library has a standard exception class which is base class for all standard exceptions.
7. In C++, all exceptions are unchecked. Compiler doesn’t check whether an exception is caught or not.
8. When an exception is thrown, all objects created inside the enclosing try block are destructed before the control is transferred to catch block.



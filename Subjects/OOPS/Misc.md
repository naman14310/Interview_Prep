# Misc

<br>

## Some IMP Keywords in C++

### [This pointer](https://www.geeksforgeeks.org/this-pointer-in-c/)
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

### [Const](https://www.geeksforgeeks.org/const-keyword-in-cpp/)
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



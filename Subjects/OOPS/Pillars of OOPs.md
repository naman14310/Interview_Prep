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

<br>

#### Advantages of Inheritance
1. Code reusability.
2. One superclass can be used for the number of subclasses in a hierarchy.
3. No changes to be done in all classes; just do changes in base class only.
4. Inheritance avoids duplicity and data redundancy.

<br>

#### Real world example
Lets take an example of bank. RBI is the parent of all banks in India. It has some set of rules which needs to be followed by every bank. Hence RBI will be our base class and other banks will be derived class which inherits RBI properties.

<br>

### Visibility Modes in Inheritance

```
Base class visibility	                                    Derived class visibility
-----------------------------------------------------------------------------------------------------------------------------------
                                 Public	                            Private	                                Protected
-----------------------------------------------------------------------------------------------------------------------------------
Private	                      Not Inherited	                      Not Inherited	                          Not Inherited
Protected	                    Protected	                        Private	                                Protected
Public	                         Public	                            Private	                                Protected
```

1. Private members of base class are not inheritable.
2. Public members of base class are inherited in the visiblity mode that we specified during inheritance.
3. Protected members will never be inherited as public.

<br>

### Types of Inheritance
1. **Single Inheritance:** One derived class inherits from one base class.
2. **Multiple Inheritance:** One derived class inherits from many base classes.
3. **Multilevel Inheritance:** One derived class inherits from other derived classes.
4. **Hierarchal Inheritance:** More than one derived classes inherit from one base class.
5. **Hybrid Inheritance:** A combination of more than one type of inheritance.





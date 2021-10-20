# OOPS Basics

<br>

### What is OOPS ?
OOP (object-oriented programming) is a programming paradigm which involves creation of classes and objects. The main aim of OOP is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function.

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

### Why OOPs is so important ?
1. OOP provides code reusability (using Inheritance) which reduce the duplication of code.
2. OOP provides a clear modular structure (using Encapsulation) for programs which makes it good for defining abstract datatypes where implementation details are hidden and the unit has a clearly defined interface.
3. OOP makes it easy to maintain and modify existing code as new objects can be created with small differences to existing ones.
4. The principle of data hiding helps the programmer to build secure programs which cannot be invaded by the code in other parts of the program.
5. OOP systems can be easily upgraded from small to large systems.

<br>

### Limitations of OOPs
1. The length of the programmes developed using OOP language is much larger than the procedural approach.
2. Object-oriented programs are typically slower than procedurebased programs, as they typically require more instructions to be executed.
3. Not suitable for all types of problems.


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

#### Real World Example
Capsule will be our class and various medicines filled under the capsule will be our data and member functions.

<br>


## @ [Some Jargons in OOPs](https://javapapers.com/oops/association-aggregation-composition-abstraction-generalization-realization-dependency/)

#### Association
Association is a relationship between two objects. one-to-one, one-to-many, many-to-one, many-to-many all these words define an association between objects. Example: A Student and a Faculty are having an association.

<br>

#### Aggregation
Aggregation is a special case of association. A directional association between objects. When an object ‘has-a’ another object, then you have got an aggregation between them. Aggregation is also called a “Has-a” relationship.

<br>

#### Composition
A restricted aggregation is called composition. When an object contains the other object, if the contained object cannot exist without the existence of container object, then it is called composition. Example: A class contains students. A student cannot exist without a class. There exists composition between class and students.

<br>

#### Difference between aggregation and composition
Composition is more restrictive. When there is a composition between two objects, the composed object cannot exist without the other object. This restriction is not there in aggregation. Though one object can contain the other object, there is no condition that the composed object must exist. 

For example: A Library contains students and books. Relationship between library and student is aggregation. Relationship between library and book is composition. A student can exist without a library and therefore it is aggregation. A book cannot exist without a library and therefore its a composition.

<br>

#### Generalization
Generalization uses a “is-a” relationship from a specialization to the generalization class. Common structure and behaviour are used from the specializtion to the generalized class. Example: Consider there exists a class named Person. A student is a person. A faculty is a person. Therefore here the relationship between student and person, similarly faculty and person is generalization.

<br>

#### Realization
Realization is a relationship between the blueprint class and the object containing its respective implementation level details. Example: A particular model of a car ‘GTB Fiorano’ that implements the blueprint of a car realizes the abstraction.

<br>






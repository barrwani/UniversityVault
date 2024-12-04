# Static VS Dynamic Languages

#cs 

## Basic Comparison

**Dynamic** | **Static**
---------------|--------------
Objects are typed and referenced by variable| Variables are typed and object is stored without reference
New references made when object is reassigned regardless of type (for immutable objects) | Object can only be reassigned if it matches variable type, but it is stored in the same location without reference
For mutable objects (such as lists, dicts etc.), the reference is maintained when the object is updated | Most objects (except strings) are mutable


## Mutability

### Dynamic
In dynamic languages like Python, most objects are immutable, meaning that they cannot be changed and a new object must be made and referenced when a variable's value is changed. 

This process is expensive but it allows for variables to change type without needing to declare them as a new type. 

### Static
In static languages like Java and [[Basics of C++|C++]], most objects are mutable, meaning that they can be changed without needing a new object. References are not used, and the data is stored directly in the RAM allocation rather than the reference to the data. 

This is a more efficient process but this is because variables are typed, meaning that the type must be redeclared when changing the type of the variables contents. 

- When Immutable objects are modified locally, a new object will be referenced.

- When Mutable objects are modified locally, a global object will be referenced. 

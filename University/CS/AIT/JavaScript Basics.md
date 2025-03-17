# JavaScript Basics
#cs 

JS is a dynamically typed, weakly typed language. 

Dynamic typing is essentially no type declarations. 

Weak typing is when operations involving incompatible types will perform type coercions. 
- `>'a' + 100`
- `'a100'`

### Running JS

Using node
- Server side JS runtime and networking framework
- node as interpreter
- node as repl - read eval print loop

### String

- UTF-16
- Operators
	- '+' concatenation
	- `.repeat` 
- `""`, `''`
- backticks...template string

### Null vs Undefined

- Undefined is default value if there is no value
- Null is something you intentionally set something to
	- No object


# JavaScript Key Concepts

## Variable Declarations (`var`, `let`, `const`)

- `var`: Function-scoped, can be redeclared, hoisted but not block-scoped.
    
- `let`: Block-scoped, cannot be redeclared in the same scope, hoisted but not initialized.
    
- `const`: Block-scoped, must be initialized at declaration, cannot be reassigned.
    

### Example:

```
function example() {
    if (true) {
        var x = 10;
        let y = 20;
        const z = 30;
    }
    console.log(x); // ✅ 10 (function-scoped)
    console.log(y); // ❌ ReferenceError (block-scoped)
    console.log(z); // ❌ ReferenceError (block-scoped)
}
```

## Undefined vs ReferenceError

- `undefined`: A declared variable that has not been assigned a value.
    
- `ReferenceError`: Accessing a variable that has not been declared.


### Example:

```
console.log(a); // ❌ ReferenceError: a is not defined
let b;
console.log(b); // ✅ undefined (b is declared but not assigned)
```

## Objects

- Objects store key-value pairs and can include methods.
    
- Properties can be mutable or immutable.
    
- **Constructor functions** create multiple instances of an object.
    
- **Holes** in arrays exist when elements are missing.
    

### Example:

```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
const person = new Person("Alice", 25);
console.log(person.name); // Alice
```

## Functions & Higher-Order Functions

- Functions can be assigned to variables, passed as arguments, and returned from other functions.
    
- **Higher-order functions** take functions as parameters or return functions.
    

### Example:

```
const add = (a, b) => a + b;
const applyFunction = (fn, x, y) => fn(x, y);
console.log(applyFunction(add, 2, 3)); // 5
```

## Strings, Arrays, Mutability & Variable Parameters

- Strings are immutable; arrays are mutable.
    
- **Spread (**`**...**`**)** and **rest parameters** help handle variable arguments.
    

### Example:

```
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // [1, 2, 3, 4]
```

## Modules & Require

- **CommonJS (**`**require**`**)** is used in Node.js.
    
- **ES Modules (**`**import/export**`**)** are used in modern JavaScript.
    

### Example (CommonJS):

```
const fs = require("fs");
```

### Example (ES Modules):

```
import { readFile } from "fs/promises";
```

## File IO with `fs.readFile`

- Used for reading files asynchronously in Node.js.
    

### Example:

```
const fs = require("fs");
fs.readFile("data.txt", "utf8", (err, data) => {
    if (err) throw err;
    console.log(data);
});
```

## `this` in JavaScript

- `this` refers to the object that calls the function.
    
- Arrow functions do **not** bind `this`.
    

### Example:

```
class Car {
    constructor(brand) {
        this.brand = brand;
    }
    showBrand() {
        console.log(this.brand);
    }
}
const myCar = new Car("Toyota");
myCar.showBrand(); // Toyota
```

## Objects, Prototypes & Classes

- JavaScript uses **prototypal inheritance**.
    
- Classes in ES6 are syntactic sugar over prototypes.
    

### Example:

```
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}
class Dog extends Animal {
    speak() {
        console.log(`${this.name} barks.`);
    }
}
const dog = new Dog("Buddy");
dog.speak(); // Buddy barks.
```

## Where Not to Use Arrow Functions

- Arrow functions **do not** have their own `this`, making them unsuitable for object methods.
    

### Example:

```
const obj = {
    name: "Test",
    showName: () => {
        console.log(this.name); // ❌ Undefined
    }
};
obj.showName();
```

## Summary

|Concept|Explanation|
|---|---|
|`var` vs `let` vs `const`|Scope and reassignment differences|
|Undefined vs ReferenceError|Undefined is declared but uninitialized; ReferenceError is undeclared|
|Objects & Constructors|Objects store key-value pairs; constructors create instances|
|Functions & Higher-Order|Functions can be passed as arguments or returned|
|Strings, Arrays, Mutability|Strings immutable; arrays mutable; spread/rest for variable args|
|Modules (`require`, `import`)|CommonJS (`require`) vs ES Modules (`import/export`)|
|File IO (`fs.readFile`)|Reading files asynchronously in Node.js|
|`this` & Arrow Functions|Arrow functions do not bind `this`|
|Objects & Prototypes|JavaScript uses prototypal inheritance|
|Where Not to Use Arrow Functions|Avoid in object methods where `this` is needed|

These notes cover the basics of JavaScript concepts. Let me know if you need more details! 🚀
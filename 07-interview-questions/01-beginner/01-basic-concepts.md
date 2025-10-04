# JavaScript Interview Questions - Beginner Level

## Basic Concepts & Fundamentals

### 1. What is JavaScript and how is it different from Java?

**Answer:**
JavaScript is a high-level, interpreted programming language primarily used for web development. It's dynamically typed and runs in browsers and on servers (Node.js).

**Key Differences from Java:**
- **Compilation**: JavaScript is interpreted, Java is compiled
- **Typing**: JavaScript is dynamically typed, Java is statically typed
- **Platform**: JavaScript runs in browsers, Java runs on JVM
- **Syntax**: JavaScript is more flexible, Java is more strict

**Example:**
```javascript
// JavaScript - dynamically typed
let x = 5;        // number
x = "hello";      // string - no error
x = true;         // boolean - no error

// Java equivalent would be:
// int x = 5;     // must stay as integer
```

### 2. Explain the difference between `var`, `let`, and `const`.

**Answer:**
These are three ways to declare variables in JavaScript with different scoping and mutability rules.

**Key Differences:**

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function-scoped | Block-scoped | Block-scoped |
| Hoisting | Hoisted and initialized with `undefined` | Hoisted but not initialized | Hoisted but not initialized |
| Re-declaration | Allowed | Not allowed | Not allowed |
| Re-assignment | Allowed | Allowed | Not allowed |

**Examples:**
```javascript
// var - function scoped, can be redeclared
function example() {
  var x = 1;
  if (true) {
    var x = 2; // Same variable, overwrites
    console.log(x); // 2
  }
  console.log(x); // 2
}

// let - block scoped, cannot be redeclared
function example2() {
  let y = 1;
  if (true) {
    let y = 2; // Different variable
    console.log(y); // 2
  }
  console.log(y); // 1
}

// const - block scoped, cannot be reassigned
const z = 1;
// z = 2; // Error: Assignment to constant variable

// But const objects can have properties modified
const obj = { name: 'John' };
obj.name = 'Jane'; // This is allowed
// obj = {}; // This would be an error
```

### 3. What are the different data types in JavaScript?

**Answer:**
JavaScript has 8 data types: 7 primitive types and 1 object type.

**Primitive Types:**
1. **number** - integers and floating-point numbers
2. **string** - text data
3. **boolean** - true/false
4. **undefined** - variable declared but not assigned
5. **null** - intentional absence of value
6. **symbol** - unique identifier (ES6)
7. **bigint** - large integers (ES2020)

**Object Type:**
- **object** - collections of key-value pairs

**Examples:**
```javascript
// Primitive types
let num = 42;                    // number
let str = "Hello";               // string
let bool = true;                 // boolean
let undef = undefined;           // undefined
let nul = null;                  // null
let sym = Symbol('id');          // symbol
let big = 123n;                  // bigint

// Object type
let obj = { name: 'John' };      // object
let arr = [1, 2, 3];            // object (arrays are objects)
let func = function() {};        // object (functions are objects)

// Check types
console.log(typeof num);    // "number"
console.log(typeof str);    // "string"
console.log(typeof bool);   // "boolean"
console.log(typeof undef);  // "undefined"
console.log(typeof nul);    // "object" (quirk of JavaScript)
console.log(typeof sym);    // "symbol"
console.log(typeof big);    // "bigint"
console.log(typeof obj);    // "object"
console.log(typeof func);   // "function"
```

### 4. What is the difference between `==` and `===`?

**Answer:**
`==` (loose equality) performs type coercion before comparison, while `===` (strict equality) compares both value and type without coercion.

**Examples:**
```javascript
// Loose equality (==) - performs type coercion
console.log(5 == "5");     // true (string "5" converted to number 5)
console.log(true == 1);    // true (boolean true converted to 1)
console.log(null == undefined); // true
console.log(0 == false);   // true
console.log("" == false);  // true

// Strict equality (===) - no type coercion
console.log(5 === "5");    // false (different types)
console.log(true === 1);   // false (different types)
console.log(null === undefined); // false
console.log(0 === false);  // false
console.log("" === false); // false

// Best practice: Always use === unless you specifically need type coercion
```

### 5. What is hoisting in JavaScript?

**Answer:**
Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their containing scope during compilation, before code execution.

**Key Points:**
- Only declarations are hoisted, not initializations
- `var` declarations are hoisted and initialized with `undefined`
- `let` and `const` are hoisted but not initialized (temporal dead zone)
- Function declarations are fully hoisted

**Examples:**
```javascript
// Variable hoisting
console.log(x); // undefined (not ReferenceError)
var x = 5;

// Equivalent to:
var x;
console.log(x); // undefined
x = 5;

// let and const hoisting (temporal dead zone)
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;

// Function hoisting
sayHello(); // "Hello!" - works because function is fully hoisted
function sayHello() {
  console.log("Hello!");
}

// Function expression hoisting
sayGoodbye(); // TypeError: sayGoodbye is not a function
var sayGoodbye = function() {
  console.log("Goodbye!");
};
```

### 6. What is the difference between `null` and `undefined`?

**Answer:**
Both represent "no value" but in different contexts:

- **`undefined`**: Variable declared but not assigned, missing object properties, function parameters not provided
- **`null`**: Intentional absence of value, explicitly set by developer

**Examples:**
```javascript
// undefined scenarios
let x;
console.log(x); // undefined

let obj = {};
console.log(obj.property); // undefined

function test(a) {
  console.log(a); // undefined if no argument passed
}

// null scenarios
let y = null; // explicitly set to null
console.log(y); // null

// Type checking
console.log(typeof undefined); // "undefined"
console.log(typeof null);      // "object" (quirk of JavaScript)

// Equality
console.log(null == undefined);  // true (loose equality)
console.log(null === undefined); // false (strict equality)

// Practical usage
function findUser(id) {
  // If user not found, return null (intentional absence)
  if (!userExists(id)) {
    return null;
  }
  return getUser(id);
}
```

### 7. What are the different ways to create objects in JavaScript?

**Answer:**
There are several ways to create objects in JavaScript:

**1. Object Literal:**
```javascript
const person = {
  name: 'John',
  age: 30,
  greet: function() {
    console.log(`Hello, I'm ${this.name}`);
  }
};
```

**2. Constructor Function:**
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.greet = function() {
    console.log(`Hello, I'm ${this.name}`);
  };
}

const person = new Person('John', 30);
```

**3. Object.create():**
```javascript
const personPrototype = {
  greet: function() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

const person = Object.create(personPrototype);
person.name = 'John';
person.age = 30;
```

**4. ES6 Classes:**
```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
}

const person = new Person('John', 30);
```

**5. Factory Function:**
```javascript
function createPerson(name, age) {
  return {
    name,
    age,
    greet() {
      console.log(`Hello, I'm ${this.name}`);
    }
  };
}

const person = createPerson('John', 30);
```

### 8. What is the difference between function declaration and function expression?

**Answer:**
The main difference is in hoisting behavior and when the function becomes available.

**Function Declaration:**
- Hoisted completely (can be called before declaration)
- Must have a name
- Creates a function variable in the current scope

**Function Expression:**
- Not hoisted (cannot be called before assignment)
- Can be anonymous or named
- Assigned to a variable

**Examples:**
```javascript
// Function Declaration - hoisted
sayHello(); // Works! Function is hoisted
function sayHello() {
  console.log("Hello!");
}

// Function Expression - not hoisted
sayGoodbye(); // TypeError: sayGoodbye is not a function
var sayGoodbye = function() {
  console.log("Goodbye!");
};

// Named Function Expression
var factorial = function fact(n) {
  if (n <= 1) return 1;
  return n * fact(n - 1);
};

// Arrow Function Expression (ES6)
const multiply = (a, b) => a * b;

// IIFE (Immediately Invoked Function Expression)
(function() {
  console.log("This runs immediately!");
})();
```

### 9. What is the `this` keyword in JavaScript?

**Answer:**
`this` refers to the object that is currently executing the function. Its value depends on how the function is called.

**Rules for `this` binding:**
1. **Default binding**: `this` refers to global object (or `undefined` in strict mode)
2. **Implicit binding**: `this` refers to the object that calls the function
3. **Explicit binding**: `this` is explicitly set using `call()`, `apply()`, or `bind()`
4. **New binding**: `this` refers to the newly created object when using `new`

**Examples:**
```javascript
// 1. Default binding
function sayName() {
  console.log(this.name); // undefined in strict mode, global.name otherwise
}
sayName();

// 2. Implicit binding
const person = {
  name: 'John',
  sayName: function() {
    console.log(this.name); // 'John'
  }
};
person.sayName();

// 3. Explicit binding
function greet() {
  console.log(`Hello, ${this.name}`);
}

const person1 = { name: 'Alice' };
const person2 = { name: 'Bob' };

greet.call(person1);  // "Hello, Alice"
greet.apply(person2); // "Hello, Bob"

const boundGreet = greet.bind(person1);
boundGreet(); // "Hello, Alice"

// 4. New binding
function Person(name) {
  this.name = name;
  this.sayName = function() {
    console.log(this.name);
  };
}

const john = new Person('John');
john.sayName(); // "John"

// Arrow functions don't have their own 'this'
const obj = {
  name: 'John',
  regularFunction: function() {
    console.log(this.name); // 'John'
  },
  arrowFunction: () => {
    console.log(this.name); // undefined (inherits from outer scope)
  }
};
```

### 10. What are arrays and how do you work with them?

**Answer:**
Arrays are ordered collections of data that can hold multiple values of any type. They are objects with numeric indices.

**Creating Arrays:**
```javascript
// Different ways to create arrays
const arr1 = [1, 2, 3, 4, 5];
const arr2 = new Array(1, 2, 3, 4, 5);
const arr3 = Array.of(1, 2, 3, 4, 5);
const arr4 = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
```

**Common Array Methods:**
```javascript
const fruits = ['apple', 'banana', 'orange'];

// Adding/Removing elements
fruits.push('grape');        // Add to end
fruits.pop();               // Remove from end
fruits.unshift('kiwi');     // Add to beginning
fruits.shift();             // Remove from beginning

// Finding elements
fruits.indexOf('banana');   // 1
fruits.includes('apple');   // true
fruits.find(fruit => fruit.startsWith('a')); // 'apple'

// Iteration methods
fruits.forEach(fruit => console.log(fruit));
const upperFruits = fruits.map(fruit => fruit.toUpperCase());
const longFruits = fruits.filter(fruit => fruit.length > 5);

// Array length
console.log(fruits.length); // 3
```

**Array Destructuring:**
```javascript
const [first, second, ...rest] = ['apple', 'banana', 'orange', 'grape'];
console.log(first);  // 'apple'
console.log(rest);   // ['orange', 'grape']
```

## Practice Questions

1. **What will be the output of this code?**
```javascript
console.log(typeof typeof 1);
```

2. **Explain the output:**
```javascript
var a = 1;
function test() {
  console.log(a);
  var a = 2;
}
test();
```

3. **What's the difference between these two function calls?**
```javascript
function test() {
  console.log(this);
}
test();
new test();
```

4. **Predict the output:**
```javascript
console.log([] + [] + 'foo'.split(''));
```

5. **What will this return?**
```javascript
console.log(0.1 + 0.2 === 0.3);
```

## Key Takeaways

- Master the fundamentals: variables, data types, operators
- Understand hoisting and scope
- Know the difference between `==` and `===`
- Understand `this` binding rules
- Practice with arrays and objects
- Always use `const` and `let` instead of `var`
- Understand the difference between function declarations and expressions

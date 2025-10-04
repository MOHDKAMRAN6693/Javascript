# JavaScript Interview Questions - Beginner Level

## Functions & Scope

### 1. What is a function in JavaScript and how do you create one?

**Answer:**
A function is a reusable block of code that performs a specific task. Functions can accept parameters and return values.

**Ways to create functions:**

**1. Function Declaration:**
```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
console.log(greet('John')); // "Hello, John!"
```

**2. Function Expression:**
```javascript
const greet = function(name) {
  return `Hello, ${name}!`;
};
console.log(greet('John')); // "Hello, John!"
```

**3. Arrow Function (ES6):**
```javascript
const greet = (name) => {
  return `Hello, ${name}!`;
};

// Shorter syntax for single expression
const greet = name => `Hello, ${name}!`;
console.log(greet('John')); // "Hello, John!"
```

**4. Constructor Function:**
```javascript
const greet = new Function('name', 'return `Hello, ${name}!`');
console.log(greet('John')); // "Hello, John!"
```

### 2. What is function scope and how does it work?

**Answer:**
Function scope means that variables declared inside a function are only accessible within that function and any nested functions.

**Examples:**
```javascript
function outerFunction() {
  const outerVariable = 'I am outside';
  
  function innerFunction() {
    const innerVariable = 'I am inside';
    console.log(outerVariable); // Can access outer variable
    console.log(innerVariable); // Can access inner variable
  }
  
  innerFunction();
  // console.log(innerVariable); // Error: innerVariable is not defined
}

outerFunction();
```

**Scope Chain:**
```javascript
const globalVar = 'Global';

function level1() {
  const level1Var = 'Level 1';
  
  function level2() {
    const level2Var = 'Level 2';
    
    function level3() {
      const level3Var = 'Level 3';
      
      // Can access all variables from outer scopes
      console.log(globalVar);  // "Global"
      console.log(level1Var);  // "Level 1"
      console.log(level2Var);  // "Level 2"
      console.log(level3Var);  // "Level 3"
    }
    
    level3();
  }
  
  level2();
}

level1();
```

### 3. What is the difference between parameters and arguments?

**Answer:**
- **Parameters**: Variables defined in the function declaration
- **Arguments**: Actual values passed to the function when calling it

**Examples:**
```javascript
// Parameters: name, age
function createUser(name, age) {
  return {
    name: name,
    age: age,
    greet: function() {
      console.log(`Hello, I'm ${this.name}`);
    }
}

// Arguments: "John", 25
const user = createUser("John", 25);
user.greet(); // "Hello, I'm John"

// Function with default parameters
function greet(name = 'Guest', greeting = 'Hello') {
  return `${greeting}, ${name}!`;
}

console.log(greet()); // "Hello, Guest!"
console.log(greet('Alice')); // "Hello, Alice!"
console.log(greet('Bob', 'Hi')); // "Hi, Bob!"
```

### 4. What are higher-order functions?

**Answer:**
Higher-order functions are functions that either:
1. Take other functions as arguments
2. Return functions as their result
3. Both

**Examples:**
```javascript
// 1. Function that takes another function as argument
function calculate(operation, a, b) {
  return operation(a, b);
}

function add(x, y) { return x + y; }
function multiply(x, y) { return x * y; }

console.log(calculate(add, 5, 3)); // 8
console.log(calculate(multiply, 5, 3)); // 15

// 2. Function that returns another function
function createMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 3. Built-in higher-order functions
const numbers = [1, 2, 3, 4, 5];

// map - transforms each element
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - selects elements based on condition
const evens = numbers.filter(x => x % 2 === 0);
console.log(evens); // [2, 4]

// reduce - reduces array to single value
const sum = numbers.reduce((acc, x) => acc + x, 0);
console.log(sum); // 15
```

### 5. What is the difference between `call()`, `apply()`, and `bind()`?

**Answer:**
All three methods are used to control the `this` context of functions, but they work differently:

**`call()`**: Calls function immediately with specified `this` and individual arguments
**`apply()`**: Calls function immediately with specified `this` and array of arguments
**`bind()`**: Returns new function with bound `this` and arguments, doesn't call immediately

**Examples:**
```javascript
const person = {
  name: 'John',
  greet: function(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
  }
};

const anotherPerson = {
  name: 'Alice'
};

// call() - individual arguments
person.greet.call(anotherPerson, 'Hello', '!'); // "Hello, I'm Alice!"

// apply() - array of arguments
person.greet.apply(anotherPerson, ['Hi', '.']); // "Hi, I'm Alice."

// bind() - returns new function
const boundGreet = person.greet.bind(anotherPerson, 'Hey');
boundGreet('!'); // "Hey, I'm Alice!"

// Practical example with event handlers
class Button {
  constructor(name) {
    this.name = name;
    this.element = document.createElement('button');
    this.element.textContent = name;
    
    // Without bind, 'this' would be the button element
    this.element.addEventListener('click', this.handleClick.bind(this));
  }
  
  handleClick() {
    console.log(`Button ${this.name} was clicked!`);
  }
}
```

### 6. What are closures in JavaScript?

**Answer:**
A closure is a function that has access to variables in its outer (enclosing) scope even after the outer function has returned. Closures are created every time a function is created.

**Examples:**
```javascript
// Basic closure
function outerFunction(x) {
  // Outer function's variable
  
  function innerFunction(y) {
    // Inner function has access to outer function's variable
    return x + y;
  }
  
  return innerFunction;
}

const addFive = outerFunction(5);
console.log(addFive(3)); // 8
console.log(addFive(10)); // 15

// Closure with private variables
function createCounter() {
  let count = 0; // Private variable
  
  return {
    increment: function() {
      count++;
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    getCount: function() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
// console.log(counter.count); // undefined - count is private
```

**Common Closure Patterns:**
```javascript
// Module pattern
const calculator = (function() {
  let result = 0;
  
  return {
    add: function(x) {
      result += x;
      return this;
    },
    multiply: function(x) {
      result *= x;
      return this;
    },
    getResult: function() {
      return result;
    },
    reset: function() {
      result = 0;
      return this;
    }
  };
})();

calculator.add(5).multiply(2).getResult(); // 10

// Function factory
function createValidator(min, max) {
  return function(value) {
    return value >= min && value <= max;
  };
}

const validateAge = createValidator(18, 65);
console.log(validateAge(25)); // true
console.log(validateAge(70)); // false
```

### 7. What is the difference between function declaration and arrow function?

**Answer:**
Arrow functions are a shorter syntax for writing functions, but they have several important differences:

**Key Differences:**

| Feature | Function Declaration | Arrow Function |
|---------|-------------------|----------------|
| `this` binding | Has its own `this` | Inherits `this` from outer scope |
| `arguments` object | Has `arguments` | No `arguments` object |
| Hoisting | Fully hoisted | Not hoisted |
| Constructor | Can be used as constructor | Cannot be used as constructor |
| `prototype` | Has `prototype` property | No `prototype` property |

**Examples:**
```javascript
// this binding difference
const obj = {
  name: 'John',
  
  regularFunction: function() {
    console.log(this.name); // 'John'
    
    // Nested function loses 'this'
    function nested() {
      console.log(this.name); // undefined (or global.name)
    }
    nested();
  },
  
  arrowFunction: function() {
    console.log(this.name); // 'John'
    
    // Arrow function inherits 'this'
    const nested = () => {
      console.log(this.name); // 'John'
    };
    nested();
  }
};

obj.regularFunction();
obj.arrowFunction();

// arguments object
function regularFunc() {
  console.log(arguments); // [1, 2, 3]
}

const arrowFunc = () => {
  console.log(arguments); // ReferenceError: arguments is not defined
};

regularFunc(1, 2, 3);
// arrowFunc(1, 2, 3); // Error

// Constructor
function RegularConstructor(name) {
  this.name = name;
}

const ArrowConstructor = (name) => {
  this.name = name;
};

const regular = new RegularConstructor('John'); // Works
// const arrow = new ArrowConstructor('John'); // TypeError: ArrowConstructor is not a constructor
```

### 8. What are default parameters and how do you use them?

**Answer:**
Default parameters allow you to set default values for function parameters when no argument is provided or when `undefined` is passed.

**Examples:**
```javascript
// Basic default parameters
function greet(name = 'Guest', greeting = 'Hello') {
  return `${greeting}, ${name}!`;
}

console.log(greet()); // "Hello, Guest!"
console.log(greet('John')); // "Hello, John!"
console.log(greet('John', 'Hi')); // "Hi, John!"

// Default parameters can be expressions
function createUser(name, age = 18, id = Math.random().toString(36)) {
  return { name, age, id };
}

console.log(createUser('Alice')); // { name: 'Alice', age: 18, id: '0.abc123' }

// Default parameters can reference other parameters
function createRectangle(width, height = width) {
  return { width, height };
}

console.log(createRectangle(10)); // { width: 10, height: 10 }
console.log(createRectangle(10, 20)); // { width: 10, height: 20 }

// Default parameters are evaluated each time
function addTimestamp(message = `Message at ${new Date().toISOString()}`) {
  return message;
}

console.log(addTimestamp()); // "Message at 2024-01-15T10:30:00.000Z"
console.log(addTimestamp()); // "Message at 2024-01-15T10:30:01.000Z" (different timestamp)
```

### 9. What is the rest parameter and spread operator?

**Answer:**
The rest parameter (`...`) allows you to represent an indefinite number of arguments as an array. The spread operator (`...`) allows you to expand an iterable (array, string) into individual elements.

**Rest Parameter:**
```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

// Rest parameter must be last
function example(first, second, ...rest) {
  console.log(first);  // 1
  console.log(second); // 2
  console.log(rest);    // [3, 4, 5]
}

example(1, 2, 3, 4, 5);
```

**Spread Operator:**
```javascript
// Array spreading
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Object spreading
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const combinedObj = { ...obj1, ...obj2 };
console.log(combinedObj); // { a: 1, b: 2, c: 3, d: 4 }

// Function calls
function add(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(add(...numbers)); // 6

// String spreading
const str = 'hello';
const chars = [...str];
console.log(chars); // ['h', 'e', 'l', 'l', 'o']
```

### 10. What is function currying?

**Answer:**
Currying is a technique where a function with multiple arguments is transformed into a sequence of functions, each with a single argument.

**Examples:**
```javascript
// Regular function
function add(a, b, c) {
  return a + b + c;
}

// Curried version
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

console.log(curriedAdd(1)(2)(3)); // 6

// Arrow function currying
const curriedAddArrow = a => b => c => a + b + c;
console.log(curriedAddArrow(1)(2)(3)); // 6

// Partial application
const addOne = curriedAdd(1);
const addOneAndTwo = addOne(2);
console.log(addOneAndTwo(3)); // 6

// Generic curry function
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...nextArgs) {
        return curried.apply(this, args.concat(nextArgs));
      };
    }
  };
}

const curriedMultiply = curry((a, b, c) => a * b * c);
console.log(curriedMultiply(2)(3)(4)); // 24
console.log(curriedMultiply(2, 3)(4)); // 24
console.log(curriedMultiply(2)(3, 4)); // 24
```

## Practice Questions

1. **What will be the output?**
```javascript
function test() {
  console.log(a);
  var a = 1;
}
test();
```

2. **Explain the output:**
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

3. **What's the difference between these two?**
```javascript
const obj = {
  name: 'John',
  greet: function() {
    console.log(this.name);
  },
  greetArrow: () => {
    console.log(this.name);
  }
};
obj.greet();
obj.greetArrow();
```

4. **Predict the output:**
```javascript
function outer() {
  let x = 1;
  return function inner() {
    console.log(x++);
  };
}
const fn = outer();
fn();
fn();
```

5. **What will this return?**
```javascript
const arr = [1, 2, 3, 4, 5];
const result = arr.map(x => x * 2).filter(x => x > 5);
console.log(result);
```

## Key Takeaways

- Functions are first-class citizens in JavaScript
- Understand scope and the scope chain
- Master closures for data privacy
- Know the differences between function declarations and expressions
- Understand `this` binding and how to control it
- Practice with higher-order functions and array methods
- Learn closures for creating private variables and modules
- Understand currying for functional programming patterns

# 24 - Functional Programming

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. JavaScript supports functional programming concepts through higher-order functions, immutability, and pure functions.

## 📚 Table of Contents
- [Core Concepts](#core-concepts)
- [Pure Functions](#pure-functions)
- [Immutability](#immutability)
- [Higher-Order Functions](#higher-order-functions)
- [Array Methods](#array-methods)
- [Function Composition](#function-composition)
- [Currying](#currying)
- [Closures in Functional Programming](#closures-in-functional-programming)
- [Recursion](#recursion)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## Core Concepts

Functional programming emphasizes:
- **Pure functions**: Functions that always return the same output for the same input
- **Immutability**: Data that doesn't change after creation
- **Higher-order functions**: Functions that take or return other functions
- **Function composition**: Combining simple functions to create complex ones

## Pure Functions

Pure functions have no side effects and always return the same output for the same input.

```javascript
// Pure function - always returns the same output for the same input
function add(a, b) {
  return a + b;
}

// Impure function - has side effects
let counter = 0;
function impureAdd(a, b) {
  counter++; // Side effect
  console.log('Adding numbers'); // Side effect
  return a + b;
}

// Pure function - no side effects
function multiply(x, y) {
  return x * y;
}

// Impure function - depends on external state
const taxRate = 0.1;
function calculateTax(amount) {
  return amount * taxRate; // Depends on external variable
}

// Pure version
function calculateTaxPure(amount, rate) {
  return amount * rate;
}
```

## Immutability

Immutability means data doesn't change after creation. In JavaScript, we can achieve this by creating new objects instead of modifying existing ones.

```javascript
// Mutable approach (avoid)
const person = { name: 'John', age: 30 };
person.age = 31; // Mutates the original object

// Immutable approach
const person = { name: 'John', age: 30 };
const updatedPerson = { ...person, age: 31 }; // Creates new object

// For arrays
const numbers = [1, 2, 3, 4, 5];

// Mutable (avoid)
numbers.push(6); // Modifies original array

// Immutable
const newNumbers = [...numbers, 6]; // Creates new array
const doubledNumbers = numbers.map(n => n * 2); // Creates new array

// For nested objects
const user = {
  name: 'John',
  address: {
    city: 'New York',
    country: 'USA'
  }
};

// Immutable update
const updatedUser = {
  ...user,
  address: {
    ...user.address,
    city: 'Boston'
  }
};
```

## Higher-Order Functions

Higher-order functions are functions that take other functions as arguments or return functions.

```javascript
// Function that takes another function as argument
function withLogging(fn) {
  return function(...args) {
    console.log(`Calling function with arguments:`, args);
    const result = fn(...args);
    console.log(`Function returned:`, result);
    return result;
  };
}

// Usage
const add = (a, b) => a + b;
const loggedAdd = withLogging(add);
console.log(loggedAdd(2, 3)); // Logs the call and result

// Function that returns another function
function createMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Function that takes and returns a function
function compose(f, g) {
  return function(x) {
    return f(g(x));
  };
}

const addOne = x => x + 1;
const multiplyByTwo = x => x * 2;
const addOneThenDouble = compose(multiplyByTwo, addOne);

console.log(addOneThenDouble(5)); // (5 + 1) * 2 = 12
```

## Array Methods

JavaScript provides powerful array methods that support functional programming.

### map()
Transforms each element in an array.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

const users = [
  { name: 'John', age: 30 },
  { name: 'Jane', age: 25 },
  { name: 'Bob', age: 35 }
];

const names = users.map(user => user.name);
console.log(names); // ['John', 'Jane', 'Bob']
```

### filter()
Creates a new array with elements that pass a test.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const evens = numbers.filter(n => n % 2 === 0);
console.log(evens); // [2, 4, 6, 8, 10]

const adults = users.filter(user => user.age >= 18);
console.log(adults); // All users (all are adults)
```

### reduce()
Reduces an array to a single value.

```javascript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

const product = numbers.reduce((acc, n) => acc * n, 1);
console.log(product); // 120

// Grouping
const words = ['apple', 'banana', 'cherry', 'date'];
const grouped = words.reduce((acc, word) => {
  const firstLetter = word[0];
  if (!acc[firstLetter]) {
    acc[firstLetter] = [];
  }
  acc[firstLetter].push(word);
  return acc;
}, {});
console.log(grouped); // { a: ['apple'], b: ['banana'], c: ['cherry'], d: ['date'] }
```

### forEach()
Executes a function for each array element.

```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.forEach(n => console.log(n * 2)); // Logs: 2, 4, 6, 8, 10
```

### find() and findIndex()
Find the first element that satisfies a condition.

```javascript
const users = [
  { id: 1, name: 'John', age: 30 },
  { id: 2, name: 'Jane', age: 25 },
  { id: 3, name: 'Bob', age: 35 }
];

const user = users.find(u => u.age > 30);
console.log(user); // { id: 3, name: 'Bob', age: 35 }

const index = users.findIndex(u => u.name === 'Jane');
console.log(index); // 1
```

### some() and every()
Test if any or all elements satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4, 5];
const hasEven = numbers.some(n => n % 2 === 0);
console.log(hasEven); // true

const allPositive = numbers.every(n => n > 0);
console.log(allPositive); // true
```

## Function Composition

Function composition is the process of combining simple functions to create more complex ones.

```javascript
// Simple composition
const add = x => x + 1;
const multiply = x => x * 2;
const subtract = x => x - 1;

// Manual composition
const composed = x => subtract(multiply(add(x)));

// Generic compose function
function compose(...fns) {
  return function(x) {
    return fns.reduceRight((acc, fn) => fn(acc), x);
  };
}

const composed2 = compose(subtract, multiply, add);
console.log(composed2(5)); // ((5 + 1) * 2) - 1 = 11

// Pipe function (left to right composition)
function pipe(...fns) {
  return function(x) {
    return fns.reduce((acc, fn) => fn(acc), x);
  };
}

const piped = pipe(add, multiply, subtract);
console.log(piped(5)); // ((5 + 1) * 2) - 1 = 11

// Real-world example
const users = [
  { name: 'John', age: 30, salary: 50000 },
  { name: 'Jane', age: 25, salary: 60000 },
  { name: 'Bob', age: 35, salary: 70000 }
];

const processUsers = pipe(
  users => users.filter(user => user.age >= 30),
  users => users.map(user => ({ ...user, salary: user.salary * 1.1 })),
  users => users.sort((a, b) => b.salary - a.salary)
);

const processedUsers = processUsers(users);
console.log(processedUsers);
```

## Currying

Currying is the technique of converting a function that takes multiple arguments into a sequence of functions that each take a single argument.

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

// Arrow function version
const curriedAddArrow = a => b => c => a + b + c;

console.log(curriedAdd(1)(2)(3)); // 6
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

// Practical example
const createValidator = curry((minLength, maxLength, value) => {
  return value.length >= minLength && value.length <= maxLength;
});

const validatePassword = createValidator(8, 20);
const validateUsername = createValidator(3, 15);

console.log(validatePassword('password123')); // true
console.log(validateUsername('john')); // true
```

## Closures in Functional Programming

Closures are essential for functional programming as they allow functions to capture and maintain access to variables from their lexical scope.

```javascript
// Counter using closure
function createCounter(initialValue = 0) {
  let count = initialValue;
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getValue: () => count,
    reset: () => count = initialValue
  };
}

const counter = createCounter(10);
console.log(counter.getValue()); // 10
console.log(counter.increment()); // 11
console.log(counter.increment()); // 12
console.log(counter.decrement()); // 11

// Memoization using closure
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.has(key)) {
      console.log('Cache hit');
      return cache.get(key);
    }
    
    console.log('Cache miss');
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const expensiveFunction = (n) => {
  console.log('Computing...');
  return n * n;
};

const memoizedFunction = memoize(expensiveFunction);
console.log(memoizedFunction(5)); // Computing... 25
console.log(memoizedFunction(5)); // Cache hit 25
console.log(memoizedFunction(6)); // Computing... 36
```

## Recursion

Recursion is a fundamental concept in functional programming where a function calls itself.

```javascript
// Basic recursion
function factorial(n) {
  if (n <= 1) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120

// Tail recursion (optimized)
function factorialTail(n, acc = 1) {
  if (n <= 1) {
    return acc;
  }
  return factorialTail(n - 1, n * acc);
}

console.log(factorialTail(5)); // 120

// Recursive array processing
function sumArray(arr) {
  if (arr.length === 0) {
    return 0;
  }
  return arr[0] + sumArray(arr.slice(1));
}

console.log(sumArray([1, 2, 3, 4, 5])); // 15

// Tree traversal
const tree = {
  value: 1,
  children: [
    {
      value: 2,
      children: [
        { value: 4, children: [] },
        { value: 5, children: [] }
      ]
    },
    {
      value: 3,
      children: [
        { value: 6, children: [] }
      ]
    }
  ]
};

function traverseTree(node, callback) {
  callback(node.value);
  node.children.forEach(child => traverseTree(child, callback));
}

const values = [];
traverseTree(tree, value => values.push(value));
console.log(values); // [1, 2, 4, 5, 3, 6]
```

## Practical Examples

### Example 1: Data Processing Pipeline

```javascript
class DataProcessor {
  constructor() {
    this.pipeline = [];
  }
  
  addStep(step) {
    this.pipeline.push(step);
    return this;
  }
  
  process(data) {
    return this.pipeline.reduce((acc, step) => step(acc), data);
  }
}

// Processing steps
const filterAdults = users => users.filter(user => user.age >= 18);
const addFullName = users => users.map(user => ({
  ...user,
  fullName: `${user.firstName} ${user.lastName}`
}));
const sortByAge = users => users.sort((a, b) => a.age - b.age);
const addId = users => users.map((user, index) => ({
  ...user,
  id: index + 1
}));

// Usage
const users = [
  { firstName: 'John', lastName: 'Doe', age: 30 },
  { firstName: 'Jane', lastName: 'Smith', age: 17 },
  { firstName: 'Bob', lastName: 'Johnson', age: 25 }
];

const processor = new DataProcessor()
  .addStep(filterAdults)
  .addStep(addFullName)
  .addStep(sortByAge)
  .addStep(addId);

const processedUsers = processor.process(users);
console.log(processedUsers);
```

### Example 2: Event System

```javascript
class EventSystem {
  constructor() {
    this.events = new Map();
  }
  
  on(event, handler) {
    if (!this.events.has(event)) {
      this.events.set(event, []);
    }
    this.events.get(event).push(handler);
    return this;
  }
  
  off(event, handler) {
    if (this.events.has(event)) {
      const handlers = this.events.get(event);
      const index = handlers.indexOf(handler);
      if (index > -1) {
        handlers.splice(index, 1);
      }
    }
    return this;
  }
  
  emit(event, data) {
    if (this.events.has(event)) {
      this.events.get(event).forEach(handler => handler(data));
    }
    return this;
  }
  
  once(event, handler) {
    const onceHandler = (data) => {
      handler(data);
      this.off(event, onceHandler);
    };
    this.on(event, onceHandler);
    return this;
  }
}

// Usage
const eventSystem = new EventSystem();

const logHandler = (data) => console.log('Log:', data);
const alertHandler = (data) => console.log('Alert:', data);

eventSystem
  .on('user:login', logHandler)
  .on('user:login', alertHandler)
  .once('user:login', (data) => console.log('First login:', data));

eventSystem.emit('user:login', { userId: 123, username: 'john' });
```

### Example 3: State Management

```javascript
class StateManager {
  constructor(initialState = {}) {
    this.state = initialState;
    this.listeners = [];
    this.middleware = [];
  }
  
  getState() {
    return { ...this.state };
  }
  
  setState(newState) {
    const prevState = this.state;
    this.state = { ...this.state, ...newState };
    this.notifyListeners(prevState, this.state);
  }
  
  subscribe(listener) {
    this.listeners.push(listener);
    return () => {
      const index = this.listeners.indexOf(listener);
      if (index > -1) {
        this.listeners.splice(index, 1);
      }
    };
  }
  
  addMiddleware(middleware) {
    this.middleware.push(middleware);
  }
  
  dispatch(action) {
    let newState = this.state;
    
    // Apply middleware
    for (const middleware of this.middleware) {
      newState = middleware(newState, action);
    }
    
    this.setState(newState);
  }
  
  notifyListeners(prevState, newState) {
    this.listeners.forEach(listener => listener(newState, prevState));
  }
}

// Usage
const store = new StateManager({ count: 0, user: null });

// Middleware
const loggerMiddleware = (state, action) => {
  console.log('Action:', action, 'State:', state);
  return state;
};

const counterReducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    case 'SET_USER':
      return { ...state, user: action.payload };
    default:
      return state;
  }
};

store.addMiddleware(loggerMiddleware);
store.addMiddleware(counterReducer);

// Subscribe to changes
const unsubscribe = store.subscribe((newState, prevState) => {
  console.log('State changed:', prevState, '->', newState);
});

// Dispatch actions
store.dispatch({ type: 'INCREMENT' });
store.dispatch({ type: 'SET_USER', payload: { name: 'John' } });
store.dispatch({ type: 'DECREMENT' });
```

### Example 4: Functional Utilities

```javascript
// Utility functions
const pipe = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value);
const compose = (...fns) => (value) => fns.reduceRight((acc, fn) => fn(acc), value);
const curry = (fn) => {
  const arity = fn.length;
  return function curried(...args) {
    if (args.length >= arity) {
      return fn.apply(this, args);
    } else {
      return (...nextArgs) => curried.apply(this, args.concat(nextArgs));
    }
  };
};

// Functional utilities
const map = curry((fn, arr) => arr.map(fn));
const filter = curry((fn, arr) => arr.filter(fn));
const reduce = curry((fn, initial, arr) => arr.reduce(fn, initial));
const sort = curry((fn, arr) => [...arr].sort(fn));

// Data transformation pipeline
const users = [
  { name: 'John', age: 30, salary: 50000, department: 'IT' },
  { name: 'Jane', age: 25, salary: 60000, department: 'HR' },
  { name: 'Bob', age: 35, salary: 70000, department: 'IT' },
  { name: 'Alice', age: 28, salary: 55000, department: 'Finance' }
];

const processUsers = pipe(
  filter(user => user.age >= 25),
  map(user => ({ ...user, salary: user.salary * 1.1 })),
  sort((a, b) => b.salary - a.salary),
  map(user => ({ name: user.name, salary: user.salary }))
);

const processedUsers = processUsers(users);
console.log(processedUsers);

// Partial application
const filterByAge = filter(user => user.age >= 30);
const mapSalary = map(user => ({ ...user, salary: user.salary * 1.1 }));

const processUsers2 = pipe(
  filterByAge,
  mapSalary
);

const processedUsers2 = processUsers2(users);
console.log(processedUsers2);
```

## Exercises

### Exercise 1: Functional Calculator
Create a calculator using only pure functions, currying, and function composition.

### Exercise 2: Immutable Data Structure
Implement an immutable list with methods like push, pop, insert, and remove.

### Exercise 3: Functional Router
Build a simple router using function composition and higher-order functions.

### Exercise 4: Recursive Tree Operations
Create functions to traverse, search, and transform tree structures recursively.

### Exercise 5: Functional State Machine
Implement a state machine using pure functions and immutability.

## Key Takeaways

- Pure functions are the foundation of functional programming
- Immutability prevents bugs and makes code more predictable
- Higher-order functions enable powerful abstractions
- Function composition allows building complex behavior from simple parts
- Currying enables partial application and function reuse
- Closures provide encapsulation and state management
- Recursion is natural for tree structures and mathematical problems
- Array methods (map, filter, reduce) are essential functional tools
- Avoid side effects and mutable state when possible
- Use functional programming to write more maintainable and testable code

Functional programming in JavaScript helps create more predictable, testable, and maintainable code by emphasizing pure functions and immutability.

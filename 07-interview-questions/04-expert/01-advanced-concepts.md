# JavaScript Interview Questions - Expert Level

## Advanced Concepts

### 1. What is the Event Loop and how does it work in detail?

**Answer:**
The Event Loop is JavaScript's mechanism for handling asynchronous operations. It continuously monitors the call stack and task queues to execute code.

**Event Loop Components:**
1. **Call Stack**: Where synchronous code is executed
2. **Web APIs**: Browser APIs (setTimeout, DOM events, etc.)
3. **Task Queue (Macrotask Queue)**: Lower priority queue
4. **Microtask Queue**: Higher priority queue
5. **Event Loop**: Continuously checks and executes tasks

**Execution Priority:**
```javascript
console.log('1'); // Synchronous - Call Stack

setTimeout(() => console.log('2'), 0); // Macrotask Queue

Promise.resolve().then(() => console.log('3')); // Microtask Queue

queueMicrotask(() => console.log('4')); // Microtask Queue

console.log('5'); // Synchronous - Call Stack

// Output: 1, 5, 3, 4, 2
// Explanation: Call stack → Microtasks → Macrotasks
```

**Detailed Event Loop Process:**
```javascript
// 1. Execute all synchronous code
console.log('Start');

// 2. Schedule microtasks
Promise.resolve().then(() => {
  console.log('Microtask 1');
  Promise.resolve().then(() => {
    console.log('Microtask 2');
  });
});

// 3. Schedule macrotasks
setTimeout(() => {
  console.log('Macrotask 1');
  setTimeout(() => {
    console.log('Macrotask 2');
  }, 0);
}, 0);

console.log('End');

// Output: Start, End, Microtask 1, Microtask 2, Macrotask 1, Macrotask 2
```

### 2. What are WeakMaps and WeakSets and when would you use them?

**Answer:**
WeakMap and WeakSet are collections that hold weak references to objects, allowing them to be garbage collected even if they're in the collection.

**WeakMap:**
```javascript
const weakMap = new WeakMap();

// Keys must be objects
const obj1 = { name: 'John' };
const obj2 = { name: 'Jane' };

weakMap.set(obj1, 'value1');
weakMap.set(obj2, 'value2');

console.log(weakMap.get(obj1)); // 'value1'

// WeakMap doesn't prevent garbage collection
obj1 = null; // obj1 can be garbage collected
// weakMap.get(obj1) would return undefined

// WeakMap is not iterable
// for (const [key, value] of weakMap) { } // Error

// Use cases: Private data, metadata, caching
const privateData = new WeakMap();

class User {
  constructor(name) {
    this.name = name;
    privateData.set(this, { password: 'secret' });
  }
  
  getPassword() {
    return privateData.get(this).password;
  }
}
```

**WeakSet:**
```javascript
const weakSet = new WeakSet();

const obj1 = { name: 'John' };
const obj2 = { name: 'Jane' };

weakSet.add(obj1);
weakSet.add(obj2);

console.log(weakSet.has(obj1)); // true

// WeakSet doesn't prevent garbage collection
obj1 = null; // obj1 can be garbage collected
// weakSet.has(obj1) would return false

// Use cases: Tracking objects, preventing memory leaks
const processedObjects = new WeakSet();

function processObject(obj) {
  if (processedObjects.has(obj)) {
    return; // Already processed
  }
  
  processedObjects.add(obj);
  // Process object...
}
```

### 3. What are Proxies and how do you use them?

**Answer:**
Proxies allow you to intercept and customize operations performed on objects (property lookup, assignment, enumeration, function invocation, etc.).

**Basic Proxy:**
```javascript
const target = {
  name: 'John',
  age: 30
};

const handler = {
  get(target, property) {
    console.log(`Getting ${property}`);
    return target[property];
  },
  
  set(target, property, value) {
    console.log(`Setting ${property} to ${value}`);
    target[property] = value;
    return true;
  }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // "Getting name" then "John"
proxy.age = 31; // "Setting age to 31"
```

**Advanced Proxy Examples:**
```javascript
// Validation Proxy
function createValidatedObject(schema) {
  return new Proxy({}, {
    set(target, property, value) {
      if (schema[property]) {
        const validator = schema[property];
        if (!validator(value)) {
          throw new Error(`Invalid value for ${property}`);
        }
      }
      target[property] = value;
      return true;
    }
  });
}

const schema = {
  name: (value) => typeof value === 'string',
  age: (value) => typeof value === 'number' && value > 0
};

const person = createValidatedObject(schema);
person.name = 'John'; // OK
person.age = 30; // OK
// person.age = -5; // Error: Invalid value for age
```

**Function Proxy:**
```javascript
function createLoggingProxy(fn) {
  return new Proxy(fn, {
    apply(target, thisArg, argumentsList) {
      console.log(`Calling ${target.name} with arguments:`, argumentsList);
      const result = target.apply(thisArg, argumentsList);
      console.log(`Result:`, result);
      return result;
    }
  });
}

function add(a, b) {
  return a + b;
}

const loggedAdd = createLoggingProxy(add);
loggedAdd(2, 3); // Logs the call and result
```

### 4. What are Symbols and how do you use them?

**Answer:**
Symbols are primitive data types that represent unique identifiers. They are immutable and unique.

**Basic Symbol Usage:**
```javascript
// Creating symbols
const sym1 = Symbol();
const sym2 = Symbol('description');
const sym3 = Symbol('description');

console.log(sym1 === sym2); // false
console.log(sym2 === sym3); // false (even with same description)

// Using symbols as object keys
const obj = {};
const sym = Symbol('id');
obj[sym] = 'value';
console.log(obj[sym]); // 'value'

// Symbols are not enumerable
for (const key in obj) {
  console.log(key); // Won't log the symbol key
}
```

**Well-known Symbols:**
```javascript
// Symbol.iterator - makes object iterable
const iterable = {
  [Symbol.iterator]() {
    let count = 0;
    return {
      next() {
        if (count < 3) {
          return { value: count++, done: false };
        }
        return { done: true };
      }
    };
  }
};

for (const value of iterable) {
  console.log(value); // 0, 1, 2
}

// Symbol.toPrimitive - custom type conversion
const obj = {
  [Symbol.toPrimitive](hint) {
    if (hint === 'number') {
      return 42;
    }
    if (hint === 'string') {
      return 'hello';
    }
    return 'default';
  }
};

console.log(+obj); // 42
console.log(`${obj}`); // 'hello'
console.log(obj + ''); // 'default'
```

**Symbol.for() and Symbol.keyFor():**
```javascript
// Symbol.for() - creates or retrieves global symbol
const sym1 = Symbol.for('global');
const sym2 = Symbol.for('global');
console.log(sym1 === sym2); // true

// Symbol.keyFor() - gets the key for a global symbol
console.log(Symbol.keyFor(sym1)); // 'global'
```

### 5. What are Generators and how do you use them?

**Answer:**
Generators are functions that can be paused and resumed, allowing you to control the execution flow and create iterators.

**Basic Generator:**
```javascript
function* simpleGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = simpleGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

**Generator with Parameters:**
```javascript
function* generatorWithParams() {
  const value1 = yield 'First yield';
  const value2 = yield 'Second yield';
  return `Final: ${value1} and ${value2}`;
}

const gen = generatorWithParams();
console.log(gen.next()); // { value: 'First yield', done: false }
console.log(gen.next('Hello')); // { value: 'Second yield', done: false }
console.log(gen.next('World')); // { value: 'Final: Hello and World', done: true }
```

**Generator for Iteration:**
```javascript
function* fibonacci() {
  let a = 0, b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fib = fibonacci();
for (let i = 0; i < 10; i++) {
  console.log(fib.next().value); // 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
}
```

**Generator with try-catch:**
```javascript
function* generatorWithError() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (error) {
    console.log('Caught error:', error);
    yield 'Error handled';
  }
}

const gen = generatorWithError();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.throw('Something went wrong')); // { value: 'Error handled', done: false }
```

### 6. What are Async Generators and how do you use them?

**Answer:**
Async generators are generators that can yield promises and be used with async/await syntax.

**Basic Async Generator:**
```javascript
async function* asyncGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

async function consumeAsyncGenerator() {
  for await (const value of asyncGenerator()) {
    console.log(value); // 1, 2, 3
  }
}

consumeAsyncGenerator();
```

**Async Generator with Promises:**
```javascript
async function* fetchUsers() {
  const userIds = [1, 2, 3, 4, 5];
  
  for (const id of userIds) {
    try {
      const response = await fetch(`/api/users/${id}`);
      const user = await response.json();
      yield user;
    } catch (error) {
      console.error(`Failed to fetch user ${id}:`, error);
    }
  }
}

async function processUsers() {
  for await (const user of fetchUsers()) {
    console.log('Processing user:', user.name);
    // Process each user as it arrives
  }
}
```

### 7. What are Decorators and how do you use them?

**Answer:**
Decorators are functions that modify the behavior of classes, methods, or properties. They are currently a stage 3 proposal.

**Basic Decorator:**
```javascript
// Decorator function
function log(target, propertyKey, descriptor) {
  const originalMethod = descriptor.value;
  
  descriptor.value = function(...args) {
    console.log(`Calling ${propertyKey} with arguments:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };
  
  return descriptor;
}

class Calculator {
  @log
  add(a, b) {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(2, 3); // Logs the call and result
```

**Property Decorator:**
```javascript
function readonly(target, propertyKey, descriptor) {
  descriptor.writable = false;
  return descriptor;
}

class Person {
  @readonly
  name = 'John';
}

const person = new Person();
// person.name = 'Jane'; // Error in strict mode
```

**Class Decorator:**
```javascript
function sealed(constructor) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class MyClass {
  method() {
    return 'hello';
  }
}
```

### 8. What are Template Literals and Tagged Template Literals?

**Answer:**
Template literals are string literals that allow embedded expressions and multi-line strings. Tagged template literals are function calls with template literals.

**Basic Template Literals:**
```javascript
const name = 'John';
const age = 30;

// Basic template literal
const message = `Hello, my name is ${name} and I am ${age} years old`;

// Multi-line strings
const multiLine = `
  This is a
  multi-line
  string
`;

// Expression evaluation
const calculation = `2 + 3 = ${2 + 3}`;
console.log(calculation); // "2 + 3 = 5"
```

**Tagged Template Literals:**
```javascript
function highlight(strings, ...values) {
  return strings.reduce((result, string, i) => {
    const value = values[i] ? `<mark>${values[i]}</mark>` : '';
    return result + string + value;
  }, '');
}

const name = 'John';
const age = 30;
const html = highlight`Hello, my name is ${name} and I am ${age} years old`;
console.log(html); // "Hello, my name is <mark>John</mark> and I am <mark>30</mark> years old"
```

**Advanced Tagged Template Literal:**
```javascript
function sql(strings, ...values) {
  return {
    query: strings.join('?'),
    params: values
  };
}

const name = 'John';
const age = 30;
const { query, params } = sql`SELECT * FROM users WHERE name = ${name} AND age = ${age}`;
console.log(query); // "SELECT * FROM users WHERE name = ? AND age = ?"
console.log(params); // ["John", 30]
```

### 9. What are Private Fields and Methods in Classes?

**Answer:**
Private fields and methods are class members that can only be accessed from within the class itself.

**Private Fields:**
```javascript
class BankAccount {
  #balance = 0;
  #accountNumber;
  
  constructor(accountNumber, initialBalance = 0) {
    this.#accountNumber = accountNumber;
    this.#balance = initialBalance;
  }
  
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return this.#balance;
    }
    throw new Error('Invalid amount');
  }
  
  withdraw(amount) {
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return this.#balance;
    }
    throw new Error('Invalid amount or insufficient funds');
  }
  
  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount('123456', 1000);
console.log(account.getBalance()); // 1000
// console.log(account.#balance); // SyntaxError: Private field '#balance' must be declared in an enclosing class
```

**Private Methods:**
```javascript
class User {
  #password;
  
  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }
  
  #validatePassword(password) {
    return password.length >= 8;
  }
  
  changePassword(oldPassword, newPassword) {
    if (oldPassword !== this.#password) {
      throw new Error('Invalid old password');
    }
    
    if (!this.#validatePassword(newPassword)) {
      throw new Error('Password must be at least 8 characters');
    }
    
    this.#password = newPassword;
  }
}

const user = new User('John', 'oldpassword');
user.changePassword('oldpassword', 'newpassword123');
// user.#validatePassword('test'); // SyntaxError: Private method '#validatePassword' must be declared in an enclosing class
```

### 10. What are the different ways to implement memoization?

**Answer:**
Memoization is a technique to cache the results of expensive function calls.

**Basic Memoization:**
```javascript
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Usage
const expensiveFunction = memoize(function(n) {
  console.log('Computing...');
  return n * n;
});

console.log(expensiveFunction(5)); // Computing... 25
console.log(expensiveFunction(5)); // 25 (from cache)
```

**Advanced Memoization with WeakMap:**
```javascript
function memoizeWithWeakMap(fn) {
  const cache = new WeakMap();
  
  return function(obj, ...args) {
    if (!cache.has(obj)) {
      cache.set(obj, new Map());
    }
    
    const objCache = cache.get(obj);
    const key = JSON.stringify(args);
    
    if (objCache.has(key)) {
      return objCache.get(key);
    }
    
    const result = fn.call(obj, ...args);
    objCache.set(key, result);
    return result;
  };
}

// Usage
const obj = { name: 'John' };
const memoizedMethod = memoizeWithWeakMap(function(prop) {
  console.log('Computing...');
  return this.name + prop;
});

console.log(memoizedMethod.call(obj, ' Doe')); // Computing... John Doe
console.log(memoizedMethod.call(obj, ' Doe')); // John Doe (from cache)
```

**Memoization with LRU Cache:**
```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (this.cache.has(key)) {
      const value = this.cache.get(key);
      this.cache.delete(key);
      this.cache.set(key, value);
      return value;
    }
    return undefined;
  }
  
  set(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    } else if (this.cache.size >= this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    this.cache.set(key, value);
  }
}

function memoizeWithLRU(fn, capacity = 100) {
  const cache = new LRUCache(capacity);
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.get(key) !== undefined) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}
```

## Practice Questions

1. **What will be the output?**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
queueMicrotask(() => console.log('4'));
console.log('5');
```

2. **Explain the output:**
```javascript
const weakMap = new WeakMap();
const obj = { name: 'John' };
weakMap.set(obj, 'value');
obj = null;
console.log(weakMap.get(obj));
```

3. **What's the difference between these two?**
```javascript
// Version 1
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

// Version 2
async function* asyncGenerator() {
  yield 1;
  yield 2;
  yield 3;
}
```

4. **Predict the output:**
```javascript
const proxy = new Proxy({}, {
  get(target, property) {
    return `Getting ${property}`;
  }
});
console.log(proxy.anything);
```

5. **What will this return?**
```javascript
const sym = Symbol('test');
const obj = { [sym]: 'value' };
console.log(Object.keys(obj));
console.log(Object.getOwnPropertySymbols(obj));
```

## Key Takeaways

- Master the Event Loop and execution order
- Understand WeakMaps and WeakSets for memory management
- Learn Proxies for advanced object manipulation
- Master Symbols for unique identifiers and well-known symbols
- Understand Generators for control flow
- Learn Async Generators for async iteration
- Understand Decorators for class modification
- Master Template Literals and Tagged Templates
- Learn Private Fields and Methods for encapsulation
- Practice different memoization techniques

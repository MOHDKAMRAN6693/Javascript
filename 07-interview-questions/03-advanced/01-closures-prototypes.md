# JavaScript Interview Questions - Advanced Level

## Closures & Prototypes

### 1. What are closures and how do they work?

**Answer:**
A closure is a function that has access to variables in its outer (enclosing) scope even after the outer function has returned. Closures are created every time a function is created.

**Basic Closure Example:**
```javascript
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
```

**Closure with Private Variables:**
```javascript
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

**Closure in Loops (Common Pitfall):**
```javascript
// Problem: All functions reference the same variable
function createFunctions() {
  const result = [];
  for (var i = 0; i < 3; i++) {
    result[i] = function() {
      return i; // All functions return 3
    };
  }
  return result;
}

const functions = createFunctions();
console.log(functions[0]()); // 3
console.log(functions[1]()); // 3
console.log(functions[2]()); // 3

// Solution 1: Use let instead of var
function createFunctionsFixed1() {
  const result = [];
  for (let i = 0; i < 3; i++) {
    result[i] = function() {
      return i; // Each function has its own i
    };
  }
  return result;
}

// Solution 2: Use IIFE (Immediately Invoked Function Expression)
function createFunctionsFixed2() {
  const result = [];
  for (var i = 0; i < 3; i++) {
    result[i] = (function(index) {
      return function() {
        return index;
      };
    })(i);
  }
  return result;
}
```

### 2. What is the prototype chain in JavaScript?

**Answer:**
The prototype chain is JavaScript's mechanism for inheritance. Every object has a prototype, and when a property or method is accessed, JavaScript looks up the chain until it finds the property or reaches the end.

**Basic Prototype Chain:**
```javascript
// Constructor function
function Person(name) {
  this.name = name;
}

// Adding method to prototype
Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

// Creating instance
const person = new Person('John');
console.log(person.greet()); // "Hello, I'm John"

// Prototype chain lookup
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('greet')); // false (inherited from prototype)
console.log(person.__proto__.hasOwnProperty('greet')); // true
```

**Prototype Chain Visualization:**
```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return `${this.name} barks`;
};

const dog = new Dog('Buddy', 'Golden Retriever');

// Prototype chain: dog -> Dog.prototype -> Animal.prototype -> Object.prototype -> null
console.log(dog.bark()); // "Buddy barks"
console.log(dog.speak()); // "Buddy makes a sound"
console.log(dog.toString()); // "[object Object]" (from Object.prototype)
```

### 3. What is the difference between `__proto__` and `prototype`?

**Answer:**
- **`prototype`**: Property of constructor functions, used to set up inheritance
- **`__proto__`**: Property of instances, points to the prototype of the constructor

**Examples:**
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const person = new Person('John');

// prototype is on the constructor function
console.log(Person.prototype); // { greet: function, constructor: Person }
console.log(Person.prototype.greet); // function

// __proto__ is on the instance
console.log(person.__proto__); // { greet: function, constructor: Person }
console.log(person.__proto__ === Person.prototype); // true

// Modern way to access prototype
console.log(Object.getPrototypeOf(person) === Person.prototype); // true
```

### 4. How do you implement inheritance in JavaScript?

**Answer:**
There are several ways to implement inheritance in JavaScript:

**1. Constructor Function Inheritance:**
```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

function Dog(name, breed) {
  Animal.call(this, name); // Call parent constructor
  this.breed = breed;
}

// Set up prototype chain
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return `${this.name} barks`;
};

const dog = new Dog('Buddy', 'Golden Retriever');
console.log(dog.speak()); // "Buddy makes a sound"
console.log(dog.bark()); // "Buddy barks"
```

**2. ES6 Class Inheritance:**
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }
  
  bark() {
    return `${this.name} barks`;
  }
}

const dog = new Dog('Buddy', 'Golden Retriever');
console.log(dog.speak()); // "Buddy makes a sound"
console.log(dog.bark()); // "Buddy barks"
```

**3. Object.create() Inheritance:**
```javascript
const animal = {
  speak() {
    return `${this.name} makes a sound`;
  }
};

const dog = Object.create(animal);
dog.name = 'Buddy';
dog.bark = function() {
  return `${this.name} barks`;
};

console.log(dog.speak()); // "Buddy makes a sound"
console.log(dog.bark()); // "Buddy barks"
```

### 5. What is the difference between classical and prototypal inheritance?

**Answer:**
- **Classical Inheritance**: Classes are blueprints, objects are instances of classes
- **Prototypal Inheritance**: Objects inherit directly from other objects

**Classical Inheritance (ES6 Classes):**
```javascript
class Vehicle {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
  
  start() {
    return `${this.make} ${this.model} is starting`;
  }
}

class Car extends Vehicle {
  constructor(make, model, doors) {
    super(make, model);
    this.doors = doors;
  }
  
  honk() {
    return `${this.make} ${this.model} honks`;
  }
}

const car = new Car('Toyota', 'Camry', 4);
console.log(car.start()); // "Toyota Camry is starting"
console.log(car.honk()); // "Toyota Camry honks"
```

**Prototypal Inheritance:**
```javascript
const vehicle = {
  start() {
    return `${this.make} ${this.model} is starting`;
  }
};

const car = Object.create(vehicle);
car.make = 'Toyota';
car.model = 'Camry';
car.doors = 4;
car.honk = function() {
  return `${this.make} ${this.model} honks`;
};

console.log(car.start()); // "Toyota Camry is starting"
console.log(car.honk()); // "Toyota Camry honks"
```

### 6. What are mixins and how do you use them?

**Answer:**
Mixins are a way to add functionality to objects without inheritance. They allow you to compose objects with multiple behaviors.

**Basic Mixin:**
```javascript
const canFly = {
  fly() {
    return `${this.name} is flying`;
  }
};

const canSwim = {
  swim() {
    return `${this.name} is swimming`;
  }
};

const canWalk = {
  walk() {
    return `${this.name} is walking`;
  }
};

// Mixin function
function mixin(target, ...sources) {
  sources.forEach(source => {
    Object.getOwnPropertyNames(source).forEach(name => {
      if (name !== 'constructor') {
        target[name] = source[name];
      }
    });
  });
  return target;
}

// Usage
function Duck(name) {
  this.name = name;
}

mixin(Duck.prototype, canFly, canSwim, canWalk);

const duck = new Duck('Donald');
console.log(duck.fly()); // "Donald is flying"
console.log(duck.swim()); // "Donald is swimming"
console.log(duck.walk()); // "Donald is walking"
```

**ES6 Mixin with Classes:**
```javascript
const Flyable = {
  fly() {
    return `${this.name} is flying`;
  }
};

const Swimmable = {
  swim() {
    return `${this.name} is swimming`;
  }
};

class Duck {
  constructor(name) {
    this.name = name;
  }
}

// Apply mixins
Object.assign(Duck.prototype, Flyable, Swimmable);

const duck = new Duck('Donald');
console.log(duck.fly()); // "Donald is flying"
console.log(duck.swim()); // "Donald is swimming"
```

### 7. What is the difference between `Object.create()` and `new`?

**Answer:**
Both create objects, but they work differently:

- **`new`**: Creates an instance of a constructor function, calls the constructor
- **`Object.create()`**: Creates an object with a specified prototype, doesn't call any constructor

**Examples:**
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

// Using 'new'
const person1 = new Person('John');
console.log(person1.name); // 'John'
console.log(person1.greet()); // "Hello, I'm John"

// Using Object.create()
const person2 = Object.create(Person.prototype);
person2.name = 'Jane';
console.log(person2.name); // 'Jane'
console.log(person2.greet()); // "Hello, I'm Jane"

// Object.create() with properties
const person3 = Object.create(Person.prototype, {
  name: {
    value: 'Bob',
    writable: true,
    enumerable: true,
    configurable: true
  }
});

console.log(person3.name); // 'Bob'
console.log(person3.greet()); // "Hello, I'm Bob"
```

### 8. What are getters and setters in prototypes?

**Answer:**
Getters and setters allow you to define how properties are accessed and modified on objects and their prototypes.

**Basic Getters and Setters:**
```javascript
function Person(name) {
  this._name = name;
}

Person.prototype = {
  get name() {
    return this._name;
  },
  
  set name(value) {
    if (typeof value !== 'string') {
      throw new Error('Name must be a string');
    }
    this._name = value;
  },
  
  get fullName() {
    return this._name.toUpperCase();
  }
};

const person = new Person('John');
console.log(person.name); // 'John'
console.log(person.fullName); // 'JOHN'

person.name = 'Jane';
console.log(person.name); // 'Jane'
// person.name = 123; // Error: Name must be a string
```

**Computed Properties:**
```javascript
function Circle(radius) {
  this._radius = radius;
}

Circle.prototype = {
  get radius() {
    return this._radius;
  },
  
  set radius(value) {
    if (value < 0) {
      throw new Error('Radius cannot be negative');
    }
    this._radius = value;
  },
  
  get area() {
    return Math.PI * this._radius * this._radius;
  },
  
  get circumference() {
    return 2 * Math.PI * this._radius;
  }
};

const circle = new Circle(5);
console.log(circle.area); // 78.54...
circle.radius = 10;
console.log(circle.area); // 314.16...
```

### 9. What is the difference between `hasOwnProperty()` and `in` operator?

**Answer:**
- **`hasOwnProperty()`**: Checks if the object has the property as its own property (not inherited)
- **`in` operator**: Checks if the property exists anywhere in the prototype chain

**Examples:**
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.speak = function() {
  return `${this.name} speaks`;
};

const person = new Person('John');

// hasOwnProperty() - only own properties
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('speak')); // false (inherited)

// 'in' operator - anywhere in prototype chain
console.log('name' in person); // true
console.log('speak' in person); // true
console.log('toString' in person); // true (from Object.prototype)

// Practical usage
function logOwnProperties(obj) {
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      console.log(`Own property: ${key}`);
    } else {
      console.log(`Inherited property: ${key}`);
    }
  }
}

logOwnProperties(person);
// Own property: name
// Inherited property: speak
// Inherited property: constructor
// Inherited property: toString
// Inherited property: valueOf
// ... and more
```

### 10. What are the different ways to create objects with prototypes?

**Answer:**
There are several ways to create objects with prototypes:

**1. Constructor Function:**
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const person = new Person('John');
```

**2. Object.create():**
```javascript
const personPrototype = {
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};

const person = Object.create(personPrototype);
person.name = 'John';
```

**3. Object.create() with Properties:**
```javascript
const person = Object.create(personPrototype, {
  name: {
    value: 'John',
    writable: true,
    enumerable: true,
    configurable: true
  }
});
```

**4. Factory Function with Object.create():**
```javascript
function createPerson(name) {
  const person = Object.create(personPrototype);
  person.name = name;
  return person;
}

const person = createPerson('John');
```

**5. ES6 Classes:**
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  
  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

const person = new Person('John');
```

## Practice Questions

1. **What will be the output?**
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

2. **Explain the output:**
```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.speak = function() {
  return `${this.name} speaks`;
};

const person = new Person('John');
console.log(person.hasOwnProperty('speak'));
console.log('speak' in person);
```

3. **What's the difference between these two?**
```javascript
// Version 1
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

// Version 2
const personPrototype = {
  greet() {
    return `Hello, ${this.name}`;
  }
};
function Person(name) {
  this.name = name;
}
Person.prototype = personPrototype;
```

4. **Predict the output:**
```javascript
function createFunctions() {
  const result = [];
  for (var i = 0; i < 3; i++) {
    result[i] = function() {
      return i;
    };
  }
  return result;
}
const functions = createFunctions();
console.log(functions[0]());
```

5. **What will this return?**
```javascript
const obj = Object.create(null);
console.log(obj.toString);
console.log(obj.__proto__);
```

## Key Takeaways

- Closures provide data privacy and function factories
- Prototypes are the foundation of JavaScript inheritance
- Understand the difference between `__proto__` and `prototype`
- Master different inheritance patterns
- Know when to use mixins vs inheritance
- Understand the difference between `hasOwnProperty()` and `in`
- Practice with different object creation patterns
- Understand the prototype chain lookup process

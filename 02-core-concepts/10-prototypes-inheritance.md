# 10 - Prototypes and Inheritance

## What are Prototypes?

In JavaScript, every object has a prototype. A prototype is an object from which other objects inherit properties and methods. This is JavaScript's way of implementing inheritance.

## Understanding the Prototype Chain

### Basic Prototype
```javascript
let person = {
    name: "John",
    age: 25,
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

// Accessing prototype
console.log(person.__proto__); // Object.prototype
console.log(Object.getPrototypeOf(person)); // Object.prototype (recommended way)
```

### Prototype Chain
```javascript
let animal = {
    type: "mammal",
    breathe: function() {
        console.log("Breathing...");
    }
};

let dog = {
    name: "Buddy",
    breed: "Golden Retriever"
};

// Set up inheritance
Object.setPrototypeOf(dog, animal);

console.log(dog.name); // "Buddy" (own property)
console.log(dog.type); // "mammal" (inherited from animal)
dog.breathe(); // "Breathing..." (inherited method)
```

## Creating Objects with Prototypes

### Using Object.create()
```javascript
let animal = {
    type: "mammal",
    breathe: function() {
        console.log("Breathing...");
    }
};

let dog = Object.create(animal);
dog.name = "Buddy";
dog.breed = "Golden Retriever";

console.log(dog.name); // "Buddy"
console.log(dog.type); // "mammal"
dog.breathe(); // "Breathing..."
```

### Using Constructor Functions
```javascript
function Animal(type) {
    this.type = type;
}

Animal.prototype.breathe = function() {
    console.log("Breathing...");
};

function Dog(name, breed) {
    Animal.call(this, "mammal"); // Call parent constructor
    this.name = name;
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    console.log("Woof!");
};

let buddy = new Dog("Buddy", "Golden Retriever");
console.log(buddy.name); // "Buddy"
console.log(buddy.type); // "mammal"
buddy.breathe(); // "Breathing..."
buddy.bark(); // "Woof!"
```

## ES6 Classes and Inheritance

### Class Declaration
```javascript
class Animal {
    constructor(type) {
        this.type = type;
    }
    
    breathe() {
        console.log("Breathing...");
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super("mammal"); // Call parent constructor
        this.name = name;
        this.breed = breed;
    }
    
    bark() {
        console.log("Woof!");
    }
}

let buddy = new Dog("Buddy", "Golden Retriever");
console.log(buddy.name); // "Buddy"
console.log(buddy.type); // "mammal"
buddy.breathe(); // "Breathing..."
buddy.bark(); // "Woof!"
```

### Static Methods
```javascript
class MathUtils {
    static add(a, b) {
        return a + b;
    }
    
    static multiply(a, b) {
        return a * b;
    }
}

console.log(MathUtils.add(5, 3)); // 8
console.log(MathUtils.multiply(4, 6)); // 24
```

### Getters and Setters
```javascript
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
    
    set fullName(name) {
        const parts = name.split(" ");
        this.firstName = parts[0];
        this.lastName = parts[1];
    }
}

let person = new Person("John", "Doe");
console.log(person.fullName); // "John Doe"

person.fullName = "Jane Smith";
console.log(person.firstName); // "Jane"
console.log(person.lastName); // "Smith"
```

## Advanced Prototype Concepts

### Method Overriding
```javascript
class Animal {
    makeSound() {
        console.log("Some generic sound");
    }
}

class Dog extends Animal {
    makeSound() {
        console.log("Woof!");
    }
}

class Cat extends Animal {
    makeSound() {
        console.log("Meow!");
    }
}

let dog = new Dog();
let cat = new Cat();

dog.makeSound(); // "Woof!"
cat.makeSound(); // "Meow!"
```

### Super Keyword
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Call parent constructor
        this.breed = breed;
    }
    
    speak() {
        super.speak(); // Call parent method
        console.log(`${this.name} barks`);
    }
}

let dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // "Buddy makes a sound" then "Buddy barks"
```

### Private Fields (ES2022)
```javascript
class BankAccount {
    #balance = 0; // Private field
    #accountNumber; // Private field
    
    constructor(accountNumber) {
        this.#accountNumber = accountNumber;
    }
    
    deposit(amount) {
        this.#balance += amount;
    }
    
    withdraw(amount) {
        if (amount <= this.#balance) {
            this.#balance -= amount;
        }
    }
    
    getBalance() {
        return this.#balance;
    }
}

let account = new BankAccount("12345");
account.deposit(1000);
console.log(account.getBalance()); // 1000
// console.log(account.#balance); // Error! Private field
```

## Mixins and Composition

### Mixin Pattern
```javascript
let flyMixin = {
    fly: function() {
        console.log("Flying...");
    }
};

let swimMixin = {
    swim: function() {
        console.log("Swimming...");
    }
};

class Duck {
    constructor(name) {
        this.name = name;
    }
    
    quack() {
        console.log("Quack!");
    }
}

// Apply mixins
Object.assign(Duck.prototype, flyMixin, swimMixin);

let duck = new Duck("Donald");
duck.quack(); // "Quack!"
duck.fly(); // "Flying..."
duck.swim(); // "Swimming..."
```

### Composition over Inheritance
```javascript
class Engine {
    start() {
        console.log("Engine started");
    }
    
    stop() {
        console.log("Engine stopped");
    }
}

class Wheels {
    constructor(count) {
        this.count = count;
    }
    
    rotate() {
        console.log(`${this.count} wheels rotating`);
    }
}

class Car {
    constructor(brand, model) {
        this.brand = brand;
        this.model = model;
        this.engine = new Engine();
        this.wheels = new Wheels(4);
    }
    
    start() {
        this.engine.start();
        this.wheels.rotate();
        console.log(`${this.brand} ${this.model} is ready to go!`);
    }
}

let car = new Car("Toyota", "Camry");
car.start();
```

## Practical Examples

### Example 1: Shape Hierarchy
```javascript
class Shape {
    constructor(color) {
        this.color = color;
    }
    
    getArea() {
        throw new Error("getArea() must be implemented by subclass");
    }
    
    getPerimeter() {
        throw new Error("getPerimeter() must be implemented by subclass");
    }
    
    describe() {
        console.log(`This is a ${this.color} shape`);
    }
}

class Circle extends Shape {
    constructor(color, radius) {
        super(color);
        this.radius = radius;
    }
    
    getArea() {
        return Math.PI * this.radius * this.radius;
    }
    
    getPerimeter() {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(color, width, height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    getArea() {
        return this.width * this.height;
    }
    
    getPerimeter() {
        return 2 * (this.width + this.height);
    }
}

let circle = new Circle("red", 5);
let rectangle = new Rectangle("blue", 4, 6);

circle.describe(); // "This is a red shape"
console.log(circle.getArea()); // 78.54
console.log(rectangle.getArea()); // 24
```

### Example 2: Employee Management System
```javascript
class Employee {
    constructor(name, id, department) {
        this.name = name;
        this.id = id;
        this.department = department;
    }
    
    work() {
        console.log(`${this.name} is working`);
    }
    
    getInfo() {
        return `Name: ${this.name}, ID: ${this.id}, Department: ${this.department}`;
    }
}

class Manager extends Employee {
    constructor(name, id, department, teamSize) {
        super(name, id, department);
        this.teamSize = teamSize;
    }
    
    manage() {
        console.log(`${this.name} is managing a team of ${this.teamSize}`);
    }
    
    getInfo() {
        return `${super.getInfo()}, Team Size: ${this.teamSize}`;
    }
}

class Developer extends Employee {
    constructor(name, id, department, programmingLanguage) {
        super(name, id, department);
        this.programmingLanguage = programmingLanguage;
    }
    
    code() {
        console.log(`${this.name} is coding in ${this.programmingLanguage}`);
    }
    
    getInfo() {
        return `${super.getInfo()}, Language: ${this.programmingLanguage}`;
    }
}

let manager = new Manager("Alice", 1, "Engineering", 5);
let developer = new Developer("Bob", 2, "Engineering", "JavaScript");

manager.work(); // "Alice is working"
manager.manage(); // "Alice is managing a team of 5"
developer.work(); // "Bob is working"
developer.code(); // "Bob is coding in JavaScript"
```

## Practice Exercises

### Exercise 1: Vehicle Hierarchy
Create a vehicle hierarchy with Car, Motorcycle, and Truck classes:
```javascript
class Vehicle {
    constructor(brand, model, year) {
        // Implementation
    }
    
    start() {
        // Implementation
    }
    
    stop() {
        // Implementation
    }
}

class Car extends Vehicle {
    constructor(brand, model, year, doors) {
        // Implementation
    }
    
    openTrunk() {
        // Implementation
    }
}
```

### Exercise 2: Bank Account System
Create a bank account system with different account types:
```javascript
class BankAccount {
    constructor(accountNumber, balance) {
        // Implementation
    }
    
    deposit(amount) {
        // Implementation
    }
    
    withdraw(amount) {
        // Implementation
    }
}

class SavingsAccount extends BankAccount {
    constructor(accountNumber, balance, interestRate) {
        // Implementation
    }
    
    addInterest() {
        // Implementation
    }
}
```

### Exercise 3: Animal Kingdom
Create an animal kingdom with different animal types:
```javascript
class Animal {
    constructor(name, species) {
        // Implementation
    }
    
    eat() {
        // Implementation
    }
    
    sleep() {
        // Implementation
    }
}

class Bird extends Animal {
    constructor(name, species, canFly) {
        // Implementation
    }
    
    fly() {
        // Implementation
    }
}
```

### Exercise 4: Mixin Implementation
Create mixins for different abilities:
```javascript
let flyable = {
    fly: function() {
        // Implementation
    }
};

let swimmable = {
    swim: function() {
        // Implementation
    }
};

class Duck {
    constructor(name) {
        // Implementation
    }
}
```

### Exercise 5: Composition Example
Create a computer system using composition:
```javascript
class CPU {
    constructor(brand, cores) {
        // Implementation
    }
}

class Memory {
    constructor(size) {
        // Implementation
    }
}

class Computer {
    constructor(brand, model) {
        // Implementation
    }
}
```

## Common Mistakes

### 1. Forgetting to Call Super
```javascript
// Wrong
class Child extends Parent {
    constructor(name) {
        this.name = name; // Error! Must call super() first
    }
}

// Correct
class Child extends Parent {
    constructor(name) {
        super();
        this.name = name;
    }
}
```

### 2. Not Setting Constructor
```javascript
// Wrong
Child.prototype = Object.create(Parent.prototype);
// Child.prototype.constructor is now Parent

// Correct
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```

### 3. Using Arrow Functions in Classes
```javascript
// Wrong - arrow functions don't have their own this
class MyClass {
    myMethod = () => {
        console.log(this); // Always refers to the instance
    }
}

// Correct - use regular methods
class MyClass {
    myMethod() {
        console.log(this); // Refers to the instance when called
    }
}
```

## Key Takeaways

- Prototypes are the foundation of JavaScript inheritance
- Every object has a prototype that it inherits from
- Use `Object.create()` to create objects with specific prototypes
- ES6 classes provide a cleaner syntax for inheritance
- Use `super()` to call parent constructors and methods
- Composition is often better than inheritance
- Mixins allow for flexible code reuse
- Practice with different inheritance patterns

## Next Steps

In the next lesson, we'll learn about:
- DOM events and event handling
- Event types and event objects
- Event bubbling and capturing
- Event delegation
- Custom events

---

**Ready to handle user interactions? Let's learn about Events! 🚀**

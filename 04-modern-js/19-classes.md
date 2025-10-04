# 19 - Classes

Classes in JavaScript provide a cleaner syntax for creating objects and handling inheritance. They were introduced in ES6 and are syntactic sugar over JavaScript's existing prototype-based inheritance.

## 📚 Table of Contents
- [Class Declaration](#class-declaration)
- [Constructor](#constructor)
- [Methods](#methods)
- [Static Methods](#static-methods)
- [Getters and Setters](#getters-and-setters)
- [Inheritance](#inheritance)
- [Private Fields](#private-fields)
- [Class Expressions](#class-expressions)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## Class Declaration

Classes are declared using the `class` keyword followed by the class name.

```javascript
class Person {
  // Class body
}
```

## Constructor

The `constructor` method is a special method for creating and initializing objects.

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

// Creating an instance
const person1 = new Person("Alice", 30);
console.log(person1.name); // "Alice"
console.log(person1.age);  // 30
```

## Methods

Methods are functions defined inside a class.

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    return `Hello, I'm ${this.name}`;
  }
  
  getAge() {
    return this.age;
  }
  
  haveBirthday() {
    this.age++;
    return `Happy birthday! I'm now ${this.age}`;
  }
}

const person = new Person("Bob", 25);
console.log(person.greet());        // "Hello, I'm Bob"
console.log(person.getAge());       // 25
console.log(person.haveBirthday()); // "Happy birthday! I'm now 26"
```

## Static Methods

Static methods belong to the class itself, not instances.

```javascript
class MathUtils {
  static add(a, b) {
    return a + b;
  }
  
  static multiply(a, b) {
    return a * b;
  }
  
  static PI = 3.14159;
}

// Call static methods on the class
console.log(MathUtils.add(5, 3));        // 8
console.log(MathUtils.multiply(4, 6));   // 24
console.log(MathUtils.PI);               // 3.14159

// Cannot call on instances
const math = new MathUtils();
// math.add(1, 2); // Error: math.add is not a function
```

## Getters and Setters

Getters and setters allow you to define methods that are accessed like properties.

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius;
  }
  
  get radius() {
    return this._radius;
  }
  
  set radius(value) {
    if (value < 0) {
      throw new Error("Radius cannot be negative");
    }
    this._radius = value;
  }
  
  get area() {
    return Math.PI * this._radius * this._radius;
  }
  
  get circumference() {
    return 2 * Math.PI * this._radius;
  }
}

const circle = new Circle(5);
console.log(circle.radius);        // 5
console.log(circle.area);          // 78.54...
console.log(circle.circumference); // 31.42...

circle.radius = 10;
console.log(circle.area);          // 314.16...

// circle.radius = -5; // Error: Radius cannot be negative
```

## Inheritance

Classes can extend other classes using the `extends` keyword.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a sound`;
  }
  
  eat() {
    return `${this.name} is eating`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks`;
  }
  
  fetch() {
    return `${this.name} fetches the ball`;
  }
}

class Cat extends Animal {
  constructor(name, color) {
    super(name);
    this.color = color;
  }
  
  speak() {
    return `${this.name} meows`;
  }
  
  purr() {
    return `${this.name} purrs softly`;
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
const cat = new Cat("Whiskers", "Orange");

console.log(dog.speak());  // "Buddy barks"
console.log(dog.eat());    // "Buddy is eating"
console.log(dog.fetch());  // "Buddy fetches the ball"

console.log(cat.speak());  // "Whiskers meows"
console.log(cat.eat());    // "Whiskers is eating"
console.log(cat.purr());   // "Whiskers purrs softly"
```

## Private Fields

Private fields are declared with a `#` prefix and can only be accessed within the class.

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
      return `Deposited $${amount}. New balance: $${this.#balance}`;
    }
    return "Invalid deposit amount";
  }
  
  withdraw(amount) {
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return `Withdrew $${amount}. New balance: $${this.#balance}`;
    }
    return "Insufficient funds or invalid amount";
  }
  
  getBalance() {
    return this.#balance;
  }
  
  getAccountNumber() {
    return this.#accountNumber;
  }
}

const account = new BankAccount("123456", 1000);
console.log(account.deposit(500));   // "Deposited $500. New balance: $1500"
console.log(account.withdraw(200));  // "Withdrew $200. New balance: $1300"
console.log(account.getBalance());  // 1300

// These will cause errors:
// console.log(account.#balance);        // SyntaxError
// console.log(account.#accountNumber);  // SyntaxError
```

## Class Expressions

Classes can be defined using expressions, similar to function expressions.

```javascript
// Named class expression
const Rectangle = class RectangleClass {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  getArea() {
    return this.width * this.height;
  }
};

// Anonymous class expression
const Square = class {
  constructor(side) {
    this.side = side;
  }
  
  getArea() {
    return this.side * this.side;
  }
};

const rect = new Rectangle(5, 10);
const square = new Square(4);

console.log(rect.getArea());  // 50
console.log(square.getArea()); // 16
```

## Practical Examples

### Example 1: Library Management System

```javascript
class Book {
  constructor(title, author, isbn, copies = 1) {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
    this.copies = copies;
    this.availableCopies = copies;
  }
  
  borrow() {
    if (this.availableCopies > 0) {
      this.availableCopies--;
      return `Successfully borrowed "${this.title}"`;
    }
    return `"${this.title}" is not available`;
  }
  
  return() {
    if (this.availableCopies < this.copies) {
      this.availableCopies++;
      return `Successfully returned "${this.title}"`;
    }
    return "All copies are already returned";
  }
  
  getAvailability() {
    return `${this.availableCopies} of ${this.copies} copies available`;
  }
}

class Library {
  constructor(name) {
    this.name = name;
    this.books = [];
  }
  
  addBook(book) {
    this.books.push(book);
    return `Added "${book.title}" to library`;
  }
  
  findBook(title) {
    return this.books.find(book => 
      book.title.toLowerCase().includes(title.toLowerCase())
    );
  }
  
  listAvailableBooks() {
    return this.books
      .filter(book => book.availableCopies > 0)
      .map(book => `${book.title} by ${book.author}`);
  }
}

// Usage
const library = new Library("City Library");

const book1 = new Book("JavaScript Guide", "John Doe", "123456", 3);
const book2 = new Book("Python Basics", "Jane Smith", "789012", 2);

library.addBook(book1);
library.addBook(book2);

console.log(book1.borrow()); // "Successfully borrowed 'JavaScript Guide'"
console.log(book1.getAvailability()); // "2 of 3 copies available"

console.log(library.listAvailableBooks());
// ["JavaScript Guide by John Doe", "Python Basics by Jane Smith"]
```

### Example 2: Game Character System

```javascript
class Character {
  constructor(name, health, attack, defense) {
    this.name = name;
    this.health = health;
    this.maxHealth = health;
    this.attack = attack;
    this.defense = defense;
    this.level = 1;
    this.experience = 0;
  }
  
  attack(target) {
    const damage = Math.max(1, this.attack - target.defense);
    target.takeDamage(damage);
    return `${this.name} attacks ${target.name} for ${damage} damage`;
  }
  
  takeDamage(damage) {
    this.health = Math.max(0, this.health - damage);
    return this.health === 0 ? `${this.name} is defeated!` : `${this.name} takes ${damage} damage`;
  }
  
  heal(amount) {
    this.health = Math.min(this.maxHealth, this.health + amount);
    return `${this.name} heals for ${amount} health`;
  }
  
  levelUp() {
    this.level++;
    this.maxHealth += 10;
    this.health = this.maxHealth;
    this.attack += 2;
    this.defense += 1;
    return `${this.name} leveled up to level ${this.level}!`;
  }
  
  getStatus() {
    return `${this.name} (Level ${this.level}): ${this.health}/${this.maxHealth} HP, ${this.attack} ATK, ${this.defense} DEF`;
  }
}

class Warrior extends Character {
  constructor(name) {
    super(name, 120, 15, 8);
    this.specialAttack = "Berserker Rage";
  }
  
  berserkerRage(target) {
    const damage = this.attack * 2;
    target.takeDamage(damage);
    this.health -= 10; // Cost of special attack
    return `${this.name} uses ${this.specialAttack} for ${damage} damage!`;
  }
}

class Mage extends Character {
  constructor(name) {
    super(name, 80, 20, 3);
    this.mana = 100;
    this.maxMana = 100;
  }
  
  fireball(target) {
    if (this.mana >= 20) {
      this.mana -= 20;
      const damage = this.attack + 10;
      target.takeDamage(damage);
      return `${this.name} casts Fireball for ${damage} damage!`;
    }
    return `${this.name} doesn't have enough mana!`;
  }
  
  heal() {
    if (this.mana >= 15) {
      this.mana -= 15;
      return super.heal(25);
    }
    return `${this.name} doesn't have enough mana to heal!`;
  }
}

// Usage
const warrior = new Warrior("Conan");
const mage = new Mage("Gandalf");

console.log(warrior.getStatus());
console.log(mage.getStatus());

console.log(warrior.attack(mage));
console.log(mage.getStatus());

console.log(mage.fireball(warrior));
console.log(warrior.getStatus());

console.log(warrior.berserkerRage(mage));
console.log(mage.getStatus());
```

## Exercises

### Exercise 1: Vehicle Classes
Create a `Vehicle` class with properties for `make`, `model`, and `year`. Create subclasses `Car` and `Motorcycle` that inherit from `Vehicle`. Add specific methods for each subclass.

### Exercise 2: Student Management
Create a `Student` class with properties for name, age, and grades. Add methods to calculate GPA, add grades, and get student information.

### Exercise 3: Bank Account with Interest
Extend the `BankAccount` class to include different account types (Savings, Checking) with different interest rates and minimum balance requirements.

### Exercise 4: Shape Calculator
Create a `Shape` base class and subclasses for `Circle`, `Rectangle`, and `Triangle`. Each shape should have methods to calculate area and perimeter.

### Exercise 5: Library with Categories
Extend the library system to include book categories (Fiction, Non-fiction, Science, etc.) and allow filtering books by category.

## Key Takeaways

- Classes provide a cleaner syntax for object-oriented programming
- Use `constructor` to initialize object properties
- Static methods belong to the class, not instances
- Getters and setters provide controlled access to properties
- Inheritance allows classes to extend other classes
- Private fields (with `#`) provide encapsulation
- Classes can be defined using expressions
- Always use `super()` when extending classes

Classes make JavaScript code more organized and easier to understand, especially for complex applications with many related objects.

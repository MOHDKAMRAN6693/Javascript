# 25 - Design Patterns

Design patterns are reusable solutions to common problems in software design. They provide templates for solving recurring design problems and help create more maintainable, scalable, and flexible code.

## 📚 Table of Contents
- [Creational Patterns](#creational-patterns)
- [Structural Patterns](#structural-patterns)
- [Behavioral Patterns](#behavioral-patterns)
- [JavaScript-Specific Patterns](#javascript-specific-patterns)
- [Modern JavaScript Patterns](#modern-javascript-patterns)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## Creational Patterns

### Singleton Pattern
Ensures a class has only one instance and provides global access to it.

```javascript
class DatabaseConnection {
  constructor() {
    if (DatabaseConnection.instance) {
      return DatabaseConnection.instance;
    }
    
    this.connection = null;
    DatabaseConnection.instance = this;
  }
  
  connect(url) {
    if (!this.connection) {
      this.connection = { url, connected: true };
      console.log(`Connected to ${url}`);
    }
    return this.connection;
  }
  
  disconnect() {
    if (this.connection) {
      this.connection = null;
      console.log('Disconnected from database');
    }
  }
}

// Usage
const db1 = new DatabaseConnection();
const db2 = new DatabaseConnection();

console.log(db1 === db2); // true (same instance)

db1.connect('mongodb://localhost:27017');
db2.connect('postgresql://localhost:5432'); // Still uses same connection
```

### Factory Pattern
Creates objects without specifying their exact class.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    throw new Error('Method must be implemented');
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks`;
  }
}

class Cat extends Animal {
  speak() {
    return `${this.name} meows`;
  }
}

class Bird extends Animal {
  speak() {
    return `${this.name} chirps`;
  }
}

class AnimalFactory {
  static createAnimal(type, name) {
    switch (type.toLowerCase()) {
      case 'dog':
        return new Dog(name);
      case 'cat':
        return new Cat(name);
      case 'bird':
        return new Bird(name);
      default:
        throw new Error(`Unknown animal type: ${type}`);
    }
  }
}

// Usage
const dog = AnimalFactory.createAnimal('dog', 'Buddy');
const cat = AnimalFactory.createAnimal('cat', 'Whiskers');
const bird = AnimalFactory.createAnimal('bird', 'Tweety');

console.log(dog.speak()); // "Buddy barks"
console.log(cat.speak()); // "Whiskers meows"
console.log(bird.speak()); // "Tweety chirps"
```

### Builder Pattern
Constructs complex objects step by step.

```javascript
class Pizza {
  constructor() {
    this.size = null;
    this.crust = null;
    this.toppings = [];
    this.sauce = null;
    this.cheese = null;
  }
  
  toString() {
    return `Pizza: ${this.size} ${this.crust} crust with ${this.toppings.join(', ')} toppings, ${this.sauce} sauce, and ${this.cheese} cheese`;
  }
}

class PizzaBuilder {
  constructor() {
    this.pizza = new Pizza();
  }
  
  setSize(size) {
    this.pizza.size = size;
    return this;
  }
  
  setCrust(crust) {
    this.pizza.crust = crust;
    return this;
  }
  
  addTopping(topping) {
    this.pizza.toppings.push(topping);
    return this;
  }
  
  setSauce(sauce) {
    this.pizza.sauce = sauce;
    return this;
  }
  
  setCheese(cheese) {
    this.pizza.cheese = cheese;
    return this;
  }
  
  build() {
    return this.pizza;
  }
}

// Usage
const pizza = new PizzaBuilder()
  .setSize('Large')
  .setCrust('Thin')
  .addTopping('Pepperoni')
  .addTopping('Mushrooms')
  .setSauce('Tomato')
  .setCheese('Mozzarella')
  .build();

console.log(pizza.toString());
```

### Prototype Pattern
Creates objects by cloning existing objects.

```javascript
class Shape {
  constructor() {
    this.type = null;
    this.color = null;
    this.x = 0;
    this.y = 0;
  }
  
  clone() {
    const cloned = Object.create(Object.getPrototypeOf(this));
    Object.assign(cloned, this);
    return cloned;
  }
  
  draw() {
    console.log(`Drawing ${this.color} ${this.type} at (${this.x}, ${this.y})`);
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.type = 'Circle';
    this.radius = radius;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.type = 'Rectangle';
    this.width = width;
    this.height = height;
  }
}

// Usage
const redCircle = new Circle(5);
redCircle.color = 'red';
redCircle.x = 10;
redCircle.y = 20;

const blueCircle = redCircle.clone();
blueCircle.color = 'blue';
blueCircle.x = 30;
blueCircle.y = 40;

redCircle.draw(); // "Drawing red Circle at (10, 20)"
blueCircle.draw(); // "Drawing blue Circle at (30, 40)"
```

## Structural Patterns

### Adapter Pattern
Allows incompatible interfaces to work together.

```javascript
// Old interface
class OldPrinter {
  printText(text) {
    console.log(`Old printer: ${text}`);
  }
}

// New interface
class NewPrinter {
  print(message) {
    console.log(`New printer: ${message}`);
  }
}

// Adapter
class PrinterAdapter {
  constructor(oldPrinter) {
    this.oldPrinter = oldPrinter;
  }
  
  print(message) {
    this.oldPrinter.printText(message);
  }
}

// Usage
const oldPrinter = new OldPrinter();
const newPrinter = new NewPrinter();
const adapter = new PrinterAdapter(oldPrinter);

newPrinter.print('Hello World');
adapter.print('Hello World');
```

### Decorator Pattern
Adds behavior to objects dynamically.

```javascript
class Coffee {
  getCost() {
    return 2.0;
  }
  
  getDescription() {
    return 'Simple coffee';
  }
}

class CoffeeDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  
  getCost() {
    return this.coffee.getCost();
  }
  
  getDescription() {
    return this.coffee.getDescription();
  }
}

class MilkDecorator extends CoffeeDecorator {
  getCost() {
    return this.coffee.getCost() + 0.5;
  }
  
  getDescription() {
    return this.coffee.getDescription() + ', milk';
  }
}

class SugarDecorator extends CoffeeDecorator {
  getCost() {
    return this.coffee.getCost() + 0.2;
  }
  
  getDescription() {
    return this.coffee.getDescription() + ', sugar';
  }
}

// Usage
let coffee = new Coffee();
console.log(`${coffee.getDescription()}: $${coffee.getCost()}`);

coffee = new MilkDecorator(coffee);
console.log(`${coffee.getDescription()}: $${coffee.getCost()}`);

coffee = new SugarDecorator(coffee);
console.log(`${coffee.getDescription()}: $${coffee.getCost()}`);
```

### Facade Pattern
Provides a simplified interface to a complex subsystem.

```javascript
class CPU {
  freeze() {
    console.log('CPU frozen');
  }
  
  jump(position) {
    console.log(`CPU jumping to position ${position}`);
  }
  
  execute() {
    console.log('CPU executing');
  }
}

class Memory {
  load(position, data) {
    console.log(`Memory loading data at position ${position}`);
  }
}

class HardDrive {
  read(lba, size) {
    console.log(`Hard drive reading ${size} bytes from LBA ${lba}`);
    return 'boot data';
  }
}

class ComputerFacade {
  constructor() {
    this.cpu = new CPU();
    this.memory = new Memory();
    this.hardDrive = new HardDrive();
  }
  
  start() {
    console.log('Starting computer...');
    this.cpu.freeze();
    this.memory.load(0, this.hardDrive.read(0, 1024));
    this.cpu.jump(0);
    this.cpu.execute();
    console.log('Computer started successfully');
  }
}

// Usage
const computer = new ComputerFacade();
computer.start();
```

### Proxy Pattern
Provides a placeholder for another object to control access.

```javascript
class RealImage {
  constructor(filename) {
    this.filename = filename;
    this.loadFromDisk();
  }
  
  loadFromDisk() {
    console.log(`Loading ${this.filename} from disk`);
  }
  
  display() {
    console.log(`Displaying ${this.filename}`);
  }
}

class ProxyImage {
  constructor(filename) {
    this.filename = filename;
    this.realImage = null;
  }
  
  display() {
    if (!this.realImage) {
      this.realImage = new RealImage(this.filename);
    }
    this.realImage.display();
  }
}

// Usage
const image = new ProxyImage('photo.jpg');
console.log('Image created, but not loaded yet');

image.display(); // Now loads and displays
image.display(); // Uses cached image
```

## Behavioral Patterns

### Observer Pattern
Defines a one-to-many dependency between objects.

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }
  
  addObserver(observer) {
    this.observers.push(observer);
  }
  
  removeObserver(observer) {
    const index = this.observers.indexOf(observer);
    if (index > -1) {
      this.observers.splice(index, 1);
    }
  }
  
  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}

class Observer {
  constructor(name) {
    this.name = name;
  }
  
  update(data) {
    console.log(`${this.name} received: ${data}`);
  }
}

class NewsAgency extends Subject {
  constructor() {
    super();
    this.news = null;
  }
  
  setNews(news) {
    this.news = news;
    this.notify(news);
  }
}

// Usage
const newsAgency = new NewsAgency();
const subscriber1 = new Observer('Subscriber 1');
const subscriber2 = new Observer('Subscriber 2');

newsAgency.addObserver(subscriber1);
newsAgency.addObserver(subscriber2);

newsAgency.setNews('Breaking: JavaScript is awesome!');
```

### Strategy Pattern
Defines a family of algorithms and makes them interchangeable.

```javascript
class PaymentStrategy {
  pay(amount) {
    throw new Error('Method must be implemented');
  }
}

class CreditCardPayment extends PaymentStrategy {
  constructor(cardNumber, cvv) {
    super();
    this.cardNumber = cardNumber;
    this.cvv = cvv;
  }
  
  pay(amount) {
    console.log(`Paid $${amount} using credit card ending in ${this.cardNumber.slice(-4)}`);
  }
}

class PayPalPayment extends PaymentStrategy {
  constructor(email) {
    super();
    this.email = email;
  }
  
  pay(amount) {
    console.log(`Paid $${amount} using PayPal account ${this.email}`);
  }
}

class BankTransferPayment extends PaymentStrategy {
  constructor(accountNumber) {
    super();
    this.accountNumber = accountNumber;
  }
  
  pay(amount) {
    console.log(`Paid $${amount} using bank transfer to account ${this.accountNumber}`);
  }
}

class PaymentProcessor {
  constructor() {
    this.paymentStrategy = null;
  }
  
  setPaymentStrategy(strategy) {
    this.paymentStrategy = strategy;
  }
  
  processPayment(amount) {
    if (!this.paymentStrategy) {
      throw new Error('Payment strategy not set');
    }
    this.paymentStrategy.pay(amount);
  }
}

// Usage
const processor = new PaymentProcessor();

processor.setPaymentStrategy(new CreditCardPayment('1234567890123456', '123'));
processor.processPayment(100);

processor.setPaymentStrategy(new PayPalPayment('user@example.com'));
processor.processPayment(50);

processor.setPaymentStrategy(new BankTransferPayment('987654321'));
processor.processPayment(200);
```

### Command Pattern
Encapsulates a request as an object.

```javascript
class Command {
  execute() {
    throw new Error('Method must be implemented');
  }
  
  undo() {
    throw new Error('Method must be implemented');
  }
}

class Light {
  constructor() {
    this.isOn = false;
  }
  
  turnOn() {
    this.isOn = true;
    console.log('Light is on');
  }
  
  turnOff() {
    this.isOn = false;
    console.log('Light is off');
  }
}

class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }
  
  execute() {
    this.light.turnOn();
  }
  
  undo() {
    this.light.turnOff();
  }
}

class LightOffCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }
  
  execute() {
    this.light.turnOff();
  }
  
  undo() {
    this.light.turnOn();
  }
}

class RemoteControl {
  constructor() {
    this.commands = [];
    this.currentCommand = null;
  }
  
  setCommand(command) {
    this.currentCommand = command;
  }
  
  pressButton() {
    if (this.currentCommand) {
      this.currentCommand.execute();
      this.commands.push(this.currentCommand);
    }
  }
  
  pressUndo() {
    if (this.commands.length > 0) {
      const lastCommand = this.commands.pop();
      lastCommand.undo();
    }
  }
}

// Usage
const light = new Light();
const lightOn = new LightOnCommand(light);
const lightOff = new LightOffCommand(light);

const remote = new RemoteControl();

remote.setCommand(lightOn);
remote.pressButton(); // Light is on

remote.setCommand(lightOff);
remote.pressButton(); // Light is off

remote.pressUndo(); // Light is on
remote.pressUndo(); // Light is off
```

### State Pattern
Allows an object to alter its behavior when its internal state changes.

```javascript
class State {
  handle(context) {
    throw new Error('Method must be implemented');
  }
}

class ConcreteStateA extends State {
  handle(context) {
    console.log('Handling in State A');
    context.setState(new ConcreteStateB());
  }
}

class ConcreteStateB extends State {
  handle(context) {
    console.log('Handling in State B');
    context.setState(new ConcreteStateA());
  }
}

class Context {
  constructor() {
    this.state = new ConcreteStateA();
  }
  
  setState(state) {
    this.state = state;
  }
  
  request() {
    this.state.handle(this);
  }
}

// Usage
const context = new Context();
context.request(); // Handling in State A
context.request(); // Handling in State B
context.request(); // Handling in State A
```

## JavaScript-Specific Patterns

### Module Pattern
Encapsulates code and provides a clean interface.

```javascript
const Calculator = (function() {
  let result = 0;
  
  function add(x) {
    result += x;
    return this;
  }
  
  function subtract(x) {
    result -= x;
    return this;
  }
  
  function multiply(x) {
    result *= x;
    return this;
  }
  
  function divide(x) {
    if (x === 0) {
      throw new Error('Division by zero');
    }
    result /= x;
    return this;
  }
  
  function getResult() {
    return result;
  }
  
  function reset() {
    result = 0;
    return this;
  }
  
  return {
    add,
    subtract,
    multiply,
    divide,
    getResult,
    reset
  };
})();

// Usage
const result = Calculator
  .add(10)
  .multiply(2)
  .subtract(5)
  .getResult();

console.log(result); // 15
```

### Revealing Module Pattern
Similar to Module Pattern but with explicit public API.

```javascript
const UserManager = (function() {
  let users = [];
  let currentUser = null;
  
  function addUser(user) {
    users.push(user);
  }
  
  function removeUser(id) {
    users = users.filter(user => user.id !== id);
  }
  
  function getUsers() {
    return [...users]; // Return copy to prevent external modification
  }
  
  function setCurrentUser(user) {
    currentUser = user;
  }
  
  function getCurrentUser() {
    return currentUser;
  }
  
  // Public API
  return {
    addUser,
    removeUser,
    getUsers,
    setCurrentUser,
    getCurrentUser
  };
})();
```

## Modern JavaScript Patterns

### ES6 Class Patterns

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }
  
  on(event, listener) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(listener);
    return this;
  }
  
  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach(listener => listener(...args));
    }
    return this;
  }
  
  off(event, listener) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(l => l !== listener);
    }
    return this;
  }
}

// Usage
const emitter = new EventEmitter();
emitter.on('data', (data) => console.log('Received:', data));
emitter.emit('data', 'Hello World');
```

### Promise Patterns

```javascript
class PromiseQueue {
  constructor(concurrency = 1) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }
  
  add(promiseFactory) {
    return new Promise((resolve, reject) => {
      this.queue.push({
        promiseFactory,
        resolve,
        reject
      });
      this.process();
    });
  }
  
  async process() {
    if (this.running >= this.concurrency || this.queue.length === 0) {
      return;
    }
    
    this.running++;
    const { promiseFactory, resolve, reject } = this.queue.shift();
    
    try {
      const result = await promiseFactory();
      resolve(result);
    } catch (error) {
      reject(error);
    } finally {
      this.running--;
      this.process();
    }
  }
}

// Usage
const queue = new PromiseQueue(2);

const tasks = [
  () => new Promise(resolve => setTimeout(() => resolve('Task 1'), 1000)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 2'), 1000)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 3'), 1000)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 4'), 1000))
];

tasks.forEach(task => {
  queue.add(task).then(result => console.log(result));
});
```

## Practical Examples

### Example 1: MVC Pattern

```javascript
// Model
class TodoModel {
  constructor() {
    this.todos = [];
    this.listeners = [];
  }
  
  addTodo(todo) {
    this.todos.push({ ...todo, id: Date.now() });
    this.notifyListeners();
  }
  
  removeTodo(id) {
    this.todos = this.todos.filter(todo => todo.id !== id);
    this.notifyListeners();
  }
  
  toggleTodo(id) {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      this.notifyListeners();
    }
  }
  
  getTodos() {
    return [...this.todos];
  }
  
  addListener(listener) {
    this.listeners.push(listener);
  }
  
  notifyListeners() {
    this.listeners.forEach(listener => listener(this.todos));
  }
}

// View
class TodoView {
  constructor(containerId) {
    this.container = document.getElementById(containerId);
  }
  
  render(todos) {
    this.container.innerHTML = `
      <ul>
        ${todos.map(todo => `
          <li class="${todo.completed ? 'completed' : ''}">
            <input type="checkbox" ${todo.completed ? 'checked' : ''} 
                   onchange="controller.toggleTodo(${todo.id})">
            <span>${todo.text}</span>
            <button onclick="controller.removeTodo(${todo.id})">Delete</button>
          </li>
        `).join('')}
      </ul>
    `;
  }
}

// Controller
class TodoController {
  constructor(model, view) {
    this.model = model;
    this.view = view;
    
    this.model.addListener(todos => this.view.render(todos));
  }
  
  addTodo(text) {
    this.model.addTodo({ text, completed: false });
  }
  
  removeTodo(id) {
    this.model.removeTodo(id);
  }
  
  toggleTodo(id) {
    this.model.toggleTodo(id);
  }
}

// Usage
const model = new TodoModel();
const view = new TodoView('todo-container');
const controller = new TodoController(model, view);

// Add some todos
controller.addTodo('Learn JavaScript');
controller.addTodo('Build a todo app');
controller.addTodo('Master design patterns');
```

### Example 2: Repository Pattern

```javascript
class UserRepository {
  constructor() {
    this.users = new Map();
  }
  
  create(user) {
    const id = Date.now();
    const newUser = { ...user, id };
    this.users.set(id, newUser);
    return newUser;
  }
  
  findById(id) {
    return this.users.get(id);
  }
  
  findAll() {
    return Array.from(this.users.values());
  }
  
  update(id, updates) {
    const user = this.users.get(id);
    if (user) {
      const updatedUser = { ...user, ...updates };
      this.users.set(id, updatedUser);
      return updatedUser;
    }
    return null;
  }
  
  delete(id) {
    return this.users.delete(id);
  }
  
  findByEmail(email) {
    return this.findAll().find(user => user.email === email);
  }
}

class UserService {
  constructor(userRepository) {
    this.userRepository = userRepository;
  }
  
  createUser(userData) {
    // Validation
    if (!userData.email || !userData.name) {
      throw new Error('Email and name are required');
    }
    
    if (this.userRepository.findByEmail(userData.email)) {
      throw new Error('User with this email already exists');
    }
    
    return this.userRepository.create(userData);
  }
  
  getUser(id) {
    const user = this.userRepository.findById(id);
    if (!user) {
      throw new Error('User not found');
    }
    return user;
  }
  
  updateUser(id, updates) {
    const user = this.userRepository.findById(id);
    if (!user) {
      throw new Error('User not found');
    }
    
    return this.userRepository.update(id, updates);
  }
  
  deleteUser(id) {
    const deleted = this.userRepository.delete(id);
    if (!deleted) {
      throw new Error('User not found');
    }
    return true;
  }
  
  getAllUsers() {
    return this.userRepository.findAll();
  }
}

// Usage
const userRepository = new UserRepository();
const userService = new UserService(userRepository);

try {
  const user = userService.createUser({
    name: 'John Doe',
    email: 'john@example.com'
  });
  console.log('User created:', user);
  
  const foundUser = userService.getUser(user.id);
  console.log('Found user:', foundUser);
  
  const updatedUser = userService.updateUser(user.id, { name: 'Jane Doe' });
  console.log('Updated user:', updatedUser);
  
} catch (error) {
  console.error('Error:', error.message);
}
```

## Exercises

### Exercise 1: Shopping Cart with Patterns
Implement a shopping cart using the Observer pattern for notifications and the Strategy pattern for different payment methods.

### Exercise 2: Game Engine with State Pattern
Create a simple game with different states (menu, playing, paused, game over) using the State pattern.

### Exercise 3: Caching System with Proxy Pattern
Build a caching system that uses the Proxy pattern to cache expensive operations.

### Exercise 4: Event System with Command Pattern
Implement an event system that supports undo/redo functionality using the Command pattern.

### Exercise 5: Plugin System with Factory Pattern
Create a plugin system that allows dynamic loading of different types of plugins using the Factory pattern.

## Key Takeaways

- Design patterns provide reusable solutions to common problems
- Creational patterns focus on object creation
- Structural patterns deal with object composition
- Behavioral patterns define communication between objects
- Choose patterns based on your specific requirements
- Don't over-engineer - use patterns when they add value
- Modern JavaScript features can simplify some traditional patterns
- Patterns should improve code maintainability and flexibility
- Consider the trade-offs between complexity and benefits
- Practice implementing patterns to understand them better

Design patterns are powerful tools that help create more maintainable, flexible, and scalable JavaScript applications.

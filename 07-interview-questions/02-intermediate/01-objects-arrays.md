# JavaScript Interview Questions - Intermediate Level

## Objects & Arrays

### 1. What are the different ways to create objects in JavaScript?

**Answer:**
There are several ways to create objects in JavaScript, each with its own use case:

**1. Object Literal:**
```javascript
const person = {
  name: 'John',
  age: 30,
  greet() {
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
  greet() {
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

### 2. What is object destructuring and how do you use it?

**Answer:**
Object destructuring allows you to extract properties from objects and assign them to variables with a concise syntax.

**Basic Destructuring:**
```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York',
  country: 'USA'
};

// Extract specific properties
const { name, age } = person;
console.log(name); // 'John'
console.log(age);  // 30

// Extract with different variable names
const { name: personName, age: personAge } = person;
console.log(personName); // 'John'
console.log(personAge);  // 30

// Extract with default values
const { name, age, salary = 50000 } = person;
console.log(salary); // 50000 (default value)
```

**Nested Destructuring:**
```javascript
const user = {
  name: 'John',
  address: {
    street: '123 Main St',
    city: 'New York',
    coordinates: {
      lat: 40.7128,
      lng: -74.0060
    }
  }
};

// Nested destructuring
const { 
  name, 
  address: { 
    city, 
    coordinates: { lat, lng } 
  } 
} = user;

console.log(name); // 'John'
console.log(city); // 'New York'
console.log(lat);  // 40.7128
console.log(lng);  // -74.0060
```

**Function Parameters:**
```javascript
function greetUser({ name, age = 18 }) {
  console.log(`Hello ${name}, you are ${age} years old`);
}

greetUser({ name: 'John', age: 30 }); // "Hello John, you are 30 years old"
greetUser({ name: 'Jane' }); // "Hello Jane, you are 18 years old"

// Rest properties
function processUser({ name, ...otherProps }) {
  console.log(`Processing ${name}`);
  console.log('Other properties:', otherProps);
}

processUser({ name: 'John', age: 30, city: 'NYC' });
```

### 3. What are the different array methods and when to use them?

**Answer:**
JavaScript provides many array methods for different operations:

**Transformation Methods:**
```javascript
const numbers = [1, 2, 3, 4, 5];

// map() - transforms each element
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// flatMap() - maps and flattens
const nested = [[1, 2], [3, 4], [5, 6]];
const flattened = nested.flatMap(arr => arr.map(x => x * 2));
console.log(flattened); // [2, 4, 6, 8, 10, 12]
```

**Filtering Methods:**
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// filter() - selects elements based on condition
const evens = numbers.filter(x => x % 2 === 0);
console.log(evens); // [2, 4, 6, 8, 10]

// find() - finds first element that matches
const firstEven = numbers.find(x => x % 2 === 0);
console.log(firstEven); // 2

// findIndex() - finds index of first matching element
const firstEvenIndex = numbers.findIndex(x => x % 2 === 0);
console.log(firstEvenIndex); // 1
```

**Reduction Methods:**
```javascript
const numbers = [1, 2, 3, 4, 5];

// reduce() - reduces array to single value
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15

// reduceRight() - reduces from right to left
const concatenated = numbers.reduceRight((acc, curr) => acc + curr.toString(), '');
console.log(concatenated); // '54321'
```

**Iteration Methods:**
```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach() - executes function for each element
numbers.forEach((num, index) => {
  console.log(`Index ${index}: ${num}`);
});

// some() - tests if any element passes test
const hasEven = numbers.some(x => x % 2 === 0);
console.log(hasEven); // true

// every() - tests if all elements pass test
const allPositive = numbers.every(x => x > 0);
console.log(allPositive); // true
```

### 4. What is the difference between shallow copy and deep copy?

**Answer:**
- **Shallow Copy**: Creates a new object/array but copies references to nested objects/arrays
- **Deep Copy**: Creates a completely new object/array with all nested objects/arrays also copied

**Shallow Copy Examples:**
```javascript
const original = {
  name: 'John',
  age: 30,
  address: {
    city: 'New York',
    country: 'USA'
  }
};

// Shallow copy methods
const shallow1 = Object.assign({}, original);
const shallow2 = { ...original };
const shallow3 = Object.create(Object.getPrototypeOf(original), 
  Object.getOwnPropertyDescriptors(original));

// Modifying nested object affects original
shallow1.address.city = 'Boston';
console.log(original.address.city); // 'Boston' - original is affected!
```

**Deep Copy Examples:**
```javascript
// Method 1: JSON methods (limitations with functions, undefined, symbols)
const deepCopy1 = JSON.parse(JSON.stringify(original));

// Method 2: Recursive function
function deepCopy(obj) {
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  if (obj instanceof Date) {
    return new Date(obj.getTime());
  }
  
  if (obj instanceof Array) {
    return obj.map(item => deepCopy(item));
  }
  
  if (typeof obj === 'object') {
    const copy = {};
    Object.keys(obj).forEach(key => {
      copy[key] = deepCopy(obj[key]);
    });
    return copy;
  }
}

const deepCopy2 = deepCopy(original);

// Method 3: Using structuredClone (modern browsers)
const deepCopy3 = structuredClone(original);
```

### 5. What are the different ways to iterate over objects and arrays?

**Answer:**
There are multiple ways to iterate over objects and arrays:

**Array Iteration:**
```javascript
const fruits = ['apple', 'banana', 'orange'];

// 1. for loop
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// 2. for...of loop
for (const fruit of fruits) {
  console.log(fruit);
}

// 3. forEach method
fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});

// 4. for...in loop (not recommended for arrays)
for (const index in fruits) {
  console.log(fruits[index]);
}
```

**Object Iteration:**
```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

// 1. for...in loop
for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}

// 2. Object.keys()
Object.keys(person).forEach(key => {
  console.log(`${key}: ${person[key]}`);
});

// 3. Object.values()
Object.values(person).forEach(value => {
  console.log(value);
});

// 4. Object.entries()
Object.entries(person).forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});

// 5. for...of with Object.entries()
for (const [key, value] of Object.entries(person)) {
  console.log(`${key}: ${value}`);
}
```

### 6. What is the difference between `Object.keys()`, `Object.values()`, and `Object.entries()`?

**Answer:**
These methods return different representations of an object's properties:

**Object.keys()** - Returns an array of property names
**Object.values()** - Returns an array of property values  
**Object.entries()** - Returns an array of [key, value] pairs

**Examples:**
```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

console.log(Object.keys(person));    // ['name', 'age', 'city']
console.log(Object.values(person));  // ['John', 30, 'New York']
console.log(Object.entries(person)); // [['name', 'John'], ['age', 30], ['city', 'New York']]

// Practical use cases
const user = {
  name: 'John',
  email: 'john@example.com',
  age: 30,
  isActive: true
};

// Get all property names
const properties = Object.keys(user);
console.log(properties); // ['name', 'email', 'age', 'isActive']

// Get all values
const values = Object.values(user);
console.log(values); // ['John', 'john@example.com', 30, true]

// Convert object to array of objects
const entries = Object.entries(user);
const userArray = entries.map(([key, value]) => ({ property: key, value }));
console.log(userArray);
// [
//   { property: 'name', value: 'John' },
//   { property: 'email', value: 'john@example.com' },
//   { property: 'age', value: 30 },
//   { property: 'isActive', value: true }
// ]
```

### 7. What are getters and setters in objects?

**Answer:**
Getters and setters are special methods that allow you to define how properties are accessed and modified.

**Basic Getters and Setters:**
```javascript
const person = {
  _name: 'John',
  _age: 30,
  
  get name() {
    return this._name;
  },
  
  set name(value) {
    if (typeof value !== 'string') {
      throw new Error('Name must be a string');
    }
    this._name = value;
  },
  
  get age() {
    return this._age;
  },
  
  set age(value) {
    if (typeof value !== 'number' || value < 0) {
      throw new Error('Age must be a positive number');
    }
    this._age = value;
  }
};

console.log(person.name); // 'John'
person.name = 'Jane';
console.log(person.name); // 'Jane'
// person.name = 123; // Error: Name must be a string
```

**Computed Properties:**
```javascript
const circle = {
  _radius: 5,
  
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

console.log(circle.area); // 78.54...
circle.radius = 10;
console.log(circle.area); // 314.16...
```

### 8. What is the difference between `Object.freeze()`, `Object.seal()`, and `Object.preventExtensions()`?

**Answer:**
These methods provide different levels of object immutability:

**Object.preventExtensions()** - Prevents adding new properties
**Object.seal()** - Prevents adding/removing properties, but allows modifying existing ones
**Object.freeze()** - Prevents all modifications (most restrictive)

**Examples:**
```javascript
const obj = { name: 'John', age: 30 };

// Object.preventExtensions()
Object.preventExtensions(obj);
obj.city = 'NYC'; // Silently fails or throws error in strict mode
obj.name = 'Jane'; // This works
delete obj.age; // This works
console.log(obj); // { name: 'Jane', age: 30 }

// Object.seal()
const sealedObj = { name: 'John', age: 30 };
Object.seal(sealedObj);
sealedObj.city = 'NYC'; // Silently fails
sealedObj.name = 'Jane'; // This works
delete sealedObj.age; // Silently fails
console.log(sealedObj); // { name: 'Jane', age: 30 }

// Object.freeze()
const frozenObj = { name: 'John', age: 30 };
Object.freeze(frozenObj);
frozenObj.city = 'NYC'; // Silently fails
frozenObj.name = 'Jane'; // Silently fails
delete frozenObj.age; // Silently fails
console.log(frozenObj); // { name: 'John', age: 30 }

// Checking object status
console.log(Object.isExtensible(obj)); // false
console.log(Object.isSealed(sealedObj)); // true
console.log(Object.isFrozen(frozenObj)); // true
```

### 9. What are array methods like `map()`, `filter()`, `reduce()` and how do you chain them?

**Answer:**
These are functional programming methods that transform arrays without mutating the original array.

**Basic Usage:**
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// map() - transforms each element
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

// filter() - selects elements based on condition
const evens = numbers.filter(x => x % 2 === 0);
console.log(evens); // [2, 4, 6, 8, 10]

// reduce() - reduces array to single value
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 55
```

**Method Chaining:**
```javascript
const products = [
  { name: 'Laptop', price: 1000, category: 'Electronics' },
  { name: 'Book', price: 20, category: 'Education' },
  { name: 'Phone', price: 800, category: 'Electronics' },
  { name: 'Pen', price: 2, category: 'Office' },
  { name: 'Tablet', price: 600, category: 'Electronics' }
];

// Chain multiple operations
const result = products
  .filter(product => product.category === 'Electronics') // Filter electronics
  .map(product => ({ ...product, price: product.price * 1.1 })) // Add 10% tax
  .filter(product => product.price > 500) // Filter expensive items
  .reduce((total, product) => total + product.price, 0); // Sum prices

console.log(result); // 2640 (1000*1.1 + 800*1.1 + 600*1.1)
```

**Advanced Examples:**
```javascript
const students = [
  { name: 'John', grades: [85, 90, 78, 92] },
  { name: 'Jane', grades: [88, 76, 94, 89] },
  { name: 'Bob', grades: [92, 88, 85, 90] }
];

// Calculate average grade for each student
const studentsWithAverages = students.map(student => ({
  ...student,
  average: student.grades.reduce((sum, grade) => sum + grade, 0) / student.grades.length
}));

// Filter students with average > 85
const topStudents = studentsWithAverages
  .filter(student => student.average > 85)
  .sort((a, b) => b.average - a.average);

console.log(topStudents);
```

### 10. What are the different ways to merge objects?

**Answer:**
There are several ways to merge objects and arrays in JavaScript:

**Object Merging:**
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };

// Object.assign() - shallow merge
const merged1 = Object.assign({}, obj1, obj2);
console.log(merged1); // { a: 1, b: 3, c: 4 }

// Spread operator - shallow merge
const merged2 = { ...obj1, ...obj2 };
console.log(merged2); // { a: 1, b: 3, c: 4 }

// Deep merge function
function deepMerge(target, source) {
  const result = { ...target };
  
  for (const key in source) {
    if (source[key] && typeof source[key] === 'object' && !Array.isArray(source[key])) {
      result[key] = deepMerge(target[key] || {}, source[key]);
    } else {
      result[key] = source[key];
    }
  }
  
  return result;
}

const obj3 = { a: 1, nested: { x: 1, y: 2 } };
const obj4 = { b: 2, nested: { y: 3, z: 4 } };
const deepMerged = deepMerge(obj3, obj4);
console.log(deepMerged); // { a: 1, b: 2, nested: { x: 1, y: 3, z: 4 } }
```

**Array Merging:**
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Spread operator
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4, 5, 6]

// concat() method
const merged2 = arr1.concat(arr2);
console.log(merged2); // [1, 2, 3, 4, 5, 6]

// Remove duplicates
const withDuplicates = [1, 2, 2, 3, 3, 4];
const unique = [...new Set(withDuplicates)];
console.log(unique); // [1, 2, 3, 4]
```

## Practice Questions

1. **What will be the output?**
```javascript
const obj = { a: 1, b: 2 };
const { a, b, c = 3 } = obj;
console.log(a, b, c);
```

2. **Explain the output:**
```javascript
const arr = [1, 2, 3, 4, 5];
const result = arr.map(x => x * 2).filter(x => x > 5);
console.log(result);
```

3. **What's the difference between these two?**
```javascript
const obj1 = Object.assign({}, original);
const obj2 = { ...original };
```

4. **Predict the output:**
```javascript
const person = {
  name: 'John',
  get fullName() {
    return this.name + ' Doe';
  }
};
console.log(person.fullName);
```

5. **What will this return?**
```javascript
const numbers = [1, 2, 3, 4, 5];
const result = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(result);
```

## Key Takeaways

- Master object creation patterns and when to use each
- Understand destructuring for clean code
- Know the differences between array methods
- Understand shallow vs deep copying
- Practice method chaining for functional programming
- Learn object immutability methods
- Master getters and setters for data validation
- Practice with complex object and array operations

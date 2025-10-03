# 17 - Destructuring

## What is Destructuring?

Destructuring is a JavaScript expression that allows you to extract values from arrays or properties from objects into distinct variables. It provides a concise way to unpack values from data structures.

## Array Destructuring

### Basic Array Destructuring
```javascript
let colors = ["red", "green", "blue"];

// Basic destructuring
let [first, second, third] = colors;
console.log(first);  // "red"
console.log(second); // "green"
console.log(third);  // "blue"
```

### Skipping Elements
```javascript
let numbers = [1, 2, 3, 4, 5];

// Skip elements using empty slots
let [first, , third, , fifth] = numbers;
console.log(first);  // 1
console.log(third);  // 3
console.log(fifth);  // 5
```

### Default Values
```javascript
let colors = ["red"];

// Provide default values
let [first, second = "green", third = "blue"] = colors;
console.log(first);  // "red"
console.log(second); // "green" (default)
console.log(third);  // "blue" (default)
```

### Rest Elements
```javascript
let numbers = [1, 2, 3, 4, 5];

// Extract first element and rest
let [first, ...rest] = numbers;
console.log(first); // 1
console.log(rest);  // [2, 3, 4, 5]

// Extract first two elements and rest
let [first, second, ...others] = numbers;
console.log(first);  // 1
console.log(second); // 2
console.log(others); // [3, 4, 5]
```

### Swapping Variables
```javascript
let a = 1;
let b = 2;

// Swap values using destructuring
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

## Object Destructuring

### Basic Object Destructuring
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

// Basic destructuring
let { name, age, city } = person;
console.log(name); // "John"
console.log(age);  // 25
console.log(city); // "New York"
```

### Renaming Variables
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

// Rename variables during destructuring
let { name: fullName, age: years, city: location } = person;
console.log(fullName); // "John"
console.log(years);    // 25
console.log(location); // "New York"
```

### Default Values
```javascript
let person = {
    name: "John",
    age: 25
};

// Provide default values
let { name, age, city = "Unknown", country = "USA" } = person;
console.log(name);    // "John"
console.log(age);     // 25
console.log(city);    // "Unknown" (default)
console.log(country); // "USA" (default)
```

### Nested Object Destructuring
```javascript
let user = {
    name: "John",
    age: 25,
    address: {
        street: "123 Main St",
        city: "New York",
        country: "USA"
    }
};

// Nested destructuring
let { name, address: { street, city, country } } = user;
console.log(name);    // "John"
console.log(street); // "123 Main St"
console.log(city);    // "New York"
console.log(country); // "USA"
```

### Rest Properties
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York",
    country: "USA"
};

// Extract specific properties and rest
let { name, age, ...otherInfo } = person;
console.log(name);      // "John"
console.log(age);       // 25
console.log(otherInfo); // { city: "New York", country: "USA" }
```

## Function Parameter Destructuring

### Array Parameters
```javascript
function processCoordinates([x, y, z = 0]) {
    console.log(`X: ${x}, Y: ${y}, Z: ${z}`);
}

let coordinates = [10, 20];
processCoordinates(coordinates); // X: 10, Y: 20, Z: 0
```

### Object Parameters
```javascript
function greetUser({ name, age, city = "Unknown" }) {
    console.log(`Hello ${name}, you are ${age} years old from ${city}`);
}

let user = { name: "John", age: 25 };
greetUser(user); // Hello John, you are 25 years old from Unknown
```

### Mixed Parameters
```javascript
function processData([first, second], { name, age }) {
    console.log(`Numbers: ${first}, ${second}`);
    console.log(`User: ${name}, ${age}`);
}

let numbers = [1, 2];
let user = { name: "John", age: 25 };
processData(numbers, user);
```

## Advanced Destructuring Patterns

### Destructuring with Computed Properties
```javascript
let prop = "name";
let person = { name: "John", age: 25 };

// Destructure with computed property
let { [prop]: personName } = person;
console.log(personName); // "John"
```

### Destructuring with Dynamic Keys
```javascript
function extractProperty(obj, key) {
    let { [key]: value } = obj;
    return value;
}

let person = { name: "John", age: 25 };
console.log(extractProperty(person, "name")); // "John"
console.log(extractProperty(person, "age"));  // 25
```

### Destructuring with Functions
```javascript
function getCoordinates() {
    return [10, 20, 30];
}

function getUserInfo() {
    return { name: "John", age: 25, city: "New York" };
}

// Destructure function return values
let [x, y, z] = getCoordinates();
let { name, age } = getUserInfo();

console.log(x, y, z);    // 10 20 30
console.log(name, age);  // John 25
```

## Practical Examples

### Example 1: API Response Handling
```javascript
async function fetchUserData(userId) {
    const response = await fetch(`/api/users/${userId}`);
    const data = await response.json();
    
    // Destructure API response
    const { 
        id, 
        name, 
        email, 
        address: { 
            street, 
            city, 
            country 
        },
        preferences: {
            theme = "light",
            language = "en"
        } = {}
    } = data;
    
    return {
        id,
        name,
        email,
        location: `${street}, ${city}, ${country}`,
        theme,
        language
    };
}
```

### Example 2: Configuration Objects
```javascript
function createDatabaseConnection(config) {
    const {
        host = "localhost",
        port = 5432,
        database,
        username,
        password,
        ssl = false,
        timeout = 30000
    } = config;
    
    console.log(`Connecting to ${host}:${port}/${database}`);
    console.log(`SSL: ${ssl}, Timeout: ${timeout}ms`);
    
    // Use the destructured values
    return {
        host,
        port,
        database,
        username,
        password,
        ssl,
        timeout
    };
}

// Usage
const dbConfig = {
    host: "db.example.com",
    database: "myapp",
    username: "admin",
    password: "secret"
};

createDatabaseConnection(dbConfig);
```

### Example 3: Event Handling
```javascript
function handleMouseEvent({ clientX, clientY, button, ctrlKey, shiftKey }) {
    console.log(`Mouse clicked at (${clientX}, ${clientY})`);
    console.log(`Button: ${button}, Ctrl: ${ctrlKey}, Shift: ${shiftKey}`);
}

document.addEventListener("click", handleMouseEvent);
```

### Example 4: Array Processing
```javascript
function processUserList(users) {
    return users.map(({ id, name, email, ...otherInfo }) => {
        return {
            id,
            displayName: name,
            contact: email,
            metadata: otherInfo
        };
    });
}

let users = [
    { id: 1, name: "John", email: "john@example.com", age: 25, city: "NYC" },
    { id: 2, name: "Jane", email: "jane@example.com", age: 30, city: "LA" }
];

let processedUsers = processUserList(users);
console.log(processedUsers);
```

## Destructuring in Different Contexts

### Destructuring in Loops
```javascript
let users = [
    { name: "John", age: 25, city: "NYC" },
    { name: "Jane", age: 30, city: "LA" }
];

// Destructure in for...of loop
for (let { name, age, city } of users) {
    console.log(`${name} is ${age} years old from ${city}`);
}
```

### Destructuring in Function Returns
```javascript
function getCoordinates() {
    return [10, 20, 30];
}

function getUserInfo() {
    return { name: "John", age: 25 };
}

// Destructure multiple return values
let [x, y, z] = getCoordinates();
let { name, age } = getUserInfo();
```

### Destructuring with Default Parameters
```javascript
function createUser({ 
    name, 
    age, 
    email, 
    city = "Unknown",
    country = "USA",
    isActive = true 
} = {}) {
    return {
        name,
        age,
        email,
        location: `${city}, ${country}`,
        isActive
    };
}

// Usage with partial data
let user1 = createUser({ name: "John", age: 25, email: "john@example.com" });
let user2 = createUser({ name: "Jane", age: 30, email: "jane@example.com", city: "LA" });
```

## Practice Exercises

### Exercise 1: Array Destructuring
Create functions that use array destructuring to:
- Extract first and last elements
- Get middle elements
- Swap array elements
- Extract elements with default values

### Exercise 2: Object Destructuring
Create functions that use object destructuring to:
- Extract user information
- Handle nested objects
- Rename properties
- Use default values

### Exercise 3: Function Parameters
Create functions that use destructuring for:
- Array parameters
- Object parameters
- Mixed parameters
- Default parameter values

### Exercise 4: API Response Processing
Create functions that process API responses using destructuring:
- User data extraction
- Error handling
- Nested object processing
- Default value handling

### Exercise 5: Configuration Management
Create a configuration system that uses destructuring:
- Database configuration
- API configuration
- Application settings
- Environment variables

## Common Mistakes

### 1. Destructuring Non-iterable Values
```javascript
// Wrong - trying to destructure a non-iterable value
let [a, b] = null; // TypeError

// Correct - provide default value
let [a, b] = null || [];
```

### 2. Destructuring Undefined Objects
```javascript
// Wrong - destructuring undefined object
let { name, age } = undefined; // TypeError

// Correct - provide default value
let { name, age } = undefined || {};
```

### 3. Confusing Array and Object Destructuring
```javascript
// Wrong - using array syntax for object
let [name, age] = { name: "John", age: 25 }; // TypeError

// Correct - use object syntax
let { name, age } = { name: "John", age: 25 };
```

### 4. Not Using Default Values
```javascript
// Wrong - no default values
let { name, age, city } = { name: "John" };
console.log(age);  // undefined
console.log(city); // undefined

// Correct - provide default values
let { name, age = 0, city = "Unknown" } = { name: "John" };
console.log(age);  // 0
console.log(city); // "Unknown"
```

## Key Takeaways

- Destructuring provides a concise way to extract values from arrays and objects
- Use default values to handle missing properties
- Rename variables during destructuring when needed
- Destructuring works with function parameters
- Use rest syntax to capture remaining elements/properties
- Practice with real-world examples to understand different use cases

## Next Steps

In the next lesson, we'll learn about:
- Template literals and string interpolation
- Multi-line strings
- Tagged template literals
- String methods and formatting
- Advanced string manipulation

---

**Ready to work with strings like a pro? Let's learn about Template Literals! 🚀**

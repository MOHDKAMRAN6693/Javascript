# 14 - Local Storage

## What is Local Storage?

Local Storage is a web API that allows you to store data locally in the user's browser. The data persists even after the browser is closed and can be accessed across different tabs of the same origin.

## Local Storage vs Session Storage

### Local Storage
- Data persists until explicitly removed
- Available across browser sessions
- Shared across all tabs of the same origin
- Storage limit: ~5-10MB

### Session Storage
- Data is cleared when the tab is closed
- Only available in the current tab
- Storage limit: ~5-10MB

## Basic Local Storage Operations

### Storing Data
```javascript
// Store string data
localStorage.setItem("username", "john_doe");
localStorage.setItem("email", "john@example.com");

// Store number (converted to string)
localStorage.setItem("age", "25");

// Store boolean (converted to string)
localStorage.setItem("isLoggedIn", "true");
```

### Retrieving Data
```javascript
// Get string data
let username = localStorage.getItem("username");
console.log(username); // "john_doe"

// Get non-existent key
let nonExistent = localStorage.getItem("nonExistent");
console.log(nonExistent); // null

// Get with default value
let theme = localStorage.getItem("theme") || "light";
console.log(theme); // "light" if not set
```

### Removing Data
```javascript
// Remove specific item
localStorage.removeItem("username");

// Clear all data
localStorage.clear();
```

### Checking if Key Exists
```javascript
// Check if key exists
if (localStorage.getItem("username") !== null) {
    console.log("Username is stored");
}

// Alternative method
if ("username" in localStorage) {
    console.log("Username is stored");
}
```

## Working with Objects and Arrays

### Storing Objects
```javascript
let user = {
    name: "John Doe",
    email: "john@example.com",
    age: 25,
    preferences: {
        theme: "dark",
        language: "en"
    }
};

// Convert object to string before storing
localStorage.setItem("user", JSON.stringify(user));

// Retrieve and parse object
let storedUser = JSON.parse(localStorage.getItem("user"));
console.log(storedUser); // Object with all properties
```

### Storing Arrays
```javascript
let todos = [
    { id: 1, text: "Learn JavaScript", completed: false },
    { id: 2, text: "Build a project", completed: true },
    { id: 3, text: "Deploy to production", completed: false }
];

// Store array
localStorage.setItem("todos", JSON.stringify(todos));

// Retrieve array
let storedTodos = JSON.parse(localStorage.getItem("todos") || "[]");
console.log(storedTodos); // Array of todo objects
```

## Local Storage Class

### Basic Storage Manager
```javascript
class StorageManager {
    constructor(prefix = "") {
        this.prefix = prefix;
    }
    
    setItem(key, value) {
        try {
            const serializedValue = JSON.stringify(value);
            localStorage.setItem(this.prefix + key, serializedValue);
            return true;
        } catch (error) {
            console.error("Error storing data:", error);
            return false;
        }
    }
    
    getItem(key, defaultValue = null) {
        try {
            const item = localStorage.getItem(this.prefix + key);
            return item ? JSON.parse(item) : defaultValue;
        } catch (error) {
            console.error("Error retrieving data:", error);
            return defaultValue;
        }
    }
    
    removeItem(key) {
        localStorage.removeItem(this.prefix + key);
    }
    
    clear() {
        const keys = Object.keys(localStorage);
        keys.forEach(key => {
            if (key.startsWith(this.prefix)) {
                localStorage.removeItem(key);
            }
        });
    }
    
    getAllKeys() {
        const keys = Object.keys(localStorage);
        return keys.filter(key => key.startsWith(this.prefix));
    }
    
    getStorageSize() {
        let total = 0;
        for (let key in localStorage) {
            if (localStorage.hasOwnProperty(key)) {
                total += localStorage[key].length + key.length;
            }
        }
        return total;
    }
}

// Usage
let storage = new StorageManager("myApp_");

// Store data
storage.setItem("user", { name: "John", age: 25 });
storage.setItem("settings", { theme: "dark", language: "en" });

// Retrieve data
let user = storage.getItem("user");
let settings = storage.getItem("settings");

// Get all keys
let allKeys = storage.getAllKeys();
console.log(allKeys); // ["myApp_user", "myApp_settings"]
```

## Practical Examples

### Example 1: Todo App with Local Storage
```javascript
class TodoApp {
    constructor() {
        this.storage = new StorageManager("todoApp_");
        this.todos = this.storage.getItem("todos", []);
        this.init();
    }
    
    init() {
        this.render();
        this.bindEvents();
    }
    
    addTodo(text) {
        let todo = {
            id: Date.now(),
            text: text,
            completed: false,
            createdAt: new Date().toISOString()
        };
        
        this.todos.push(todo);
        this.saveTodos();
        this.render();
    }
    
    toggleTodo(id) {
        let todo = this.todos.find(t => t.id === id);
        if (todo) {
            todo.completed = !todo.completed;
            this.saveTodos();
            this.render();
        }
    }
    
    deleteTodo(id) {
        this.todos = this.todos.filter(t => t.id !== id);
        this.saveTodos();
        this.render();
    }
    
    saveTodos() {
        this.storage.setItem("todos", this.todos);
    }
    
    render() {
        // Render todos to DOM
        let container = document.getElementById("todoContainer");
        container.innerHTML = this.todos.map(todo => `
            <div class="todo-item ${todo.completed ? 'completed' : ''}">
                <input type="checkbox" ${todo.completed ? 'checked' : ''} 
                       onchange="todoApp.toggleTodo(${todo.id})">
                <span>${todo.text}</span>
                <button onclick="todoApp.deleteTodo(${todo.id})">Delete</button>
            </div>
        `).join("");
    }
    
    bindEvents() {
        document.getElementById("addTodoBtn").addEventListener("click", () => {
            let input = document.getElementById("todoInput");
            if (input.value.trim()) {
                this.addTodo(input.value.trim());
                input.value = "";
            }
        });
    }
}

// Initialize app
let todoApp = new TodoApp();
```

### Example 2: User Preferences
```javascript
class UserPreferences {
    constructor() {
        this.storage = new StorageManager("preferences_");
        this.defaults = {
            theme: "light",
            language: "en",
            fontSize: "medium",
            notifications: true,
            autoSave: true
        };
        this.preferences = this.storage.getItem("preferences", this.defaults);
        this.init();
    }
    
    init() {
        this.applyPreferences();
        this.bindEvents();
    }
    
    setPreference(key, value) {
        this.preferences[key] = value;
        this.storage.setItem("preferences", this.preferences);
        this.applyPreference(key, value);
    }
    
    getPreference(key) {
        return this.preferences[key] || this.defaults[key];
    }
    
    applyPreferences() {
        Object.keys(this.preferences).forEach(key => {
            this.applyPreference(key, this.preferences[key]);
        });
    }
    
    applyPreference(key, value) {
        switch (key) {
            case "theme":
                document.body.className = `theme-${value}`;
                break;
            case "fontSize":
                document.body.style.fontSize = this.getFontSize(value);
                break;
            case "language":
                document.documentElement.lang = value;
                break;
        }
    }
    
    getFontSize(size) {
        const sizes = {
            small: "14px",
            medium: "16px",
            large: "18px"
        };
        return sizes[size] || sizes.medium;
    }
    
    bindEvents() {
        // Theme toggle
        document.getElementById("themeToggle").addEventListener("click", () => {
            let currentTheme = this.getPreference("theme");
            let newTheme = currentTheme === "light" ? "dark" : "light";
            this.setPreference("theme", newTheme);
        });
        
        // Font size selector
        document.getElementById("fontSizeSelect").addEventListener("change", (event) => {
            this.setPreference("fontSize", event.target.value);
        });
    }
}

new UserPreferences();
```

### Example 3: Shopping Cart with Local Storage
```javascript
class ShoppingCart {
    constructor() {
        this.storage = new StorageManager("cart_");
        this.items = this.storage.getItem("items", []);
        this.init();
    }
    
    init() {
        this.render();
        this.bindEvents();
    }
    
    addItem(product) {
        let existingItem = this.items.find(item => item.id === product.id);
        
        if (existingItem) {
            existingItem.quantity += 1;
        } else {
            this.items.push({
                id: product.id,
                name: product.name,
                price: product.price,
                quantity: 1,
                image: product.image
            });
        }
        
        this.saveCart();
        this.render();
    }
    
    removeItem(productId) {
        this.items = this.items.filter(item => item.id !== productId);
        this.saveCart();
        this.render();
    }
    
    updateQuantity(productId, quantity) {
        let item = this.items.find(item => item.id === productId);
        if (item) {
            if (quantity <= 0) {
                this.removeItem(productId);
            } else {
                item.quantity = quantity;
                this.saveCart();
                this.render();
            }
        }
    }
    
    getTotal() {
        return this.items.reduce((total, item) => total + (item.price * item.quantity), 0);
    }
    
    getItemCount() {
        return this.items.reduce((count, item) => count + item.quantity, 0);
    }
    
    clearCart() {
        this.items = [];
        this.saveCart();
        this.render();
    }
    
    saveCart() {
        this.storage.setItem("items", this.items);
    }
    
    render() {
        let container = document.getElementById("cartContainer");
        container.innerHTML = `
            <h2>Shopping Cart (${this.getItemCount()} items)</h2>
            <div class="cart-items">
                ${this.items.map(item => `
                    <div class="cart-item">
                        <img src="${item.image}" alt="${item.name}">
                        <div class="item-details">
                            <h3>${item.name}</h3>
                            <p>$${item.price}</p>
                            <div class="quantity-controls">
                                <button onclick="cart.updateQuantity(${item.id}, ${item.quantity - 1})">-</button>
                                <span>${item.quantity}</span>
                                <button onclick="cart.updateQuantity(${item.id}, ${item.quantity + 1})">+</button>
                            </div>
                        </div>
                        <button onclick="cart.removeItem(${item.id})">Remove</button>
                    </div>
                `).join("")}
            </div>
            <div class="cart-total">
                <h3>Total: $${this.getTotal().toFixed(2)}</h3>
                <button onclick="cart.clearCart()">Clear Cart</button>
            </div>
        `;
    }
    
    bindEvents() {
        // Add to cart buttons
        document.querySelectorAll(".add-to-cart").forEach(button => {
            button.addEventListener("click", (event) => {
                let product = {
                    id: event.target.dataset.productId,
                    name: event.target.dataset.productName,
                    price: parseFloat(event.target.dataset.productPrice),
                    image: event.target.dataset.productImage
                };
                this.addItem(product);
            });
        });
    }
}

let cart = new ShoppingCart();
```

## Storage Limits and Best Practices

### Checking Storage Availability
```javascript
function isStorageAvailable() {
    try {
        let test = "test";
        localStorage.setItem(test, test);
        localStorage.removeItem(test);
        return true;
    } catch (error) {
        return false;
    }
}

if (isStorageAvailable()) {
    console.log("Local storage is available");
} else {
    console.log("Local storage is not available");
}
```

### Storage Quota Management
```javascript
class StorageQuotaManager {
    constructor() {
        this.maxSize = 5 * 1024 * 1024; // 5MB
        this.currentSize = 0;
        this.init();
    }
    
    init() {
        this.currentSize = this.getStorageSize();
    }
    
    getStorageSize() {
        let total = 0;
        for (let key in localStorage) {
            if (localStorage.hasOwnProperty(key)) {
                total += localStorage[key].length + key.length;
            }
        }
        return total;
    }
    
    canStore(data) {
        let dataSize = JSON.stringify(data).length;
        return (this.currentSize + dataSize) <= this.maxSize;
    }
    
    storeData(key, data) {
        if (this.canStore(data)) {
            localStorage.setItem(key, JSON.stringify(data));
            this.currentSize = this.getStorageSize();
            return true;
        } else {
            console.warn("Storage quota exceeded");
            return false;
        }
    }
    
    cleanup() {
        // Remove oldest data if quota exceeded
        let keys = Object.keys(localStorage);
        keys.sort((a, b) => {
            return localStorage.getItem(a).timestamp - localStorage.getItem(b).timestamp;
        });
        
        while (this.currentSize > this.maxSize && keys.length > 0) {
            let oldestKey = keys.shift();
            localStorage.removeItem(oldestKey);
            this.currentSize = this.getStorageSize();
        }
    }
}
```

## Practice Exercises

### Exercise 1: Settings Manager
Create a settings manager that stores user preferences:
- Theme (light/dark)
- Language
- Font size
- Notification preferences

### Exercise 2: Bookmark Manager
Create a bookmark manager that stores:
- Website URLs
- Titles
- Categories
- Tags
- Timestamps

### Exercise 3: Note Taking App
Create a note-taking app with local storage:
- Create, edit, delete notes
- Search functionality
- Categories and tags
- Auto-save

### Exercise 4: Game Progress Tracker
Create a game progress tracker that stores:
- Player stats
- Achievements
- Level progress
- Settings

### Exercise 5: Expense Tracker
Create an expense tracker that stores:
- Income and expenses
- Categories
- Dates
- Descriptions

## Common Mistakes

### 1. Not Handling Storage Errors
```javascript
// Wrong - can cause errors
localStorage.setItem("data", largeObject);

// Correct - handle errors
try {
    localStorage.setItem("data", JSON.stringify(largeObject));
} catch (error) {
    console.error("Storage error:", error);
}
```

### 2. Not Parsing JSON Data
```javascript
// Wrong - returns string
let data = localStorage.getItem("user");
console.log(data.name); // undefined

// Correct - parse JSON
let data = JSON.parse(localStorage.getItem("user"));
console.log(data.name); // works
```

### 3. Not Checking if Data Exists
```javascript
// Wrong - can cause errors
let data = JSON.parse(localStorage.getItem("data"));

// Correct - check if data exists
let data = JSON.parse(localStorage.getItem("data") || "{}");
```

## Key Takeaways

- Local Storage persists data across browser sessions
- Always handle storage errors and quota limits
- Use JSON.stringify() and JSON.parse() for complex data
- Implement proper error handling and fallbacks
- Consider storage limits and cleanup strategies
- Test with different browsers and storage scenarios

## Next Steps

In the next lesson, we'll learn about:
- Fetch API for making HTTP requests
- Handling responses and errors
- Working with different data formats
- Authentication and headers
- Modern alternatives to XMLHttpRequest

---

**Ready to fetch data from APIs? Let's learn about the Fetch API! 🚀**

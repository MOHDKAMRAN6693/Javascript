# 12 - Events

## What are Events?

Events are actions or occurrences that happen in the browser, such as clicks, key presses, mouse movements, or page loads. JavaScript allows you to respond to these events by executing code when they occur.

## Event Types

### Mouse Events
```javascript
let button = document.getElementById("myButton");

// Click event
button.addEventListener("click", function(event) {
    console.log("Button clicked!");
});

// Double click
button.addEventListener("dblclick", function(event) {
    console.log("Button double-clicked!");
});

// Mouse enter
button.addEventListener("mouseenter", function(event) {
    console.log("Mouse entered button");
});

// Mouse leave
button.addEventListener("mouseleave", function(event) {
    console.log("Mouse left button");
});

// Mouse over
button.addEventListener("mouseover", function(event) {
    console.log("Mouse over button");
});

// Mouse move
button.addEventListener("mousemove", function(event) {
    console.log("Mouse moved on button");
});
```

### Keyboard Events
```javascript
let input = document.getElementById("myInput");

// Key down
input.addEventListener("keydown", function(event) {
    console.log("Key pressed:", event.key);
});

// Key up
input.addEventListener("keyup", function(event) {
    console.log("Key released:", event.key);
});

// Key press
input.addEventListener("keypress", function(event) {
    console.log("Key pressed:", event.key);
});
```

### Form Events
```javascript
let form = document.getElementById("myForm");

// Form submit
form.addEventListener("submit", function(event) {
    event.preventDefault(); // Prevent default form submission
    console.log("Form submitted!");
});

// Input change
let input = document.getElementById("myInput");
input.addEventListener("change", function(event) {
    console.log("Input value changed:", event.target.value);
});

// Input focus
input.addEventListener("focus", function(event) {
    console.log("Input focused");
});

// Input blur
input.addEventListener("blur", function(event) {
    console.log("Input lost focus");
});
```

### Window Events
```javascript
// Page load
window.addEventListener("load", function(event) {
    console.log("Page fully loaded");
});

// DOM content loaded
document.addEventListener("DOMContentLoaded", function(event) {
    console.log("DOM content loaded");
});

// Window resize
window.addEventListener("resize", function(event) {
    console.log("Window resized");
});

// Window scroll
window.addEventListener("scroll", function(event) {
    console.log("Page scrolled");
});
```

## Event Object

The event object contains information about the event that occurred.

### Basic Event Properties
```javascript
let button = document.getElementById("myButton");

button.addEventListener("click", function(event) {
    console.log("Event type:", event.type); // "click"
    console.log("Target:", event.target); // The element that triggered the event
    console.log("Current target:", event.currentTarget); // The element the listener is attached to
    console.log("Timestamp:", event.timeStamp); // When the event occurred
});
```

### Mouse Event Properties
```javascript
let div = document.getElementById("myDiv");

div.addEventListener("click", function(event) {
    console.log("Client X:", event.clientX); // X coordinate relative to viewport
    console.log("Client Y:", event.clientY); // Y coordinate relative to viewport
    console.log("Page X:", event.pageX); // X coordinate relative to document
    console.log("Page Y:", event.pageY); // Y coordinate relative to document
    console.log("Button:", event.button); // Which mouse button was pressed
    console.log("Buttons:", event.buttons); // Which mouse buttons are pressed
});
```

### Keyboard Event Properties
```javascript
let input = document.getElementById("myInput");

input.addEventListener("keydown", function(event) {
    console.log("Key:", event.key); // The key that was pressed
    console.log("Code:", event.code); // Physical key code
    console.log("Key code:", event.keyCode); // Numeric key code (deprecated)
    console.log("Alt key:", event.altKey); // Alt key pressed
    console.log("Ctrl key:", event.ctrlKey); // Ctrl key pressed
    console.log("Shift key:", event.shiftKey); // Shift key pressed
    console.log("Meta key:", event.metaKey); // Meta key pressed (Cmd on Mac)
});
```

## Event Bubbling and Capturing

### Event Bubbling
Events bubble up from the target element to the root of the document.

```javascript
let grandparent = document.getElementById("grandparent");
let parent = document.getElementById("parent");
let child = document.getElementById("child");

// Event bubbling (default)
grandparent.addEventListener("click", function(event) {
    console.log("Grandparent clicked");
});

parent.addEventListener("click", function(event) {
    console.log("Parent clicked");
});

child.addEventListener("click", function(event) {
    console.log("Child clicked");
});

// Clicking child will log: "Child clicked", "Parent clicked", "Grandparent clicked"
```

### Event Capturing
Events are captured from the root down to the target element.

```javascript
let grandparent = document.getElementById("grandparent");
let parent = document.getElementById("parent");
let child = document.getElementById("child");

// Event capturing
grandparent.addEventListener("click", function(event) {
    console.log("Grandparent captured");
}, true); // true enables capturing

parent.addEventListener("click", function(event) {
    console.log("Parent captured");
}, true);

child.addEventListener("click", function(event) {
    console.log("Child captured");
}, true);

// Clicking child will log: "Grandparent captured", "Parent captured", "Child captured"
```

### Stopping Event Propagation
```javascript
let parent = document.getElementById("parent");
let child = document.getElementById("child");

parent.addEventListener("click", function(event) {
    console.log("Parent clicked");
});

child.addEventListener("click", function(event) {
    console.log("Child clicked");
    event.stopPropagation(); // Stop event from bubbling up
});

// Clicking child will only log: "Child clicked"
```

## Event Delegation

Event delegation allows you to attach a single event listener to a parent element to handle events for multiple child elements.

### Basic Event Delegation
```javascript
let list = document.getElementById("myList");

// Instead of adding listeners to each list item
list.addEventListener("click", function(event) {
    if (event.target.tagName === "LI") {
        console.log("List item clicked:", event.target.textContent);
    }
});

// Add new list items dynamically
function addListItem(text) {
    let li = document.createElement("li");
    li.textContent = text;
    list.appendChild(li);
    // No need to add event listener - delegation handles it
}
```

### Advanced Event Delegation
```javascript
let container = document.getElementById("container");

container.addEventListener("click", function(event) {
    let target = event.target;
    
    // Handle button clicks
    if (target.classList.contains("btn")) {
        console.log("Button clicked:", target.textContent);
    }
    
    // Handle link clicks
    if (target.tagName === "A") {
        event.preventDefault();
        console.log("Link clicked:", target.href);
    }
    
    // Handle specific elements
    if (target.dataset.action) {
        console.log("Action triggered:", target.dataset.action);
    }
});
```

## Custom Events

### Creating Custom Events
```javascript
// Create custom event
let customEvent = new CustomEvent("myCustomEvent", {
    detail: { message: "Hello from custom event!" }
});

// Listen for custom event
document.addEventListener("myCustomEvent", function(event) {
    console.log("Custom event received:", event.detail.message);
});

// Dispatch custom event
document.dispatchEvent(customEvent);
```

### Custom Events with Data
```javascript
// Create custom event with data
let dataEvent = new CustomEvent("userAction", {
    detail: {
        action: "buttonClick",
        timestamp: Date.now(),
        userId: 123
    }
});

// Listen for custom event
document.addEventListener("userAction", function(event) {
    console.log("User action:", event.detail.action);
    console.log("Timestamp:", event.detail.timestamp);
    console.log("User ID:", event.detail.userId);
});

// Dispatch custom event
document.dispatchEvent(dataEvent);
```

## Practical Examples

### Example 1: Interactive Counter
```javascript
class Counter {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.count = 0;
        this.init();
    }
    
    init() {
        this.container.innerHTML = `
            <div class="counter">
                <h2>Counter: <span id="count">0</span></h2>
                <button id="increment">+</button>
                <button id="decrement">-</button>
                <button id="reset">Reset</button>
            </div>
        `;
        
        this.bindEvents();
    }
    
    bindEvents() {
        this.container.addEventListener("click", (event) => {
            const target = event.target;
            
            if (target.id === "increment") {
                this.increment();
            } else if (target.id === "decrement") {
                this.decrement();
            } else if (target.id === "reset") {
                this.reset();
            }
        });
    }
    
    increment() {
        this.count++;
        this.updateDisplay();
    }
    
    decrement() {
        this.count--;
        this.updateDisplay();
    }
    
    reset() {
        this.count = 0;
        this.updateDisplay();
    }
    
    updateDisplay() {
        document.getElementById("count").textContent = this.count;
    }
}

// Initialize counter
new Counter("counterContainer");
```

### Example 2: Form Validation
```javascript
class FormValidator {
    constructor(formId) {
        this.form = document.getElementById(formId);
        this.init();
    }
    
    init() {
        this.form.addEventListener("submit", (event) => {
            event.preventDefault();
            this.validateForm();
        });
        
        // Real-time validation
        this.form.addEventListener("input", (event) => {
            this.validateField(event.target);
        });
    }
    
    validateForm() {
        let isValid = true;
        const fields = this.form.querySelectorAll("input[required]");
        
        fields.forEach(field => {
            if (!this.validateField(field)) {
                isValid = false;
            }
        });
        
        if (isValid) {
            this.submitForm();
        }
    }
    
    validateField(field) {
        const value = field.value.trim();
        const fieldName = field.name;
        let isValid = true;
        let errorMessage = "";
        
        // Required field validation
        if (field.hasAttribute("required") && value === "") {
            isValid = false;
            errorMessage = `${fieldName} is required`;
        }
        
        // Email validation
        if (field.type === "email" && value !== "") {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(value)) {
                isValid = false;
                errorMessage = "Please enter a valid email address";
            }
        }
        
        // Password validation
        if (field.type === "password" && value !== "") {
            if (value.length < 8) {
                isValid = false;
                errorMessage = "Password must be at least 8 characters long";
            }
        }
        
        this.showFieldError(field, isValid, errorMessage);
        return isValid;
    }
    
    showFieldError(field, isValid, message) {
        const errorElement = field.parentNode.querySelector(".error-message");
        
        if (isValid) {
            field.classList.remove("error");
            if (errorElement) {
                errorElement.remove();
            }
        } else {
            field.classList.add("error");
            if (!errorElement) {
                const errorDiv = document.createElement("div");
                errorDiv.className = "error-message";
                errorDiv.textContent = message;
                field.parentNode.appendChild(errorDiv);
            } else {
                errorElement.textContent = message;
            }
        }
    }
    
    submitForm() {
        console.log("Form is valid, submitting...");
        // Handle form submission
    }
}

// Initialize form validator
new FormValidator("myForm");
```

### Example 3: Drag and Drop
```javascript
class DragAndDrop {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.draggedElement = null;
        this.init();
    }
    
    init() {
        this.container.addEventListener("dragstart", (event) => {
            this.draggedElement = event.target;
            event.target.style.opacity = "0.5";
        });
        
        this.container.addEventListener("dragend", (event) => {
            event.target.style.opacity = "1";
            this.draggedElement = null;
        });
        
        this.container.addEventListener("dragover", (event) => {
            event.preventDefault();
        });
        
        this.container.addEventListener("drop", (event) => {
            event.preventDefault();
            if (this.draggedElement) {
                event.target.appendChild(this.draggedElement);
            }
        });
    }
}

// Initialize drag and drop
new DragAndDrop("dragContainer");
```

## Practice Exercises

### Exercise 1: Event Counter
Create a page that counts different types of events:
```javascript
class EventCounter {
    constructor() {
        this.counts = {
            clicks: 0,
            keypresses: 0,
            mousemoves: 0
        };
        this.init();
    }
    
    init() {
        // Add event listeners and display counts
    }
}
```

### Exercise 2: Keyboard Shortcuts
Create a system that responds to keyboard shortcuts:
```javascript
class KeyboardShortcuts {
    constructor() {
        this.shortcuts = {
            "ctrl+s": () => this.save(),
            "ctrl+z": () => this.undo(),
            "ctrl+y": () => this.redo()
        };
        this.init();
    }
    
    init() {
        // Add keyboard event listeners
    }
}
```

### Exercise 3: Event Delegation List
Create a dynamic list with event delegation:
```javascript
class DynamicList {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.items = [];
        this.init();
    }
    
    init() {
        // Set up event delegation for add, remove, edit
    }
}
```

### Exercise 4: Custom Event System
Create a custom event system for a game:
```javascript
class GameEventSystem {
    constructor() {
        this.listeners = {};
        this.init();
    }
    
    init() {
        // Set up custom events for game actions
    }
}
```

### Exercise 5: Form Auto-save
Create a form that auto-saves as the user types:
```javascript
class AutoSaveForm {
    constructor(formId) {
        this.form = document.getElementById(formId);
        this.saveInterval = null;
        this.init();
    }
    
    init() {
        // Set up auto-save functionality
    }
}
```

## Common Mistakes

### 1. Not Removing Event Listeners
```javascript
// Wrong - can cause memory leaks
function addButton() {
    let button = document.createElement("button");
    button.addEventListener("click", function() {
        console.log("Clicked");
    });
    document.body.appendChild(button);
}

// Better - remove event listeners when done
function addButton() {
    let button = document.createElement("button");
    let handler = function() {
        console.log("Clicked");
    };
    button.addEventListener("click", handler);
    document.body.appendChild(button);
    
    // Remove when done
    setTimeout(() => {
        button.removeEventListener("click", handler);
        button.remove();
    }, 5000);
}
```

### 2. Not Preventing Default Behavior
```javascript
// Wrong - form will submit and page will refresh
form.addEventListener("submit", function(event) {
    console.log("Form submitted");
});

// Correct - prevent default form submission
form.addEventListener("submit", function(event) {
    event.preventDefault();
    console.log("Form submitted");
});
```

### 3. Not Using Event Delegation
```javascript
// Wrong - adding listeners to each element
let buttons = document.querySelectorAll(".btn");
buttons.forEach(button => {
    button.addEventListener("click", function() {
        console.log("Button clicked");
    });
});

// Better - use event delegation
document.addEventListener("click", function(event) {
    if (event.target.classList.contains("btn")) {
        console.log("Button clicked");
    }
});
```

## Key Takeaways

- Events are user interactions that can be handled with JavaScript
- Use `addEventListener()` to attach event handlers
- The event object contains information about the event
- Event bubbling and capturing control event flow
- Event delegation is efficient for dynamic content
- Custom events allow for complex interactions
- Always clean up event listeners to prevent memory leaks
- Practice with different event types and patterns

## Next Steps

In the next lesson, we'll learn about:
- Form handling and validation
- Input types and their properties
- Form submission and data collection
- Client-side validation techniques
- Form security and best practices

---

**Ready to handle forms like a pro? Let's learn about Forms and Validation! 🚀**

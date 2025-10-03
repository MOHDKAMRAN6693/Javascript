# 11 - DOM Manipulation

## What is the DOM?

The Document Object Model (DOM) is a programming interface that represents HTML documents as a tree structure. It allows JavaScript to interact with and modify HTML elements, attributes, and content.

## DOM Tree Structure

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>This is a paragraph</p>
    <div id="container">
        <button>Click me</button>
    </div>
</body>
</html>
```

## Selecting Elements

### getElementById
```javascript
let element = document.getElementById("container");
console.log(element); // <div id="container">...</div>
```

### getElementsByClassName
```javascript
let elements = document.getElementsByClassName("my-class");
console.log(elements); // HTMLCollection of elements
```

### getElementsByTagName
```javascript
let paragraphs = document.getElementsByTagName("p");
console.log(paragraphs); // HTMLCollection of <p> elements
```

### querySelector (Modern)
```javascript
// Select first element matching CSS selector
let element = document.querySelector("#container");
let paragraph = document.querySelector("p");
let button = document.querySelector("button");

// Select all elements matching CSS selector
let allParagraphs = document.querySelectorAll("p");
let allButtons = document.querySelectorAll("button");
```

### querySelectorAll
```javascript
// Select multiple elements
let elements = document.querySelectorAll(".my-class");
let paragraphs = document.querySelectorAll("p");
let buttons = document.querySelectorAll("button");
```

## Creating Elements

### createElement
```javascript
let newDiv = document.createElement("div");
let newParagraph = document.createElement("p");
let newButton = document.createElement("button");
```

### createTextNode
```javascript
let textNode = document.createTextNode("Hello World");
```

## Adding Elements to DOM

### appendChild
```javascript
let container = document.getElementById("container");
let newDiv = document.createElement("div");
newDiv.textContent = "New div";
container.appendChild(newDiv);
```

### insertBefore
```javascript
let container = document.getElementById("container");
let newDiv = document.createElement("div");
let existingElement = container.firstChild;
container.insertBefore(newDiv, existingElement);
```

### prepend (Modern)
```javascript
let container = document.getElementById("container");
let newDiv = document.createElement("div");
container.prepend(newDiv); // Add as first child
```

### append (Modern)
```javascript
let container = document.getElementById("container");
let newDiv = document.createElement("div");
container.append(newDiv); // Add as last child
```

## Removing Elements

### removeChild
```javascript
let container = document.getElementById("container");
let elementToRemove = container.firstChild;
container.removeChild(elementToRemove);
```

### remove (Modern)
```javascript
let element = document.getElementById("myElement");
element.remove(); // Remove the element
```

## Modifying Element Content

### textContent
```javascript
let element = document.getElementById("myElement");
element.textContent = "New text content";
console.log(element.textContent); // "New text content"
```

### innerHTML
```javascript
let element = document.getElementById("myElement");
element.innerHTML = "<strong>Bold text</strong>";
console.log(element.innerHTML); // "<strong>Bold text</strong>"
```

### innerText
```javascript
let element = document.getElementById("myElement");
element.innerText = "New text";
console.log(element.innerText); // "New text"
```

## Working with Attributes

### getAttribute
```javascript
let element = document.getElementById("myElement");
let id = element.getAttribute("id");
let className = element.getAttribute("class");
```

### setAttribute
```javascript
let element = document.getElementById("myElement");
element.setAttribute("class", "new-class");
element.setAttribute("data-value", "123");
```

### removeAttribute
```javascript
let element = document.getElementById("myElement");
element.removeAttribute("class");
```

### hasAttribute
```javascript
let element = document.getElementById("myElement");
if (element.hasAttribute("class")) {
    console.log("Element has class attribute");
}
```

## Working with Classes

### classList
```javascript
let element = document.getElementById("myElement");

// Add class
element.classList.add("new-class");

// Remove class
element.classList.remove("old-class");

// Toggle class
element.classList.toggle("active");

// Check if class exists
if (element.classList.contains("active")) {
    console.log("Element has active class");
}

// Replace class
element.classList.replace("old-class", "new-class");
```

## Working with Styles

### style Property
```javascript
let element = document.getElementById("myElement");

// Set styles
element.style.color = "red";
element.style.backgroundColor = "blue";
element.style.fontSize = "16px";
element.style.display = "none";

// Get styles
console.log(element.style.color); // "red"
```

### getComputedStyle
```javascript
let element = document.getElementById("myElement");
let styles = getComputedStyle(element);
console.log(styles.color); // Actual computed color
console.log(styles.fontSize); // Actual computed font size
```

## Practical Examples

### Example 1: Dynamic List
```javascript
function addListItem(text) {
    let list = document.getElementById("myList");
    let listItem = document.createElement("li");
    listItem.textContent = text;
    list.appendChild(listItem);
}

// Usage
addListItem("New item 1");
addListItem("New item 2");
```

### Example 2: Toggle Visibility
```javascript
function toggleVisibility(elementId) {
    let element = document.getElementById(elementId);
    if (element.style.display === "none") {
        element.style.display = "block";
    } else {
        element.style.display = "none";
    }
}

// Usage
toggleVisibility("myElement");
```

### Example 3: Form Validation
```javascript
function validateForm() {
    let nameInput = document.getElementById("name");
    let emailInput = document.getElementById("email");
    
    if (nameInput.value.trim() === "") {
        nameInput.style.borderColor = "red";
        return false;
    }
    
    if (!emailInput.value.includes("@")) {
        emailInput.style.borderColor = "red";
        return false;
    }
    
    return true;
}
```

### Example 4: Dynamic Table
```javascript
function createTable(data) {
    let table = document.createElement("table");
    let headerRow = document.createElement("tr");
    
    // Create header
    for (let key in data[0]) {
        let th = document.createElement("th");
        th.textContent = key;
        headerRow.appendChild(th);
    }
    table.appendChild(headerRow);
    
    // Create data rows
    data.forEach(item => {
        let row = document.createElement("tr");
        for (let key in item) {
            let td = document.createElement("td");
            td.textContent = item[key];
            row.appendChild(td);
        }
        table.appendChild(row);
    });
    
    return table;
}

// Usage
let data = [
    { name: "John", age: 25, city: "New York" },
    { name: "Jane", age: 30, city: "Los Angeles" }
];

let table = createTable(data);
document.body.appendChild(table);
```

## Event Handling

### addEventListener
```javascript
let button = document.getElementById("myButton");
button.addEventListener("click", function() {
    console.log("Button clicked!");
});
```

### removeEventListener
```javascript
function handleClick() {
    console.log("Button clicked!");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## Common DOM Methods

### parentNode
```javascript
let element = document.getElementById("myElement");
let parent = element.parentNode;
```

### childNodes
```javascript
let container = document.getElementById("container");
let children = container.childNodes;
```

### nextSibling / previousSibling
```javascript
let element = document.getElementById("myElement");
let next = element.nextSibling;
let previous = element.previousSibling;
```

### firstChild / lastChild
```javascript
let container = document.getElementById("container");
let first = container.firstChild;
let last = container.lastChild;
```

## Practice Exercises

### Exercise 1: Todo List
Create a todo list with:
- Input field for new todos
- Add button to add todos
- Delete button for each todo
- Mark as complete functionality

### Exercise 2: Image Gallery
Create an image gallery with:
- Thumbnail images
- Click to view full size
- Previous/Next navigation
- Close button

### Exercise 3: Form Builder
Create a dynamic form builder that allows:
- Adding different input types
- Removing fields
- Validating form data
- Submitting form

### Exercise 4: Calculator
Create a calculator with:
- Number buttons (0-9)
- Operation buttons (+, -, *, /)
- Equals button
- Clear button
- Display screen

### Exercise 5: Accordion
Create an accordion component with:
- Multiple sections
- Click to expand/collapse
- Only one section open at a time
- Smooth animations

## Common Mistakes

### 1. Not waiting for DOM to load
```javascript
// Wrong - DOM might not be ready
let element = document.getElementById("myElement");

// Correct - wait for DOM to load
document.addEventListener("DOMContentLoaded", function() {
    let element = document.getElementById("myElement");
});
```

### 2. Using innerHTML with user input
```javascript
// Wrong - XSS vulnerability
let userInput = "<script>alert('XSS')</script>";
element.innerHTML = userInput;

// Correct - use textContent
element.textContent = userInput;
```

### 3. Not checking if element exists
```javascript
// Wrong - might cause error
let element = document.getElementById("myElement");
element.style.color = "red";

// Correct - check if element exists
let element = document.getElementById("myElement");
if (element) {
    element.style.color = "red";
}
```

### 4. Memory leaks with event listeners
```javascript
// Wrong - not removing event listeners
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

## Key Takeaways

- DOM allows JavaScript to interact with HTML elements
- Use modern methods like querySelector and querySelectorAll
- Be careful with innerHTML to avoid XSS attacks
- Always check if elements exist before manipulating them
- Use event listeners for user interactions
- Clean up event listeners to prevent memory leaks
- Practice with real-world examples

## Next Steps

In the next lesson, we'll learn about:
- Event types and event objects
- Event bubbling and capturing
- Event delegation
- Custom events
- Form events and validation

---

**Ready to handle user interactions? Let's learn about Events! 🚀**

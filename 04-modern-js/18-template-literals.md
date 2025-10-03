# 18 - Template Literals

## What are Template Literals?

Template literals are string literals that allow embedded expressions and multi-line strings. They are enclosed in backticks (`) instead of single or double quotes, and support string interpolation and advanced formatting.

## Basic Template Literals

### String Interpolation
```javascript
let name = "John";
let age = 25;

// Traditional string concatenation
let message1 = "Hello, " + name + "! You are " + age + " years old.";

// Template literal
let message2 = `Hello, ${name}! You are ${age} years old.`;

console.log(message1); // "Hello, John! You are 25 years old."
console.log(message2); // "Hello, John! You are 25 years old."
```

### Multi-line Strings
```javascript
// Traditional multi-line string
let traditionalString = "This is line 1\n" +
                       "This is line 2\n" +
                       "This is line 3";

// Template literal
let templateString = `This is line 1
This is line 2
This is line 3`;

console.log(traditionalString);
console.log(templateString);
```

### Expressions in Template Literals
```javascript
let a = 10;
let b = 20;

// Mathematical expressions
let result = `The sum of ${a} and ${b} is ${a + b}`;
console.log(result); // "The sum of 10 and 20 is 30"

// Function calls
function getCurrentTime() {
    return new Date().toLocaleTimeString();
}

let timeMessage = `Current time is ${getCurrentTime()}`;
console.log(timeMessage); // "Current time is 2:30:45 PM"

// Object properties
let person = { name: "John", age: 25 };
let personInfo = `Name: ${person.name}, Age: ${person.age}`;
console.log(personInfo); // "Name: John, Age: 25"
```

## Advanced Template Literals

### Nested Template Literals
```javascript
let users = [
    { name: "John", age: 25, city: "NYC" },
    { name: "Jane", age: 30, city: "LA" }
];

let userList = `
    <ul>
        ${users.map(user => `
            <li>
                <strong>${user.name}</strong> (${user.age} years old)
                <br>Location: ${user.city}
            </li>
        `).join("")}
    </ul>
`;

console.log(userList);
```

### Conditional Rendering
```javascript
function createUserCard(user) {
    return `
        <div class="user-card">
            <h2>${user.name}</h2>
            <p>Age: ${user.age}</p>
            ${user.email ? `<p>Email: ${user.email}</p>` : ""}
            ${user.isAdmin ? `<span class="admin-badge">Admin</span>` : ""}
            ${user.hobbies && user.hobbies.length > 0 ? `
                <div class="hobbies">
                    <h3>Hobbies:</h3>
                    <ul>
                        ${user.hobbies.map(hobby => `<li>${hobby}</li>`).join("")}
                    </ul>
                </div>
            ` : ""}
        </div>
    `;
}

let user = {
    name: "John",
    age: 25,
    email: "john@example.com",
    isAdmin: true,
    hobbies: ["reading", "coding", "gaming"]
};

console.log(createUserCard(user));
```

### Template Literals with Functions
```javascript
function formatCurrency(amount, currency = "USD") {
    return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: currency
    }).format(amount);
}

function createInvoice(item, quantity, price) {
    let total = quantity * price;
    
    return `
        Invoice
        =======
        Item: ${item}
        Quantity: ${quantity}
        Price: ${formatCurrency(price)}
        Total: ${formatCurrency(total)}
        =======
    `;
}

console.log(createInvoice("Laptop", 2, 999.99));
```

## Tagged Template Literals

### Basic Tagged Template
```javascript
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        return result + string + (values[i] ? `<mark>${values[i]}</mark>` : "");
    }, "");
}

let name = "John";
let age = 25;

let highlighted = highlight`Hello ${name}, you are ${age} years old.`;
console.log(highlighted); // "Hello <mark>John</mark>, you are <mark>25</mark> years old."
```

### Advanced Tagged Template
```javascript
function sql(strings, ...values) {
    let query = strings[0];
    for (let i = 0; i < values.length; i++) {
        query += `'${values[i]}'` + strings[i + 1];
    }
    return query;
}

function html(strings, ...values) {
    return strings.reduce((result, string, i) => {
        return result + string + (values[i] ? escapeHtml(values[i]) : "");
    }, "");
}

function escapeHtml(text) {
    const div = document.createElement("div");
    div.textContent = text;
    return div.innerHTML;
}

// Usage
let tableName = "users";
let condition = "age > 18";

let query = sql`SELECT * FROM ${tableName} WHERE ${condition}`;
console.log(query); // "SELECT * FROM 'users' WHERE 'age > 18'"

let userInput = "<script>alert('XSS')</script>";
let safeHtml = html`<div>${userInput}</div>`;
console.log(safeHtml); // "<div>&lt;script&gt;alert('XSS')&lt;/script&gt;</div>"
```

### Styled Components with Tagged Templates
```javascript
function styled(tag) {
    return function(strings, ...values) {
        let styles = strings[0];
        for (let i = 0; i < values.length; i++) {
            styles += values[i] + strings[i + 1];
        }
        
        return function(props) {
            let element = document.createElement(tag);
            element.style.cssText = styles;
            return element;
        };
    };
}

const Button = styled("button")`
    background-color: ${props => props.primary ? "#007bff" : "#6c757d"};
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
`;

// Usage
let button = Button({ primary: true });
button.textContent = "Click me";
document.body.appendChild(button);
```

## Practical Examples

### Example 1: HTML Template Generator
```javascript
class HTMLGenerator {
    static createCard({ title, content, image, author, date }) {
        return `
            <div class="card">
                ${image ? `<img src="${image}" alt="${title}" class="card-image">` : ""}
                <div class="card-content">
                    <h2 class="card-title">${title}</h2>
                    <p class="card-text">${content}</p>
                    <div class="card-meta">
                        <span class="author">By ${author}</span>
                        <span class="date">${new Date(date).toLocaleDateString()}</span>
                    </div>
                </div>
            </div>
        `;
    }
    
    static createTable({ headers, rows, caption }) {
        return `
            <table class="data-table">
                ${caption ? `<caption>${caption}</caption>` : ""}
                <thead>
                    <tr>
                        ${headers.map(header => `<th>${header}</th>`).join("")}
                    </tr>
                </thead>
                <tbody>
                    ${rows.map(row => `
                        <tr>
                            ${row.map(cell => `<td>${cell}</td>`).join("")}
                        </tr>
                    `).join("")}
                </tbody>
            </table>
        `;
    }
}

// Usage
let card = HTMLGenerator.createCard({
    title: "JavaScript Tips",
    content: "Learn advanced JavaScript techniques",
    author: "John Doe",
    date: "2023-01-15"
});

let table = HTMLGenerator.createTable({
    headers: ["Name", "Age", "City"],
    rows: [
        ["John", "25", "NYC"],
        ["Jane", "30", "LA"]
    ],
    caption: "User Information"
});
```

### Example 2: Email Template Generator
```javascript
class EmailTemplate {
    static welcome({ name, email, activationLink }) {
        return `
            <!DOCTYPE html>
            <html>
            <head>
                <style>
                    body { font-family: Arial, sans-serif; }
                    .container { max-width: 600px; margin: 0 auto; }
                    .header { background-color: #007bff; color: white; padding: 20px; }
                    .content { padding: 20px; }
                    .button { background-color: #28a745; color: white; padding: 10px 20px; text-decoration: none; border-radius: 4px; }
                </style>
            </head>
            <body>
                <div class="container">
                    <div class="header">
                        <h1>Welcome to Our Platform!</h1>
                    </div>
                    <div class="content">
                        <p>Hello ${name},</p>
                        <p>Thank you for signing up with email ${email}.</p>
                        <p>Please click the button below to activate your account:</p>
                        <a href="${activationLink}" class="button">Activate Account</a>
                        <p>If the button doesn't work, copy and paste this link:</p>
                        <p>${activationLink}</p>
                    </div>
                </div>
            </body>
            </html>
        `;
    }
    
    static notification({ title, message, type = "info", actionUrl, actionText }) {
        let color = {
            info: "#17a2b8",
            success: "#28a745",
            warning: "#ffc107",
            error: "#dc3545"
        }[type];
        
        return `
            <div style="border-left: 4px solid ${color}; padding: 20px; margin: 20px 0; background-color: #f8f9fa;">
                <h3 style="margin-top: 0; color: ${color};">${title}</h3>
                <p>${message}</p>
                ${actionUrl && actionText ? `
                    <a href="${actionUrl}" style="background-color: ${color}; color: white; padding: 8px 16px; text-decoration: none; border-radius: 4px;">
                        ${actionText}
                    </a>
                ` : ""}
            </div>
        `;
    }
}

// Usage
let welcomeEmail = EmailTemplate.welcome({
    name: "John Doe",
    email: "john@example.com",
    activationLink: "https://example.com/activate?token=abc123"
});

let notification = EmailTemplate.notification({
    title: "New Message",
    message: "You have received a new message from Jane.",
    type: "info",
    actionUrl: "https://example.com/messages",
    actionText: "View Message"
});
```

### Example 3: Configuration Template
```javascript
class ConfigTemplate {
    static generateConfig(settings) {
        const {
            database: {
                host = "localhost",
                port = 5432,
                name,
                username,
                password
            } = {},
            api: {
                baseUrl,
                timeout = 30000,
                retries = 3
            } = {},
            features: {
                caching = true,
                logging = true,
                monitoring = false
            } = {}
        } = settings;
        
        return `
            # Application Configuration
            # Generated on ${new Date().toISOString()}
            
            [database]
            host = ${host}
            port = ${port}
            name = ${name}
            username = ${username}
            password = ${password}
            
            [api]
            base_url = ${baseUrl}
            timeout = ${timeout}
            retries = ${retries}
            
            [features]
            caching = ${caching}
            logging = ${logging}
            monitoring = ${monitoring}
        `;
    }
}

// Usage
let config = ConfigTemplate.generateConfig({
    database: {
        host: "db.example.com",
        name: "myapp",
        username: "admin",
        password: "secret"
    },
    api: {
        baseUrl: "https://api.example.com",
        timeout: 60000
    },
    features: {
        caching: true,
        logging: true,
        monitoring: true
    }
});
```

## Practice Exercises

### Exercise 1: HTML Generator
Create a function that generates HTML using template literals:
- Generate different types of HTML elements
- Handle attributes and content
- Support nested elements
- Include conditional rendering

### Exercise 2: Email Template System
Create an email template system that:
- Generates different types of emails
- Handles dynamic content
- Includes styling
- Supports multiple recipients

### Exercise 3: Configuration Generator
Create a configuration generator that:
- Generates different config formats
- Handles nested settings
- Includes comments and documentation
- Supports environment-specific configs

### Exercise 4: Report Generator
Create a report generator that:
- Formats data into readable reports
- Includes charts and tables
- Handles different report types
- Supports export formats

### Exercise 5: Tagged Template Library
Create a library of tagged template functions:
- SQL query builder
- HTML sanitizer
- CSS-in-JS generator
- Markdown processor

## Common Mistakes

### 1. Not Escaping User Input
```javascript
// Wrong - vulnerable to XSS
let userInput = "<script>alert('XSS')</script>";
let html = `<div>${userInput}</div>`;

// Correct - escape user input
function escapeHtml(text) {
    const div = document.createElement("div");
    div.textContent = text;
    return div.innerHTML;
}

let html = `<div>${escapeHtml(userInput)}</div>`;
```

### 2. Not Handling Undefined Values
```javascript
// Wrong - can cause errors
let user = { name: "John" };
let message = `Hello ${user.name}, you are ${user.age} years old.`;

// Correct - provide default values
let message = `Hello ${user.name}, you are ${user.age || 0} years old.`;
```

### 3. Not Using Proper Indentation
```javascript
// Wrong - inconsistent indentation
let html = `
<div>
    <h1>Title</h1>
    <p>Content</p>
</div>
`;

// Correct - consistent indentation
let html = `
    <div>
        <h1>Title</h1>
        <p>Content</p>
    </div>
`;
```

## Key Takeaways

- Template literals provide a powerful way to create strings with embedded expressions
- Use multi-line strings for better readability
- Tagged templates allow for custom string processing
- Always escape user input to prevent security issues
- Use default values to handle undefined properties
- Practice with real-world examples to understand different use cases

## Next Steps

In the next lesson, we'll learn about:
- ES6 classes and inheritance
- Class methods and properties
- Static methods and properties
- Getters and setters
- Class inheritance and super keyword

---

**Ready to master object-oriented programming? Let's learn about Classes! 🚀**

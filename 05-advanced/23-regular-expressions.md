# 23 - Regular Expressions

Regular expressions (regex) are powerful patterns used to match, search, and manipulate text. They are essential for data validation, text processing, and pattern matching in JavaScript.

## 📚 Table of Contents
- [What are Regular Expressions?](#what-are-regular-expressions)
- [Creating Regular Expressions](#creating-regular-expressions)
- [Basic Patterns](#basic-patterns)
- [Character Classes](#character-classes)
- [Quantifiers](#quantifiers)
- [Anchors](#anchors)
- [Groups and Capturing](#groups-and-capturing)
- [Flags](#flags)
- [Common Patterns](#common-patterns)
- [String Methods with Regex](#string-methods-with-regex)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## What are Regular Expressions?

Regular expressions are patterns that describe sets of strings. They are used to:
- Validate input data
- Search and replace text
- Extract information from strings
- Parse structured data

## Creating Regular Expressions

### Literal Syntax
```javascript
const pattern = /hello/;
const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
```

### Constructor Syntax
```javascript
const pattern = new RegExp('hello');
const emailPattern = new RegExp('^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$');
```

### Dynamic Patterns
```javascript
function createEmailPattern(domain) {
  const escapedDomain = domain.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
  return new RegExp(`^[^\\s@]+@${escapedDomain}$`);
}

const gmailPattern = createEmailPattern('gmail.com');
console.log(gmailPattern.test('user@gmail.com')); // true
```

## Basic Patterns

### Literal Characters
```javascript
const pattern = /hello/;
console.log(pattern.test('hello world')); // true
console.log(pattern.test('Hello world')); // false (case sensitive)
```

### Special Characters
```javascript
// Dot (.) matches any character except newline
const dotPattern = /h.llo/;
console.log(dotPattern.test('hello')); // true
console.log(dotPattern.test('hallo')); // true
console.log(dotPattern.test('h\nllo')); // false

// Escaping special characters
const dotLiteral = /h\.llo/; // Matches "h.llo" literally
console.log(dotLiteral.test('h.llo')); // true
console.log(dotLiteral.test('hello')); // false
```

## Character Classes

### Basic Character Classes
```javascript
// Square brackets define character classes
const vowelPattern = /[aeiou]/;
console.log(vowelPattern.test('hello')); // true
console.log(vowelPattern.test('xyz')); // false

// Range of characters
const lowercasePattern = /[a-z]/;
const digitPattern = /[0-9]/;
const alphanumericPattern = /[a-zA-Z0-9]/;

// Negated character class
const nonDigitPattern = /[^0-9]/;
console.log(nonDigitPattern.test('123')); // false
console.log(nonDigitPattern.test('12a')); // true
```

### Predefined Character Classes
```javascript
// \d - digits (0-9)
const digitPattern = /\d/;
console.log(digitPattern.test('abc123')); // true

// \D - non-digits
const nonDigitPattern = /\D/;
console.log(nonDigitPattern.test('123')); // false

// \w - word characters (a-z, A-Z, 0-9, _)
const wordPattern = /\w/;
console.log(wordPattern.test('hello')); // true
console.log(wordPattern.test('hello-world')); // true

// \W - non-word characters
const nonWordPattern = /\W/;
console.log(nonWordPattern.test('hello')); // false
console.log(nonWordPattern.test('hello world')); // true

// \s - whitespace characters
const whitespacePattern = /\s/;
console.log(whitespacePattern.test('hello world')); // true

// \S - non-whitespace characters
const nonWhitespacePattern = /\S/;
console.log(nonWhitespacePattern.test('   ')); // false
console.log(nonWhitespacePattern.test('hello')); // true
```

## Quantifiers

### Basic Quantifiers
```javascript
// * - zero or more
const zeroOrMorePattern = /a*/;
console.log(zeroOrMorePattern.test('')); // true
console.log(zeroOrMorePattern.test('a')); // true
console.log(zeroOrMorePattern.test('aaa')); // true

// + - one or more
const oneOrMorePattern = /a+/;
console.log(oneOrMorePattern.test('')); // false
console.log(oneOrMorePattern.test('a')); // true
console.log(oneOrMorePattern.test('aaa')); // true

// ? - zero or one
const zeroOrOnePattern = /a?/;
console.log(zeroOrOnePattern.test('')); // true
console.log(zeroOrOnePattern.test('a')); // true
console.log(zeroOrOnePattern.test('aa')); // true (matches first 'a')
```

### Specific Quantifiers
```javascript
// {n} - exactly n times
const exactlyThreePattern = /a{3}/;
console.log(exactlyThreePattern.test('aaa')); // true
console.log(exactlyThreePattern.test('aa')); // false

// {n,} - n or more times
const threeOrMorePattern = /a{3,}/;
console.log(threeOrMorePattern.test('aaa')); // true
console.log(threeOrMorePattern.test('aaaa')); // true
console.log(threeOrMorePattern.test('aa')); // false

// {n,m} - between n and m times
const twoToFourPattern = /a{2,4}/;
console.log(twoToFourPattern.test('aa')); // true
console.log(twoToFourPattern.test('aaa')); // true
console.log(twoToFourPattern.test('aaaa')); // true
console.log(twoToFourPattern.test('a')); // false
console.log(twoToFourPattern.test('aaaaa')); // true (matches first 4 'a's)
```

## Anchors

### Start and End Anchors
```javascript
// ^ - start of string
const startPattern = /^hello/;
console.log(startPattern.test('hello world')); // true
console.log(startPattern.test('world hello')); // false

// $ - end of string
const endPattern = /world$/;
console.log(endPattern.test('hello world')); // true
console.log(endPattern.test('world hello')); // false

// ^ and $ together - exact match
const exactPattern = /^hello$/;
console.log(exactPattern.test('hello')); // true
console.log(exactPattern.test('hello world')); // false
```

### Word Boundaries
```javascript
// \b - word boundary
const wordBoundaryPattern = /\bcat\b/;
console.log(wordBoundaryPattern.test('cat')); // true
console.log(wordBoundaryPattern.test('category')); // false
console.log(wordBoundaryPattern.test('the cat sat')); // true

// \B - non-word boundary
const nonWordBoundaryPattern = /\Bcat\B/;
console.log(nonWordBoundaryPattern.test('cat')); // false
console.log(nonWordBoundaryPattern.test('category')); // true
```

## Groups and Capturing

### Capturing Groups
```javascript
// Parentheses create capturing groups
const phonePattern = /(\d{3})-(\d{3})-(\d{4})/;
const match = phonePattern.exec('123-456-7890');

if (match) {
  console.log(match[0]); // "123-456-7890" (full match)
  console.log(match[1]); // "123" (first group)
  console.log(match[2]); // "456" (second group)
  console.log(match[3]); // "7890" (third group)
}
```

### Named Groups (ES2018)
```javascript
const phonePattern = /(?<area>\d{3})-(?<exchange>\d{3})-(?<number>\d{4})/;
const match = phonePattern.exec('123-456-7890');

if (match) {
  console.log(match.groups.area);    // "123"
  console.log(match.groups.exchange); // "456"
  console.log(match.groups.number);  // "7890"
}
```

### Non-capturing Groups
```javascript
// (?:...) - non-capturing group
const nonCapturingPattern = /(?:Mr|Mrs|Ms)\.\s(\w+)/;
const match = nonCapturingPattern.exec('Mr. Smith');

if (match) {
  console.log(match[1]); // "Smith" (only the name is captured)
}
```

### Lookahead and Lookbehind
```javascript
// Positive lookahead (?=...)
const passwordPattern = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/;
console.log(passwordPattern.test('Password123')); // true
console.log(passwordPattern.test('password')); // false

// Negative lookahead (?!...)
const noConsecutivePattern = /^(?!.*(.)\1).*$/;
console.log(noConsecutivePattern.test('abc')); // true
console.log(noConsecutivePattern.test('aabc')); // false

// Positive lookbehind (?<=...)
const currencyPattern = /(?<=\$)\d+\.\d{2}/;
console.log(currencyPattern.test('$123.45')); // true

// Negative lookbehind (?<!...)
const notAfterDollarPattern = /(?<!\$)\d+\.\d{2}/;
console.log(notAfterDollarPattern.test('123.45')); // true
console.log(notAfterDollarPattern.test('$123.45')); // false
```

## Flags

### Global Flag (g)
```javascript
const text = 'cat bat hat';
const pattern = /at/g;
console.log(text.match(pattern)); // ['at', 'at', 'at']

// Without global flag
const nonGlobalPattern = /at/;
console.log(text.match(nonGlobalPattern)); // ['at'] (only first match)
```

### Case Insensitive Flag (i)
```javascript
const caseInsensitivePattern = /hello/i;
console.log(caseInsensitivePattern.test('Hello')); // true
console.log(caseInsensitivePattern.test('HELLO')); // true
console.log(caseInsensitivePattern.test('hElLo')); // true
```

### Multiline Flag (m)
```javascript
const text = `Line 1
Line 2
Line 3`;

// Without multiline flag
const startPattern = /^Line/;
console.log(text.match(startPattern)); // ['Line'] (only first line)

// With multiline flag
const multilinePattern = /^Line/m;
console.log(text.match(multilinePattern)); // ['Line'] (still only first match)

// To match all lines starting with "Line"
const allLinesPattern = /^Line/gm;
console.log(text.match(allLinesPattern)); // ['Line', 'Line', 'Line']
```

### Dot All Flag (s)
```javascript
const text = 'hello\nworld';
const dotPattern = /hello.world/;
console.log(dotPattern.test(text)); // false (dot doesn't match newline)

const dotAllPattern = /hello.world/s;
console.log(dotAllPattern.test(text)); // true
```

## Common Patterns

### Email Validation
```javascript
const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailPattern.test('user@example.com')); // true
console.log(emailPattern.test('invalid-email')); // false
```

### Phone Number
```javascript
const phonePattern = /^(\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})$/;
console.log(phonePattern.test('123-456-7890')); // true
console.log(phonePattern.test('(123) 456-7890')); // true
console.log(phonePattern.test('123.456.7890')); // true
```

### URL Validation
```javascript
const urlPattern = /^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)$/;
console.log(urlPattern.test('https://www.example.com')); // true
console.log(urlPattern.test('http://example.com')); // true
```

### Credit Card
```javascript
const creditCardPattern = /^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3[0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})$/;
console.log(creditCardPattern.test('4111111111111111')); // true (Visa)
console.log(creditCardPattern.test('5555555555554444')); // true (Mastercard)
```

## String Methods with Regex

### test() and exec()
```javascript
const pattern = /\d+/;
const text = 'I have 123 apples';

console.log(pattern.test(text)); // true

const match = pattern.exec(text);
console.log(match[0]); // "123"
console.log(match.index); // 7
console.log(match.input); // "I have 123 apples"
```

### match()
```javascript
const text = 'The quick brown fox jumps over the lazy dog';
const wordPattern = /\w+/g;

const matches = text.match(wordPattern);
console.log(matches); // ['The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog']
```

### replace()
```javascript
const text = 'Hello World';
const result = text.replace(/\w+/g, word => word.toUpperCase());
console.log(result); // "HELLO WORLD"

// Using replacement function
const phoneText = 'Call me at 123-456-7890';
const maskedPhone = phoneText.replace(/(\d{3})-(\d{3})-(\d{4})/, '($1) $2-$3');
console.log(maskedPhone); // "Call me at (123) 456-7890"
```

### split()
```javascript
const text = 'apple,banana,cherry';
const fruits = text.split(',');
console.log(fruits); // ['apple', 'banana', 'cherry']

// Using regex
const text2 = 'apple, banana; cherry';
const fruits2 = text2.split(/[,;]\s*/);
console.log(fruits2); // ['apple', 'banana', 'cherry']
```

### search()
```javascript
const text = 'The quick brown fox';
const index = text.search(/brown/);
console.log(index); // 10

const notFound = text.search(/purple/);
console.log(notFound); // -1
```

## Practical Examples

### Example 1: Form Validation
```javascript
class FormValidator {
  constructor() {
    this.patterns = {
      email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
      phone: /^(\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})$/,
      password: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
      username: /^[a-zA-Z0-9_]{3,20}$/,
      url: /^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)$/
    };
  }
  
  validate(field, value) {
    const pattern = this.patterns[field];
    if (!pattern) {
      throw new Error(`Unknown field: ${field}`);
    }
    
    return pattern.test(value);
  }
  
  validateAll(formData) {
    const results = {};
    
    for (const [field, value] of Object.entries(formData)) {
      results[field] = this.validate(field, value);
    }
    
    return results;
  }
}

// Usage
const validator = new FormValidator();
const formData = {
  email: 'user@example.com',
  phone: '123-456-7890',
  password: 'Password123!',
  username: 'user123',
  url: 'https://www.example.com'
};

const validationResults = validator.validateAll(formData);
console.log(validationResults);
```

### Example 2: Text Processing
```javascript
class TextProcessor {
  static extractEmails(text) {
    const emailPattern = /\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b/g;
    return text.match(emailPattern) || [];
  }
  
  static extractPhoneNumbers(text) {
    const phonePattern = /(\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})/g;
    const matches = text.match(phonePattern) || [];
    return matches.map(phone => phone.trim());
  }
  
  static extractUrls(text) {
    const urlPattern = /https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)/g;
    return text.match(urlPattern) || [];
  }
  
  static extractHashtags(text) {
    const hashtagPattern = /#\w+/g;
    return text.match(hashtagPattern) || [];
  }
  
  static extractMentions(text) {
    const mentionPattern = /@\w+/g;
    return text.match(mentionPattern) || [];
  }
  
  static cleanText(text) {
    // Remove extra whitespace
    let cleaned = text.replace(/\s+/g, ' ');
    
    // Remove special characters except basic punctuation
    cleaned = cleaned.replace(/[^\w\s.,!?;:]/g, '');
    
    // Trim whitespace
    return cleaned.trim();
  }
  
  static highlightKeywords(text, keywords) {
    const escapedKeywords = keywords.map(keyword => 
      keyword.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')
    );
    const pattern = new RegExp(`\\b(${escapedKeywords.join('|')})\\b`, 'gi');
    
    return text.replace(pattern, '<mark>$1</mark>');
  }
}

// Usage
const text = `
  Contact us at support@example.com or call 123-456-7890.
  Visit our website at https://www.example.com
  Follow us on Twitter @example and use #javascript
`;

console.log('Emails:', TextProcessor.extractEmails(text));
console.log('Phone Numbers:', TextProcessor.extractPhoneNumbers(text));
console.log('URLs:', TextProcessor.extractUrls(text));
console.log('Hashtags:', TextProcessor.extractHashtags(text));
console.log('Mentions:', TextProcessor.extractMentions(text));

const cleaned = TextProcessor.cleanText(text);
console.log('Cleaned text:', cleaned);

const highlighted = TextProcessor.highlightKeywords(text, ['example', 'javascript']);
console.log('Highlighted:', highlighted);
```

### Example 3: Log Parser
```javascript
class LogParser {
  constructor() {
    this.patterns = {
      timestamp: /(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})/,
      level: /(INFO|WARN|ERROR|DEBUG)/,
      message: /\[(.*?)\]/,
      ip: /\b(?:\d{1,3}\.){3}\d{1,3}\b/,
      userAgent: /User-Agent: (.+?)(?:\n|$)/
    };
  }
  
  parseLogLine(line) {
    const entry = {};
    
    // Extract timestamp
    const timestampMatch = line.match(this.patterns.timestamp);
    if (timestampMatch) {
      entry.timestamp = new Date(timestampMatch[1]);
    }
    
    // Extract log level
    const levelMatch = line.match(this.patterns.level);
    if (levelMatch) {
      entry.level = levelMatch[1];
    }
    
    // Extract message
    const messageMatch = line.match(this.patterns.message);
    if (messageMatch) {
      entry.message = messageMatch[1];
    }
    
    // Extract IP address
    const ipMatch = line.match(this.patterns.ip);
    if (ipMatch) {
      entry.ip = ipMatch[0];
    }
    
    // Extract User-Agent
    const userAgentMatch = line.match(this.patterns.userAgent);
    if (userAgentMatch) {
      entry.userAgent = userAgentMatch[1];
    }
    
    return entry;
  }
  
  parseLogs(logText) {
    const lines = logText.split('\n');
    return lines
      .filter(line => line.trim())
      .map(line => this.parseLogLine(line))
      .filter(entry => Object.keys(entry).length > 0);
  }
  
  filterByLevel(logs, level) {
    return logs.filter(log => log.level === level);
  }
  
  filterByDateRange(logs, startDate, endDate) {
    return logs.filter(log => {
      if (!log.timestamp) return false;
      return log.timestamp >= startDate && log.timestamp <= endDate;
    });
  }
  
  getUniqueIPs(logs) {
    const ips = logs
      .map(log => log.ip)
      .filter(ip => ip)
      .filter((ip, index, arr) => arr.indexOf(ip) === index);
    return ips;
  }
}

// Usage
const logText = `
2024-01-15 10:30:15 INFO [User login successful] IP: 192.168.1.100 User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
2024-01-15 10:31:22 ERROR [Database connection failed] IP: 192.168.1.101 User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36
2024-01-15 10:32:45 WARN [High memory usage detected] IP: 192.168.1.100 User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36
`;

const parser = new LogParser();
const logs = parser.parseLogs(logText);

console.log('All logs:', logs);
console.log('Error logs:', parser.filterByLevel(logs, 'ERROR'));
console.log('Unique IPs:', parser.getUniqueIPs(logs));
```

## Exercises

### Exercise 1: Email Validator
Create a comprehensive email validator that handles various email formats and edge cases.

### Exercise 2: Password Strength Checker
Build a password strength checker that validates password complexity using regex patterns.

### Exercise 3: HTML Tag Extractor
Create a function that extracts all HTML tags and their attributes from a string.

### Exercise 4: Credit Card Formatter
Build a credit card formatter that formats card numbers as the user types (e.g., 1234 5678 9012 3456).

### Exercise 5: Log Analyzer
Create a log analyzer that can parse different log formats and extract specific information.

## Key Takeaways

- Regular expressions are powerful tools for pattern matching and text manipulation
- Use character classes, quantifiers, and anchors to build precise patterns
- Capturing groups allow you to extract specific parts of matches
- Flags modify how regex patterns behave (global, case-insensitive, multiline, etc.)
- String methods like match(), replace(), and split() work seamlessly with regex
- Test your patterns thoroughly with various inputs
- Use online regex testers to validate complex patterns
- Consider performance implications for complex patterns on large texts
- Escape special characters when using regex in strings
- Use named groups for better readability in complex patterns

Regular expressions are essential for data validation, text processing, and pattern matching in JavaScript applications.

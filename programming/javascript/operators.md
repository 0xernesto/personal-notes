---
description: Overview of JavaScript Syntax
---

# Syntax

### **Optional Chaining Operator (?.)**&#x20;

This operator (<mark style="color:yellow;">`?.`</mark>) allows access to nested object properties without having to check if each intermediate object exists. Returns `undefined` if the property chain breaks.

**Example:** If `book` exists, give me `authorName`; if there's no `book`, leave `authorName` undefined without causing an error.

```javascript
const authorName = book?.authorName;
```

### Non-Null Assertion Operator (!)

This operator (<mark style="color:yellow;">`!`</mark>) can be appended to a variable to tell TypeScript that the variable should not be `null` or `undefined`, even if its type suggests that it could be.

**Example:** Say the type of address is `Address | null`, but we know for sure that by the time we feed it to the `truncateAddress()` function, `address` should not be null. We can append the non-null operator to `address` to tell TypeScript to shut up.

```typescript
truncateAddress(address!);
```

### Nullish Coalescing Operator (??)

This operator (<mark style="color:yellow;">`??`</mark>) returns the right-hand operand when the left-hand operand is `null` or `undefined`, otherwise, the left-hand operand is returned.

**Example:** If the user provided a nickname, use that; otherwise, use their full name as default.

```javascript
const displayName = user.nickname ?? user.fullName;
```

### Logical OR Operator (||)

This operator (<mark style="color:yellow;">`||`</mark>) can be used in logical statements or to set default/fallback values for a variable if the left-hand operand is `null` or `undefined`.

**Example:** Set `username` to `fetchedUsername` , but if `fetchedUsername` is `null` or `undefined`, set `username` to `"Guest"`.

```javascript
const username = fetchedUsername || "Guest";
```

### Logical AND Operator (&&)

This operator (<mark style="color:yellow;">`&&`</mark>) can be used in logical statements or for shorthand if-statements, where the right-hand operand is returned only if the left-hand operand is truthy (i.e., not `null` or `undefined`).

**Example:** If the user is logged in, call the `showWelcomeMessage()` function.

```javascript
isLoggedIn && showWelcomeMessage();
```

### &#x20;Ternary Statement (? :)

This operator (<mark style="color:yellow;">`? :`</mark>) is essentially a shorthand if-else statement. If the logical condition is truthy, the expression after the `?` is returned, otherwise, the expression after the `:` is returned.

**Example:** Say `isLoggedIn` is a boolean variable. If `isLoggedIn` is `true`, the `showWelcomeMessage()` function is called, otherwise the `showLoginPrompt()` function is called.

```javascript
isLoggedIn ? showWelcomeMessage() : showLoginPrompt();
```

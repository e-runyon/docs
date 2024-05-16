---
Title: 'Errors'
Description: 'Errors are objects that can provide detailed information regarding the source of a runtime error. Each error is provided with a "name" for the type of error, and a "message" to describe the error that occurred. Javascript will throw an error when the runtime environment encounters unexpected behavior. 

Throwing an error will stop the execution of the current function and return the error object. This is also known as an exception. Thrown errors are caught by the next outer "catch" block of a "try...catch...finally" statement. If no catch block exists, the program will end abruptly and display an uncaught error statement in the terminal window.'
Subjects:
  - 'Web Development'
  - 'Computer Science'
Tags:
  - 'Error Handling'
  - 'Errors'
  - 'Exceptions'
  - 'Try'
  - 'Catch'
CatalogContent:
  - 'introduction-to-javascript'
  - 'paths/create-a-back-end-app-with-javascript'
---

`Error`s are objects that can provide detailed information regarding the source of a runtime error. Each error is provided with a `name` for the type of error, and a `message` to describe the error that occurred. Javascript will throw an error when the runtime environment encounters unexpected behavior. 

Throwing an error will stop the execution of the current function and return the error object. This is also known as an exception. Thrown errors are caught by the next outer `catch` block of a `try...catch...finally` statement. If no catch block exists, the program will end abruptly and display an uncaught error statement in the terminal window.

## The Error Object

The error object holds information about the exception that was thrown in its two properties:

- `name` Provides the type of error.
- `message` A human-readable description of the error instance.

The following types of error can be returned by the `name` property:

- "EvalError" An error has occurred in the eval() function (Note: Depreciated in newer versions of JavaScript)
- "RangeError" A number "out of range" has occurred
- "ReferenceError" An illegal reference has occurred
- "SyntaxError" A syntax error has occurred
- "TypeError" A type error has occurred
- "URIError" An error in encodeURI() has occurred

These are some example messages for various types of errors:

- RangeError
  - invalid array length
  - invalid date
- ReferenceError
  - "x" is not defined
  - assignment to undeclared variable "x"
- SyntaxError
  - "x" is a reserved identifier
  - a declaration in the head of a for-of loop can't have an initializer
- TypeError
  - "x" is not a function
  - "x" is read-only
- URIError
  - malformed URI sequence

## The Error() Constructor

The `Error()` constructor is used to create new Error objects and will accept the following parameters: 

- `message` A detailed description of the problem that has occurred
- `options` An object containing a `cause` property
  - `cause` A value representing the original error. It is used to catch and re-throw errors with additional details
- `fileName` A String value of the path where the error occurred
- `lineNumber` A positive numerical value containing the line number of where the error occurred in the corresponding `fileName`

> **Note:** Both `fileName` and `lineNumber` are not compatible with all browsers and should not be used in public applications.

### Syntax

Prefacing the `Error()` constructor with the `new` operator is optional as both cases will return a new `Error` object.

```js
Error();
Error(message);
Error(message, options);
Error(message, fileName);
Error(message, fileName, lineNumber);
```

For more expressive exceptions, we can use a type of error in place of the generic `Error` descriptor.

```js
const value = "Dog";
function isNumber(x) {
  if (isNaN(x)) {
    throw RangeError("This value is not a number")
  }
}

try {
    isNumber(value);
    console.log(`${value} is a number`);
} catch (error) {
    console.error(error);
}
```

### Example

Instances of the `Error` object can be stored in variables or other objects. However, it may be more efficient throw the `Error` object instance directly.

```js
const errorMessage = Error("Username not found");
throw errorMessage;
```

```js
// Shorthand Error instance
throw Error("Username not found");
```

Both examples above will result in an error message prompt similiar to what is below:

```shell
Error: Username not found
  at Object.<anonymous> (C:\Users\...\test.js:1:7)
```

Following the error message will be the function call stack. This provides the path of function calls responsible for the error. The most recent function where the error originates from will be at the top with the file name, line number, and character number. At the bottom of the list is the starting function call that lead to the error.

### CodeByte Example

```codebyte/javascript
const x = 5;

function isEven(number){
    if ( number % 2 !== 0 ) {
        throw new Error("Number must be even");
    };
    return true;
}

try {
    const answer = isEven(x);
    console.log(answer);
} catch (error) {
    console.error(error);
}
```

## RangeError

A `RangeError` is thrown when a passed parameter exceeds the allowed range of values.

```js
const timestamp1 = new Date("23 October 2077 09:42 EST");
const timestamp2 = new Date("Today");
console.log(timestamp1.toISOString()); // Expected Output: 2077-10-23T14:42:00.000Z
console.log(timestamp2.toISOString()); // RangeError: Invalid time value
```

In this example, a `RangeError` occurs because `timestamp2` was an invalid date value. Only date strings in the correct range and format are allowed.

## ReferenceError

A `ReferenceError` occurs when trying to access a variable that does not exist or has not been initialized.

```js
console.log("Welcome " + username);
const username = "Guest";
```

Since `username` is not defined until after we log our message to the console, we encounter a reference error.

```shell
ReferenceError: Cannot access 'username' before initialization
```

## SyntaxError

A `SyntaxError` can occur when JavaScript encounters code that does not follow an expected format. In essence, it means there is a problem with the way the code is written. This may be as simple as adding or forgetting an extra operator, quotation, punctuation, or bracket.

```js
const limit = 10;

try {
    for (let i = 1; i <= limit; i++) {
        console.log(i);
    })  // An extra `)` was added here accidentally
} catch (error) {
    console.error(error);
}
```

When using parentheses, brackets, or quotations, the closing character must be accompanied the respective opening character. In our case, the extra closing parentheses after the `for` statement has no corresponding opening character.

```shell
SyntaxError: Unexpected token ')'
```

## TypeError

A `TypeError` indicates that an invalid operation was performed on a value that was not compatible. These errors are a sign that a value's type (i.e. String, Number, Array, ...) does not match what is expected.

```js
let message;
console.log(message.length);
```

This code snippet will result in a `TypeError` because the we are trying to access the method `.length` of an `undefined` variable. Initializing or reassigning the `message` variable with a String or an Array before accessing it would resolve this issue.

```shell
TypeError: Cannot read properties of undefined (reading 'length')
```

## URIError

A `URIError` comes from passing illegal parameters to the `encodeURI` or `decodeURI` global functions. These include character sequences which have no conversion to any Unicode characters, also known as lone surrogates, and poorly formed sequences that do not adhere to the Unicode standard formatting.

```js
const message = "H%C3%A911%C3%B6%20W%C3%B4R1%C3%9"; // A zero was deleted at the end of the string
console.log(decodeURI(message));
```

The message cannot be decoded because JavaScript does not have the information necessary to recompile the whole sequence of characters into its original form. Incomplete or fragmented sequences will result in a `URIError`. During these instances, evaluate the parameters being passed to the encoding or decoding functions for descrepencies.

## AggregateError

An `AggregateError` is when multiple errors occur within an operation. It is commonly used by `Promise.any()` to report when all passed promises were rejected. 

```js
const promise1 = Promise.reject(Error('Cannot be of type: Number'));
const promise2 = Promise.reject(Error('Cannot be less than 8 characters'));
const promise3 = Promise.reject(Error('Username already taken'));
const promises = [promise1, promise2, promise3];

Promise.any(promises).then((result) => {
    console.log(result);
}).catch((error) => {
    console.error(error);
});
```

Running this code will return a message similar to the following:

```shell
[AggregateError: All promises were rejected] {
  [errors]: [
    Error: Cannot be of type: Number
      at ...
    Error: Cannot be less than 8 characters
      at ...
    Error: Username already taken
      at ...
  ]
}
```

## InternalError

An `InternalError` indicates that the JavaScript engine has encountered a problem with a recursive function. A recursive function is one that calls itself until a base condition is met. In situations where no base condition is established or the recursive limit is set too high, an `InternalError` is likely to happen. In applications outside of Firefox, this may be interpreted as a `RangeError`.

> **Note:** `InternalError` is not compatible with all browsers and should not be used in public applications.

```js
const max = 1000000000000;
const start = 0;
function plusOneToMax(count) {
  if (count >= max) {
    return;
  } else {
    plusOneToMax(count + 1);
  }
}

try {
  plusOneToMax(start);
  console.log("Finished!")
} catch (error) {
  console.error(error);
}
```

Running this code will flood the call stack with the `plusOneToMax()` function. JavaScript will throw an exception if there are excessive calls to the recursive function.
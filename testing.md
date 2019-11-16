# testing

<!-- TOC -->
- [in code](#in-code)
  - [numbers](#numbers)
  - [try](#try)
  - [catch](#catch)
  - [throw](#throw)
  - [finally](#finally)
  - [errors](#errors)
- [of code](#of-code)
  - [unit testing](#unit-testing)
  - [integration testing](#integration-testing)
  - [end-to-end testing](#end-to-end-testing)

<!-- TOC END -->

## in code

### numbers
`isNaN(<value>)` tests whether a value is an illegal number.
```javascript
isNaN(123) // false
isNaN('123') // false
isNaN(true) // false
```

`Number.isInteger(<value>)` tests whether value is an integer.

### try
Requires at least one `catch` or `finally` clause.
```javascript
try {
  addMessage("Howdy friend!");
}
catch(err) {
  // handle...
}
```

### catch
If JS creates an err object, this will run. You can access the _name_ and _message_ properties.
```javascript
catch(err) {
  alert(err.message);
}
```

### throw
For creating custom error messages.
```javascript

```

### finally
Runs regardless of the try/catch result.
```javascript
finally {
  console.log('all done');
}
```

### errors
The error object has two properties, `name` and `message`.

Error name values are one of six types:
* EvalError
* RangeError
* ReferenceError
* SyntaxError
* TypeError
* URIError

## of code

### unit testing

### integration testing

### end-to-end testing

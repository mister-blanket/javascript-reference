# JavaScript Reference

<!-- TOC -->
- [general concepts](#general-concepts)
  - [scope](#scope)
  - [hoisting](#hoisting)
  - [higher order components (HOC)](#higher-order-components-hoc)
  - [inheritance](#inheritance)
  - [bubbling](#bubbling)
  - [event loop](#event-loop)
  - [strict mode](#strict-mode)
- [operators](#operators)
  - [`==`](#)
  - [`===`](#-1)
- [objects](#objects)
  - [`assign()`](#assign)
- [(functions)[functions.md]](#functionsfunctionsmd)
- [arrays](#arrays)
  - [`filter()`](#filter)
  - [`join()`](#join)
  - [`map()`](#map)
  - [`pop()`](#pop)
  - [`push()`](#push)
  - [`reverse()`](#reverse)
  - [`shift()`](#shift)
  - [`slice(start, end)`](#slicestart-end)
  - [`splice(start, items)`](#splicestart-items)
  - [`unshift()`](#unshift)
- [strings](#strings)
  - [`slice(start, end)`](#slicestart-end-1)
  - [`split()`](#split)
- [promises & callbacks](#promises--callbacks)
- [class](#class)
  - [class declarations](#class-declarations)
  - [sub classing with `extends`](#sub-classing-with-extends)
  - [class constructor](#class-constructor)
- [testing](#testing)
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


# general concepts

## scope
JS uses lexical scope, which allows scope to be determined by reading static text. Other languages (bash, C) use Dynamic Scope, which can change mid-function and is not as readable.


## hoisting
`var` is hoisted. `let` and `const` are not.

Declarations (`var x;`) are hoisted. Initializations (`x = 2;`) are not.

## higher order components (HOC)

## inheritance
JS isn't class based, it's prototype based.

__prototype chain__. Top level is null. Values are checked from the bottom up.

`hasOwnProperty()` checks only that object, and won't traverse the prototype chain.


## bubbling
Onclick events work in nested elements and bubble most-nested to least of there are multiple onclick listeners.

`event.target` is the most deeply nested element that triggered an event. Doesn't change through the bubbling process.

`event.currentTarget` (or `this`) is the current element. Changes as bubble goes up.

`event.stopPropigation()` prevents bubbling beyond clicked element.


## event loop
Messages arrive in order and are processed synchronously. Anything in a `setTimeout()` callback will be processed after all other code, even if the timeout is 0.

__stack overflow__ can happen when a recursive call gets too nested and the stack builds up. To solve this, put the recursive call in a setTimeout(), which will put each new call in the event queue rather than the stack.
```javascript
var list = readHugeList();

var nextListItem = function() {
    var item = list.pop();

    if (item) {
        // process the list item...
        setTimeout( nextListItem, 0);
    }
};
```

## strict mode
* Makes debugging easier by throwing errors, which would have otherwise passed.
* Prevents accidental globals
* Eliminates `this` being set to window


---
# operators

### `==`
Returns true if both expressions have the same value. Does coercion.

### `===`
Returns true if both expressions have the same type and same value.


---
# objects


## `assign()`
Deep clones values of all enumerable own properties.
```JavaScript
var x = {name: "michael", friends: {best: "liz", secondBest: 'puppy'}};
var y = Object.assign({}, x);
```

---
# (functions)[functions.md]

---
# arrays

### `filter()`
Creates a new array that pass through the filter requirements.
```javascript
const girlNames = ['donna', 'beverly', 'claire', 'beatrice', 'meg', 'bonita'];

const noBNames = girlNames.filter(name => name[0] !== 'b');

console.log(noBNames); // Array ["donna", "claire", "meg"]
```

### `join()`
Returns an array as a string.

```javascript
let words = ['Hello', 'there', 'comrade!'];
words.join(' '); // Hello there comrade!
```

### `map()`
Creates a new array with the results of passing all elements of another array through a function.
```JavaScript
let nouns = [sad, quick, smart];

// pass function to map
const adjectives = nouns.map(x => x + 'ly');

console.log(adjectives); // [sadly, quickly, smartly]
```

###  `pop()`
Removes the last element of an array and returns that element. _see `shift()` to remove from the beginning_.

### `push()`
Adds an element to the end of an array. _*see `unshift()` to add to beginning_.

### `reverse()`
Reverses array. Changes original array.

### `shift()`
Removes the first element of an array. * mutates the array in real time!

### `slice(start, end)`
Returns a new array with the extracted parts of the old array.
```JavaScript
let arr1 = [1, 2, 3, 5, 8]
let arr2 = arr1.slice(1,-1); // [2, 3, 5]  
```

### `splice(start, items)`
Adds/removes items from an array. Mutates the original array.

array.splice(_index, howmany, item1, ..., itemZ_)
_index_ - starting position
_howmany_ - num items to remove (default = 0)
_item1, ..., itemZ_ - new items to be added to array
```javascript
arr1 = ['justin', 'angie', 'george', 'nikki'];
arr1.splice(2, 1, 'michael', 'krystle'); // ['justin', 'angie', 'michael', 'krystle', 'nikki']
```

### `unshift()`
Adds an element to the beginning of an array.

---
# strings

### `slice(start, end)`
Returns a new string with the extracted parts of the old string.
```JavaScript
'hello friend'.slice(3,-2); //  lo frie
```

### `split()`
Splits a string into an array of substrings and returns the new array. Does not change the original string.

```javascript
let str = 'hi there, you'
str.split(''); // ['h', 'i', ' ', 't', 'h', 'e', 'r', 'e', ',', ' ', 'y', 'o', 'u']
str.split(' '); // ['hi', 'there,', 'friend']
str.split(','); // ['hi there', 'friend']
```


---
# promises & callbacks


---
# class
Unlike functions, classes are not hoisted.

## class declarations
```javascript
class Rectangle { // Rectangle is class name
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```
## sub classing with `extends`
Creates a child class from a parent class.
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); //call the super class constructor and pass in the name parameter
  }

  speak() {
    console.log(this.name + ' barks.');
  }
}

let p = new Dog('Patches');
p.speak(); // Patches barks.
```

## class constructor
Special method for creating and initializing a class object. Can use `super` to call constructor of the super class.

---

# testing

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

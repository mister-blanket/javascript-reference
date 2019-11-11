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
- [functions](#functions)
  - [this](#this)
    - [`this` in a callback](#this-in-a-callback)
    - [`this` in a closure](#this-in-a-closure)
    - [method assigned to a variable](#method-assigned-to-a-variable)
    - [borrowing methods](#borrowing-methods)
  - [arrow functions](#arrow-functions)
  - [closure](#closure)
  - [methods](#methods)
    - [apply, call, & bind](#apply-call--bind)
    - [`apply()`](#apply)
    - [`call()`](#call)
    - [`bind()`](#bind)
  - [currying](#currying)
- [arrays](#arrays)
  - [`filter()`](#filter)
  - [`join()`](#join)
  - [`map()`](#map)
  - [`pop()`](#pop)
  - [`reverse()`](#reverse)
  - [`slice(start, end)`](#slicestart-end)
  - [`splice(start, items)`](#splicestart-items)
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
# functions

## this
In regular functions, `this` represents the object which called the function. `this` is not assigned a value until an object invokes the function.

Simple object example:
```javascript
const me = {
  fn: 'Barry',
  ln: 'Hutts',
  fullName: function() {
    return this.fn + ' ' + this.ln;
  }
}
console.log(me.fullName());
```

### `this` in a callback
```javascript
// will access `button` for this, incorrect
$("button").click (user.clickHandler);

// binds `this` to user, correct.
$("button").click (user.clickHandler.bind(user));
```

### `this` in a closure
The main object's `this` will not carry to an inner function (closure), therefore you must bind the outer function's `this` to a new variable. The inner function (closure) can then access that variable.
```javascript
const user = {
  tournament: "The Masters",
  data: [
    {name: "T. Woods", age:37},
    {name: "P. Mickelson", age:43}
  ],
  clickHandler: function (event) {
  // To capture the value of "this" when it refers to the user object, we have to set it to another variable here:
  // We set the value of "this" to theUserObj variable, so we can use it later
  var theUserObj = this;
  this.data.forEach (function (person) {
    // Instead of using this.tournament, we now use theUserObj.tournament
    console.log (person.name + " is playing at " + theUserObj.tournament);
    })
  }
}

user.clickHandler();
// T. Woods is playing at The Masters
//  P. Mickelson is playing at The Masters
```

### method assigned to a variable
```javascript
// `this` becomes `window`, incorrect.
const showUserData = user.showData;

// `this` bound to `user`, correct.
const showUserData = user.showData.bind(user);
```

### borrowing methods
Use `apply()` to bind `this` to a borrowed method.
```javascript
const gameController = {
  scores: [20, 34, 55, 46, 77],
  avgScore: null,
}

const appController = {
  scores: [900, 845, 809, 950],
  avgScore: null,
  avg: function () {
    const sumOfScores = this.scores.reduce (function (prev, cur, index, array) {
      return prev + cur;
    });
    this.avgScore = sumOfScores / this.scores.length;
  }
}

// If we run the code below, the gameController.avgScore property will be set to the average score from the appController object "scores" array
gameController.avgScore = appController.avg();

// Note that we are using the apply () method, so the 2nd argument has to be an array—the arguments to pass to the appController.avg () method.
appController.avg.apply(gameController, gameController.scores);
```

## arrow functions
No bindings for `this`, `super`, `arguments`, or `new.target`. Therefore, arrow functions are ill-suited as methods.

`hello = val => "Hello " + val;`


## closure
A function bound to its lexical state. Good for data closing and encapsulation.

If closure is not specifically needed, it is better to assign methods to object prototype, as it is better for performance.
```JavaScript
myObject.prototype.getName = () => return this.name;
```

## methods

### apply, call, & bind
Built-in function methods for assigning `this` vale.

Apply & call are useful for borrowing methods, such as borrowing Array methods on an array-like object.

### `apply()`
nestedFunction.apply(cat) gives this context for nestedFunction and invokes it. Takes a single array argument. This is useful for _variadic_ functions; a function that takes a varying number of arguments.

### `call()`
Calls a function with a given `this` value and arguments given individually.
nestedFunction.apply(cat) like Apply, but takes multiple arguments, separated by commas.

### `bind()`
Useful for setting `this` in methods and currying.

Like call, but doesn't invoke function. Allows you to easily set which specific object will be bound to this when a function or method is invoked.

Use in callbacks:
```javascript
// will access `button` for this, incorrect
$("button").click (user.clickHandler);

// binds `this` to user, correct.
$("button").click (user.clickHandler.bind(user));
```

Use when method is assigned to a variable:
```javascript
// `this` becomes `window`, incorrect.
const showUserData = user.showData;

// `this` bound to `user`, correct.
const showUserData = user.showData.bind(user);
```

Use when borrowing methods:

Use when currying:


## currying
A way of splitting function parameters into nested functions, where you only need to pass some variables to each. Helps reduce repetition and encourages you to understand which variables are most likely to change. Works well for creating function factories.

```javascript
const greetMaker = function(greeting) {
  return (name) => {
    console.log(greeting + ', ' + name);
  }
}

const sayHello = greetMaker('Hello there');
sayHello('Elizabeth');

const sayBye = greetMaker('See ya later');
sayBye('Lizzie');

greetMaker('Howdy')('Michael'); // You can also pass each parameter in separate sets of parentheses.
```
To keep things from getting too messy/nested, you can create a currying function like so:
```javascript
const makeCurry = function(uncurried) {
  let param = Array.prototype.slice.call(arguments, 1);
  return function() {
    return uncurried.apply(this, param.concat(
      Array.prototype.slice.call(arguments, 0)
    ));
  };
}

const greetMaker = (greeting, separator, emphasis, name) => {
  console.log(greeting + separator + name + emphasis);
}

const sayHi = makeCurry(greetMaker, 'Hi', ', ', '.');
sayHi('Mork');
```

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
Removes the last element of an array and returns that element.

### `reverse()`
Reverses array. Changes original array.

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

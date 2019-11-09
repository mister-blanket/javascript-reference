# JavaScript Reference


# general concepts

## scope
JS uses lexical scope, which allows scope to be determined by reading static text. Other languages (bash, C) use Dynamic Scope, which can change mid-function and is not as readable.


## hoisting


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

## apply, call, & bind
Built-in function methods for assigning `this` vale.

Apply & call are useful for borrowing methods, such as borrowing Array methods on an array-like object.

### apply()
nestedFunction.apply(cat) gives this context for nestedFunction and invokes it. Takes a single array argument. This is useful for _variadic_ functions; a function that takes a varying number of arguments.

### call()
Calls a function with a given `this` value and arguments given individually.
nestedFunction.apply(cat) like Apply, but takes multiple arguments, separated by commas.

### bind()
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
## filter()
Creates a new array that pass through the filter requirements.
```javascript
const girlNames = ['donna', 'beverly', 'claire', 'beatrice', 'meg', 'bonita'];

const noBNames = girlNames.filter(name => name[0] !== 'b');

console.log(noBNames); // Array ["donna", "claire", "meg"]
```

## map()
Creates a new array with the results of passing all elements of another array through a function.
```JavaScript
let nouns = [sad, quick, smart];

// pass function to map
const adjectives = nouns.map(x => x + 'ly');

console.log(adjectives); // [sadly, quickly, smartly]
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
## unit testing

## integration testing

## end-to-end testing

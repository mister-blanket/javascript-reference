# functions

<!-- TOC -->
- [class](#class)
  - [class declarations](#class-declarations)
  - [sub classing with `extends`](#sub-classing-with-extends)
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
    - [private methods](#private-methods)
- [currying](#currying)
- [promises](#promises)
- [Higher-Order Functions](#higher-order-functions)

<!-- TOC END -->


# class
Special functions for creating and managing objects. Properties are set via the `constructor()` method. Unlike normal functions, classes are not hoisted. It is a good rule to capitalize class variable names.

Class methods can be set after the constructor method.


## class declarations

```javascript
class Rectangle { // Rectangle is class name
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  area() {
    return this.height * this.width;
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



# this
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


# closure
A function bound to its lexical state. Good for data closing and encapsulation.

A simple closure is like accessing a function with only one method:
```JavaScript
const accountBalance = () => {
  let balance = 2200.00;
  const displayBalance = () => console.log(`$${balance}`);
  return displayBalance;
}
const viewBalance = accountBalance();
viewBalance(); // $2200.00
```

More complex closures can be created by returning an object with different methods:
```JavaScript
const bankApp = () => {
  let balance = 0;
  const changeBalance = amount => {
    balance += amount;
    console.log(`Your current balance is $${balance}`);
  }
  return {
    balance: function() {console.log(`$${balance}`);},
    deposit: function(amt) {changeBalance(amt)},
    withdrawl: function(amt) {changeBalance(-amt)}
  };
}
const bank = bankApp();
bank.balance();
```


If closure is not specifically needed, it is better to assign methods to object prototype, as it is better for performance.
```JavaScript
myObject.prototype.getName = () => return this.name;
```

# methods

## apply, call, & bind
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


### private methods
Truly private methods are inefficient for memory and should rarely be used.

Example of a truly private method:
```JavaScript
const Employee = function(name, salary) {
  this.name = name;
  this.salary = salary;

  // private method
  const increaseSalary = function() { // each instance of Employee will create its own copy of this method.
    this.salary = this.salary * 1.02;
  }

  this.displayIncreasedSalary = function() {
    increaseSalary();
    console.log(this.salary);
  }
}
```


# currying
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

# promises
An async action which may, in the future, produce a value. It can notify other actions when it has completed.  

Promise as a variable:
```javascript
let twoSeconds = new Promise((resolve, reject) => {
  setTimeout(() => resolve(), 2000);
});

twoSeconds // Promise runs first
  .then(() => { // This runs after promise is resolved
  console.log('Your two seconds is up.');
});
```

Promise as a function:
```javascript
let waitSeconds = numSeconds => new Promise(resolve => {
  const message = `${numSeconds} seconds have passed!`;
  setTimeout(() => resolve(message), numSeconds * 1000);
});

waitSeconds(4)
  .then(message => console.log(message));
```


# Higher-Order Functions
A Higher-Order Function (HOF) is a function, which takes another function as a parameter. Examples are `map()`, `reduce()`, `filter()`

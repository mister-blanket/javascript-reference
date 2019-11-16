# functions

<!-- TOC -->
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
- [promises & callbacks](#promises--callbacks)

<!-- TOC END -->

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

# promises & callbacks

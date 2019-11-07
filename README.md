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
# function

## closure
A function bound to its lexical state. Good for data closing and encapsulation.

If closure is not specifically needed, it is better to assign methods to object prototype, as it is better for performance.
```JavaScript
myObject.prototype.getName = () => return this.name;
```

## apply, call bind
Built-in function methods for assigning `this` vale.

### apply()
nestedFunction.apply(cat) gives this context for nestedFunction and invokes it. Takes a single array argument.

### call()
Calls a function with a given `this` value and arguments given individually.
nestedFunction.apply(cat) like Apply, but takes multiple arguments, separated by commas.

### bind()
Like call, but doesn't invoke function.

## currying
A way of splitting function parameters into nested functions, where you only need to pass some variables to each. Helps reduce repetition and encourages you to understand which variables are most likely to change.

---
# array
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

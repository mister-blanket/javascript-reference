# objects & class

<!-- TOC -->
- [objects](#objects)
  - [`assign()`](#assign)
- [class](#class)
  - [class declarations](#class-declarations)
  - [sub classing with `extends`](#sub-classing-with-extends)
  - [class constructor](#class-constructor)

<!-- TOC END -->

# objects


## `assign()`
Deep clones values of all enumerable own properties.
```JavaScript
var x = {name: "michael", friends: {best: "liz", secondBest: 'puppy'}};
var y = Object.assign({}, x);
```

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

# objects

<!-- TOC -->
- [general](#general)
  - [creating objects](#creating-objects)
    - [object initializer](#object-initializer)
    - [constructor function (i.e. class)](#constructor-function-ie-class)
    - [`Object.create()`](#objectcreate)
    - [accessors (`get` & `set`)](#accessors-get--set)
  - [enumerating properties](#enumerating-properties)
    - [`for...in` loops](#forin-loops)
    - [`Object.keys(o)`](#objectkeyso)
- [methods](#methods)
  - [`assign()`](#assign)
  - [`delete`](#delete)

<!-- TOC END -->

# general


## creating objects

### object initializer
Also known as 'literal notation', this is simply creating an object from scratch. Objects are instances of `Object`.
```JavaScript
const me = {
  name: 'Michael',
  age: 34,
  decent: false,
  favoritePerson: 'Liz',
  greet: function() {
    return `Hi, my name is ${this.name}`;
  }
}
```


### constructor function (i.e. class)
```JavaScript
class Person {
  constructor(name, age, decent, favoritePerson) {
    this.name = name;
    this.age = age;
    this.decent = decent;
    this.favoritePerson = favoritePerson;
  }
  greet() {
    return `Hi, my name is ${this.name}, I'm ${this.age}, and I like ${this.favoritePerson}`;
  }
}

const me = new Person('Michael', 34, false, 'Liz');
```


### `Object.create()`
The benefit of using `Object.create()` is that you can choose the prototype of a new object without needing to use a constructor function.
```javascript
const Person = {
  type: 'Homo-Erectus',
  displayType: function() {
    return this.type;
  }
}

const michael = Object.create(Person);
```

### accessors (`get` & `set`)
Built-in object methods that allows you to define a process for returning the property.
```javascript
class Person {
  constructor(fn, ln, age) {
    this._fn = fn;
    this._ln = ln;
    this._mn = '';
    this._age = age;
  }
  get fn() { return this._fn };
  get ln() { return this._ln };
  get fullName() {
    return (this._mn === '' ? `${this._fn} ${this._ln}` : `${this._fn} ${this._mn} ${this._ln}`);
  };
  set mn(x) {
    this._mn = x;
  }
}

const me = new Person('Michael', 'Plunkett', 34);
me.fullName; // 'Michael Plunkett'
me.mn = 'Lee';
me.fullname; // 'Michael Lee Plunkett'
```


## enumerating properties

### `for...in` loops
Traverses all enumerable properties of an object and its prototype chain.

### `Object.keys(o)`
Returns an array with all own keys of an object.
```JavaScript
const me = {
  name: 'michael',
  age: 34,
  decent: false,
  favoritePerson: 'Liz'
}

const myKeys = Object.keys(me); // ['name', 'age', 'decent', 'favoritePerson']
```


# methods

### `assign()`
Deep clones values of all enumerable own properties.
```JavaScript
var x = {name: "michael", friends: {best: "liz", secondBest: 'puppy'}};
var y = Object.assign({}, x);
```

### `delete`
Removes the property of an object. Only affects own properties, not prototype properties.


```JavaScript
let me = {
  name: 'Michael';
  hungry: true;
}

me.hungry; // true
delete me.hungry;
me.hungry; // undefined
me.name; // michael
```

Prototype example:
```javascript
var Employee = {
  company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company); // xyz
delete emp1.__proto__.company;
console.log(emp1.company); // undefined
```

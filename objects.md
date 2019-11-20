# objects

<!-- TOC -->
- [objects](#objects)
  - [methods](#methods)
    - [`assign()`](#assign)
- [class](#class)
  - [class declarations](#class-declarations)
  - [sub classing with `extends`](#sub-classing-with-extends)
  - [class constructor](#class-constructor)

<!-- TOC END -->

# objects

```javascript
car = {
  year: 2003,
  make: 'Toyota',
  model: 'Corolla',
  cost: 3200
}
```


## methods

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

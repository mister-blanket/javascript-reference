# design patterns

<!-- TOC -->
- [creational patterns](#creational-patterns)
  - [Constructor Pattern](#constructor-pattern)
  - [Factory Pattern](#factory-pattern)
  - [Singleton Pattern](#singleton-pattern)
- [structural patterns](#structural-patterns)
  - [Decorator Pattern](#decorator-pattern)
  - [Facade Pattern](#facade-pattern)
- [behavioral patterns](#behavioral-patterns)
  - [Observer Pattern](#observer-pattern)
- [architectual patterns](#architectual-patterns)
  - [Model, View, Controller](#model-view-controller)

<!-- TOC END -->

# creational patterns
These patterns help to manage object creation.


### Constructor Pattern
Not a true patter, but a concept in JavaScript. Uses class constructors and use them to initiate new objects.

```JavaScript
// ES6
class Character {
  constructor(name, race, from) {
    this._name = name;
    this._race = race;
    this._from = from;

    this.getInfo = function() {
      const vowels = ['a', 'e', 'i', 'o', 'u'];
      const a = (vowels.indexOf(this._race[0].toLowerCase()) === -1 ? "a" : "an")
      return `${this._name} is ${a} ${this._race} from ${this._from}`;
    };
  }
}

const Frodo = new Character('Frodo Baggins', 'Hobbit', 'The Shire');
const Galadriel = new Character('Galadriel', 'Elf', 'Lothlorien');

Frodo.getInfo();
Galadriel.getInfo();
```


### Factory Pattern
Another class-based creational pattern. It is used for generating multiple types of object creators. Used for creating objects which share many similar characteristics, but some different.

```javascript
class WeaponFactory {
  constructor() {
    this.createWeapon = function(type) {
      let weapon;
      if (type === 'sword' || type === 'axe') weapon = new Melee(type);
      else if (type === 'bow') weapon = new Bow(type);
      weapon.aim = function() {
        return `You aim the ${type}.`;
      };
      return weapon;
    };    
  }
}

class Melee {
  constructor(type) {
    this._type = type;
    this.swing = function() {
      return `You swing the ${this._type}`;
    }
  }
}

class Bow {
  constructor(type) {
    this._type = type;
    this.pull = function() {
      return `You load an arrow in the ${this._type}`;
    }
  }
}

const factory = new WeaponFactory();
const myBow = factory.createWeapon('bow');
const mySword = factory.createWeapon('sword');

mySword.aim();
mySword.swing();
myBow.aim();
myBow.pull();
```


### Prototype pattern
Uses `Object.create` to take the prototype of existing objects and add new properties to it.
```javascript
const person = {
  eyes: 2,
  language: true,
  speak() {
    return 'hello';
  }
};

// Object.create(proto[, propertiesObject])

const elf = Object.create(person, { immortal: { value: true }});

elf.__proto__ === person; // true
```


### Singleton Pattern
If no instance of a class exists, then one will be created. If an instance already exists, a reference to the existing instance will be returned.

```javascript
class Database {
  constructor(data) {
    if (Database.exists) {
      return Database.instance;
    }
    this._data = data;
    Database.instance = this;
    Database.exists = true;
    return this;
  }
  getData() {
    return this._data;
  }

  setData(data) {
    this._data = data;
  }
}

const mongo = new Database('mongo');
mongo.getData(); // mongo

const mysql = new Database('mysql');
mysql.getData(); // mongo
```


# structural patterns
Useful for adding functionality to an object or class without affecting other pieces in the system.

### Decorator Pattern
A way of adding functionality to existing classes. An alternative to sub-classing.
```javascript
class Armor {
  constructor(type, smith, protection) {
    this._type = type;
    this._smith = smith;
    this.protection = protection;
  }

  getInfo() {
    return `${this._type} armor, made by ${this._smith}`;
  }
}

// decorator
function mithril(armor) {
  armor.isMithril = true;
  armor.protection += armor.protection * 2000;
  armor.reveal = function() {
    return `The ${armor.getInfo()} is revealed to be made of mithril. It offers ${armor.protection} protection.`;
  }
  return armor;
}

const frodosMail = mithril(new Armor('Dwarven', 'Dain', 50));
frodosMail.reveal();
```

### Facade Pattern
Creates a simple, public API, which hides internal methods. 


# behavioral patterns
These help dissimilar objects work together.

### Observer Pattern


# architectual patterns

### Model, View, Controller

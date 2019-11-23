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
  }

  getInfo() {
    const vowels = ['a', 'e', 'i', 'o', 'u'];
    const a = (vowels.indexOf(this._race[0].toLowerCase()) === -1 ? "a" : "an")
    return `${this._name} is ${a} ${this._race} from ${this._from}`;
  };
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
  createWeapon(type) {
    let weapon;
    if (type === 'sword' || type === 'axe') weapon = new Melee(type);
    else if (type === 'bow') weapon = new Bow(type);
    weapon.aim = function() {
      return `You aim the ${type}.`;
    };
    return weapon;
  };    
}

class Melee {
  constructor(type) {
    this._type = type;
  }
  swing() {
    return `You swing the ${this._type}`;
  }
}

class Bow {
  constructor(type) {
    this._type = type;
  }
  pull() {
    return `You load an arrow in the ${this._type}`;
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

```javascript
let currentId = 0;

class SightingRegistry {
  registerSighting(sighter, type, details, location) {
    const id = SightingRegistry._uniqueIdGenerator();
    let registry;
    if (type === 'unnatural') {
      registry = new UnnaturalSightings();
    } else {
      registry = new AnimalSightings();
    }
    return registry.addSighting({ id, sighter, details, location });
  }

  static _uniqueIdGenerator() {
    return ++currentId;
  }
}

class Sightings {
  constructor() {
    this.sightings = [];
  }

  addSighting(sighting) {
    this.sightings.push(sighting);
    return this.replyMessage(sighting);
  }

  getSighting(id) {
    return this.sightings.find(sighting => sighting.id === id);
  }

  replyMessage(sighting) {}
}

class AnimalSightings extends Sightings {
  constructor() {
    super();
    if (AnimalSightings.exists) {
      return AnimalSightings.instance;
    }
    AnimalSightings.instance = this;
    AnimalSightings.exists = true;
    return this;
  }

  replyMessage({ id, sighter, details, location }) {
    return `Sighting No. ${id}, reported by ${sighter}, regarding ${details} seen in ${location} has been logged and will be investigated by the Middle Earth Beast Investigation Unit.`
  }
}

class UnnaturalSightings extends Sightings {
  constructor() {
    super();
    if (UnnaturalSightings.exists) {
      return UnnaturalSightings.instance;
    }
    UnnaturalSightings.instance = this;
    UnnaturalSightings.exists = true;
    return this;
  }

  replyMessage({ id, sighter, details, location }) {
    return `Sighting No. ${id}, reported by ${sighter}, regarding ${details} seen in ${location} has been logged and will be investigated by the Middle Earth Unnatural Beasts Investigation Unit.`
  }
}

const registry = new SightingRegistry();

const fox = registry.registerSighting('Pippin Took', 'animal', 'a curious looking fox', "Maggot's Farm");
const wight = registry.registerSighting('Samwise Gamgee', 'unnatural', 'a barrow wight', "The Barrow Downs");
const orc = registry.registerSighting('Legolas', 'creature', 'a roaming org', "Redhorn Pass");
const balrog = registry.registerSighting('Gandalf the Grey', 'unnatural', 'a balrog', "The Mines of Moria");
```


# behavioral patterns
These help dissimilar objects work together.

### Observer Pattern
Allows many dependent objects' (subscriber) status to be updated when a main object (publisher) changes its state. Used in `addEventListener` and in React.

```JavaScript
class Newsfeed {
  constructor() {
    this._observers = [];
  }

  subscribe(observer) {
    this._observers.push(observer);
  }

  unsubscribe(observer) {
    this._observers = this._observers.filter(obs => observer !== obs);
  }

  fire(change) {
    this._observers.forEach(observer => {
      observer.update(change);
    });
  }
}

class Observer {
  constructor(state) {
    this.state = state;
    this.initialState = state;
  }

  update(change) {
    let state = this.state;
    switch(change) {
      case 'INC':
        this.state = ++ state;
        break;
      case 'DEC':
        this.state = --state;
        break;
      default:
        this.state = this.initialState;
    }
  }
}

const huffpost = new Newsfeed();
const nytimes = new Newsfeed();

const user1 = new Observer(20);
const user2 = new Observer(13);

user1.state;

nytimes.subscribe(user1);
nytimes.subscribe(user2);
huffpost.subscribe(user1);

nytimes.fire('INC');
nytimes.fire('INC');
nytimes.fire('INC');
huffpost.fire('DEC');

user1.state;
user2.state;
```

### Model, View, Controller

In a traditional application, the model uses the Observer Pattern and pushes any changes to the View (subscriber).

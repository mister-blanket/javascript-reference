# design patterns

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


### Singleton Pattern



# structural patterns
Useful for adding functionality to an object or class without affecting other pieces in the system.

### Decorator Pattern

### Facade Pattern



# behavioral patterns
These help dissimilar objects work together.

### Observer Pattern


# architectual patterns

### Model, View, Controller

# general concepts

<!-- TOC -->
- [bubbling](#bubbling)
- [chaining](#chaining)
- [destructuring](#destructuring)
- [event loop](#event-loop)
- [hoisting](#hoisting)
- [higher order components (HOC)](#higher-order-components-hoc)
- [inheritance](#inheritance)
- [modules](#modules)
  - [export](#export)
  - [import](#import)
- [operators](#operators)
  - [`==`](#)
  - [`===`](#-1)
- [recursion](#recursion)
- [scope](#scope)
- [strict mode](#strict-mode)

<!-- TOC END -->


# bubbling
Onclick events work in nested elements and bubble most-nested to least of there are multiple onclick listeners.

`event.target` is the most deeply nested element that triggered an event. Doesn't change through the bubbling process.

`event.currentTarget` (or `this`) is the current element. Changes as bubble goes up.

`event.stopPropigation()` prevents bubbling beyond clicked element.



# chaining
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

listMovies = movie => {
  const { title, year, director } = movie; // Destructuring
  return `${title}, by ${director}, came out in ${year}.`;
};

favoriteMovies
  .filter(movie => movie.year < 1960)
  .map(listMovies)
  .join('\n');
```



# destructuring
An easier way to create local variables from passed properties.
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

listMovies = movie => {
  const { title, year, director } = movie; // Destructuring
  return `${title}, by ${director}, came out in ${year}.`;
};
```



# event loop
Messages arrive in order and are processed synchronously. Anything in a `setTimeout()` callback will be processed after all other code, even if the timeout is 0.

__stack overflow__ can happen when a recursive call gets too nested and the stack builds up. To solve this, put the recursive call in a setTimeout(), which will put each new call in the event queue rather than the stack.
```javascript
var list = readHugeList();

var nextListItem = function() {
    var item = list.pop();

    if (item) {
        // process the list item...
        setTimeout( nextListItem, 0);
    }
};
```



# hoisting
`var` is hoisted. `let` and `const` are not.

Declarations (`var x;`) are hoisted. Initializations (`x = 2;`) are not.



# higher order components (HOC)



# inheritance
JS isn't class based, it's prototype based.

__prototype chain__. Top level is null. Values are checked from the bottom up.

`hasOwnProperty()` checks only that object, and won't traverse the prototype chain.


# modules


## export
`greetings.js`
```javascript
function sayHi(name) {
  return `Hello ${name}!`
}

function sayBye(name) {
  return `Good bye ${name}.`
}

export default { sayHi, sayBye };
```

## import
`main.js`
```javascript
import { sayHi, sayBye } from './greetings'

let name = 'Michael';

sayHi(name);
```

`index.html`
```html
<script type="module" src="main.js"></script>
```



# operators

### `==`
Returns true if both expressions have the same value. Does coercion.

### `===`
Returns true if both expressions have the same type and same value.

## conditional (ternary) operator
```javascript
const ouiji = () => (Math.floor(Math.random() * 2) === 0 ? "no" : "yes");
```



# recursion
Factorial
```javascript
const factorial = n =>
  n === 0
    ? 1
    : n * factorial(n-1);
```



# scope
JS uses lexical scope, which allows scope to be determined by reading static text. Other languages (bash, C) use Dynamic Scope, which can change mid-function and is not as readable.



# strict mode
* Makes debugging easier by throwing errors, which would have otherwise passed.
* Prevents accidental globals
* Eliminates `this` being set to window

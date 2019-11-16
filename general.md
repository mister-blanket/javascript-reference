# general concepts

<!-- TOC -->
- [scope](#scope)
- [hoisting](#hoisting)
- [higher order components (HOC)](#higher-order-components-hoc)
- [inheritance](#inheritance)
- [bubbling](#bubbling)
- [event loop](#event-loop)
- [strict mode](#strict-mode)
- [operators](#operators)
  - [`==`](#)
  - [`===`](#-1)

<!-- TOC END -->

## scope
JS uses lexical scope, which allows scope to be determined by reading static text. Other languages (bash, C) use Dynamic Scope, which can change mid-function and is not as readable.


## hoisting
`var` is hoisted. `let` and `const` are not.

Declarations (`var x;`) are hoisted. Initializations (`x = 2;`) are not.

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

## strict mode
* Makes debugging easier by throwing errors, which would have otherwise passed.
* Prevents accidental globals
* Eliminates `this` being set to window


---
# operators

### `==`
Returns true if both expressions have the same value. Does coercion.

### `===`
Returns true if both expressions have the same type and same value.

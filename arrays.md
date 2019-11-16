# arrays

<!-- TOC -->
- [`filter()`](#filter)
- [`join()`](#join)
- [`map()`](#map)
- [`pop()`](#pop)
- [`push()`](#push)
- [`reverse()`](#reverse)
- [`shift()`](#shift)
- [`slice(start, end)`](#slicestart-end)
- [`splice(start, items)`](#splicestart-items)
- [`unshift()`](#unshift)

<!-- TOC END -->

### `filter()`
Creates a new array that pass through the filter requirements.
```javascript
const girlNames = ['donna', 'beverly', 'claire', 'beatrice', 'meg', 'bonita'];

const noBNames = girlNames.filter(name => name[0] !== 'b');

console.log(noBNames); // Array ["donna", "claire", "meg"]
```

### `join()`
Returns an array as a string.

```javascript
let words = ['Hello', 'there', 'comrade!'];
words.join(' '); // Hello there comrade!
```

### `map()`
Creates a new array with the results of passing all elements of another array through a function.
```JavaScript
let nouns = [sad, quick, smart];

// pass function to map
const adjectives = nouns.map(x => x + 'ly');

console.log(adjectives); // [sadly, quickly, smartly]
```

###  `pop()`
Removes the last element of an array and returns that element. _see `shift()` to remove from the beginning_.

### `push()`
Adds an element to the end of an array. _*see `unshift()` to add to beginning_.

### `reverse()`
Reverses array. Changes original array.

### `shift()`
Removes the first element of an array. * mutates the array in real time!

### `slice(start, end)`
Returns a new array with the extracted parts of the old array.
```JavaScript
let arr1 = [1, 2, 3, 5, 8]
let arr2 = arr1.slice(1,-1); // [2, 3, 5]
```

### `splice(start, items)`
Adds/removes items from an array. Mutates the original array.

array.splice(_index, howmany, item1, ..., itemZ_)
_index_ - starting position
_howmany_ - num items to remove (default = 0)
_item1, ..., itemZ_ - new items to be added to array
```javascript
arr1 = ['justin', 'angie', 'george', 'nikki'];
arr1.splice(2, 1, 'michael', 'krystle'); // ['justin', 'angie', 'michael', 'krystle', 'nikki']
```

### `unshift()`
Adds an element to the beginning of an array.

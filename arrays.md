# arrays

<!-- TOC -->
- [looping](#looping)
  - [for loop](#for-loop)
  - [for of](#for-of)
  - [`forEach()`](#foreach)
- [methods](#methods)
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

# looping

### for loop
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

for (let i = 0; i < favoriteMovies.length; i++) {
  console.log(favoriteMovies[i].title);
}
```

### for of
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

for (let movie of favoriteMovies) {
  console.log(movie.title);
}
```

### `forEach()`
Allows you to extract the definition of the function
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

let listTitle = movie => console.log(movie.title);

favoriteMovies.forEach(movie => {
  console.log(movie.title)
})

favoriteMovies.forEach(listTitle);
```

# methods

### `filter()`
Creates a new array that pass through the filter requirements.
```javascript
const girlNames = ['donna', 'beverly', 'claire', 'beatrice', 'meg', 'bonita'];

const noBNames = girlNames.filter(name => name[0] !== 'b');

console.log(noBNames); // Array ["donna", "claire", "meg"]
```

#### using objects
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

let classics = favoriteMovies.filter(movie => movie.year < 1960);
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

#### using objects
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

let years = favoriteMovies.map(movie => movie.year);
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

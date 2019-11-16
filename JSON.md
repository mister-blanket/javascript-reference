# JSON

<!-- TOC -->
- [methods](#methods)
  - [`JSON.parse()`](#jsonparse)
  - [`JSON.stringify()`](#jsonstringify)

<!-- TOC END -->

# methods

### `JSON.parse()`
JSON.stringify() converts an object into JSON format. JSON.parse() converts an JSON into an object.
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

let shippable = JSON.stringify(favoriteMovies);
JSON.parse(shippable);
```

### `JSON.stringify()`
Converts an JSON into an object.
```JavaScript
favoriteMovies = [{ title:'Apocalypse Now', year:1979, director:'Francis Ford Coppola' }, { title:'Citizen Kane', year:1941, director:'Orsen Wells' }, { title:'Metropolis', year:1927, director:'Fritz Lang'}, { title:'Modern Times', year:1936, director:'Charlie Chaplin' }, { title:'The Fellowship of the Ring', year:2001, director:'Peter Jackson' }];

JSON.stringify(favoriteMovies);
```

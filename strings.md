# strings

<!-- TOC -->
- [`slice(start, end)`](#slicestart-end)
- [`split()`](#split)

<!-- TOC END -->

### `slice(start, end)`
Returns a new string with the extracted parts of the old string.
```JavaScript
'hello friend'.slice(3,-2); //  lo frie
```

### `split()`
Splits a string into an array of substrings and returns the new array. Does not change the original string.

```javascript
let str = 'hi there, you'
str.split(''); // ['h', 'i', ' ', 't', 'h', 'e', 'r', 'e', ',', ' ', 'y', 'o', 'u']
str.split(' '); // ['hi', 'there,', 'friend']
str.split(','); // ['hi there', 'friend']
```

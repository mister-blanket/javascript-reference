# strings

<!-- TOC -->
- [printing](#printing)
  - [line break](#line-break)
- [methods](#methods)
  - [`slice(start, end)`](#slicestart-end)
  - [`split()`](#split)

<!-- TOC END -->

# printing

### line break
A backslash can be inserted in a string to create a line break.
`document.write('First line \Second line');`

# methods

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

### `trim()`
Removes whitespace from both sides of a string.
```JavaScript
let str = "    Hi, you.  "
str.trim(): // "Hi, you."
```

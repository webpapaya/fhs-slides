footer: FHS
slidenumbers: true

# Frontend Development

### Wintersemester 2020

---

## [fit] ES Next

![cover](./assets/background_11.jpg)

---

TODO: some short history

---

- arrow functions 👍
- named parameters 👍
- destructuring 👍
- map/filter/reduce/flat/flatMap/find. 👍
  - TODO: add exercise
- Rest Parameters 👍
- Default Parameters 👍
- Template Literals 👍
- optional chaining
- Classes
- Object.entries/Object.values
- .string padding
- nullish coalescing
- Proxy
- BigInt
- for in/for of

---

# Destructuring/Spread operator

---

# Arrays destructuring assignment 

- makes it possible to unpack values from arrays

```js
const [a, b] = [1, 2]
console.log(a) // 1
console.log(b) // 2
```

---

# Object destructuring assignment 

- makes it possible to unpack values from arrays

```js
const { a, b } = { a: 1, b: 2 }
console.log(a) // 1
console.log(b) // 2
```

---

# Object destructuring renaming

```js
const { a: otherA, b: otherB } = { a: 1, b: 2 }
//      ^^^^^^^^^
// rename value a to otherA

console.log(otherA) // 1
console.log(otherB) // 2
```

---

# Spread operator 

- adds the rest syntax to destructuring 

```js
const [a, b, ...rest] = [1, 2, 3, 4]
console.log(rest) // [3, 4]

const { a, b, ...rest } = { a: 1, b: 2, c: 3, d: 4 }
console.log(rest) // { c: 3, d: 4 }
```

---

# Spread operator composition

```js
const [, { b: otherB }, ...rest] = [{ a: 1 }, { b: 2 }, { c: 3 }]
// 1) ^^
// 2)     ^^^^^^^^^^
// 2)                   ^^^^^^^^

// 1) ignore the first value
// 2) extract value b and rename to otherB
// 3) get all other elements
// recommendation don't overuse nested destructuring 

console.log(otherB) // 2
console.log(rest) // [{ c: 3 }]
```

---

# Destructuring and functions

---

# Destructuring as named arguments

- can be used in functions for named arguments

```js
const myFunction = ({ a, b }) => {
  //                ^^^^^^^^
  // destructure the parameters
  return a + b
}

myFunction({ a: 1, b: 2 }) // 
myFunction({ b: 2, a: 1 })
//           ^^^^^^^^^^
// order of arguments does not matter anymore
```

---

# Destructuring of tuples

- can be used to destructure tuples as well

```javascript
const myFunction = ([ a, b ]) => {
  //                ^^^^^^^^
  // assign variable names to each value 
  return a + b
}

myFunction([ 1, 2 ])
//         ^^^^^^^
// order of arguments matters in this case
```

---

# Destructuring of tuples

- I only use tuple destructuring with `Promise.all`

```js
Promise.all([
  fetchAsPromise(`/api/currentUser`),
  fetchAsPromise(`/api/weather`)
]).then(([ currentUser, weather ]) => 
//       ^^^^^^^^^^^^^^^^^^^^^^^^
// destructure each value of the promise
  console.log(currentUser)
  console.log(weather)
})
```

---

# Destructuring of tuples

- I only use tuple destructuring with `Promise.all`

```js
const [ currentUser, weather ] = await Promise.all([
// 1)                            ^^^^^
// 2)   ^^^^^^^^^^^^^^^^^^^^
  fetchAsPromise(`/api/currentUser`),
  fetchAsPromise(`/api/weather`)
])

// 1) await the promise
// 2) assign result to variables
```


---
# Arrow functions

---
# functions declaration vs. function expressions
- functions in JavaScript are values
- can be passed to other functions [^1]


```javascript
function myFunction() { console.log('Hallo') }

setTimeout(myFunction, 200)
//         ^^^^^^^^^^
// pass my function to setTimeout
// myFunction will be called after 200ms
```

[^1]: see callbacks from previous lecture

---
# functions declaration vs. function expressions

- functions can be defined like other values in JS

```javascript
const myFunction1 = function myFunction1 () { console.log('Hallo') }
const myFunction2 = function () { console.log('Hallo') }
//                          ^^^
// name can be omitted here

setTimeout(myFunction1, 200)
```

---
# functions declaration vs. function expressions

```javascript
// function declaration
function myFunction1 () { console.log('Hallo') } 

// function expression
const myFunction2 = function () { console.log('Hallo') }
```

---
# arrow function vs. function declaration
- compact alternative to function expressions
  - can't be used in all situations
    - no binding to `this`
    - no arguments keyword
    - can't be used as constructor

```javascript
const myArrowFunction = () => { console.log('hallo') }
```

---
# arrow function

- arrow functions can have an implicit return value
  
```javascript
const myFunction = () => 1 // returns 1
const myFunction = () => { 1 } // returns undefined
const myFunction = () => ({ test: 1 }) // returns { test: 1 }
```

---
# function declarations and this
- JavaScript functions bind this when the `new` keyword is used

```javascript
function Person() {
  this.age = 0
  setInterval(function() {
    this.age++
  //^^^^ references to window as the function was not created via `new`
  }, 1000)
}
const myPerson = new Person()

// wait a couple of seconds
myPerson.age === 0
window.age === NaN
```

---
# function declarations and this


```javascript
function Person() {
  const that = this  // save this as a variable so it can be used in setInterval
  this.age = 0
  setInterval(function() {
    that.age++
  }, 1000)
}
const myPerson = new Person()

// wait a couple of seconds
myPerson.age === 3
window.age === undefined
```

---
# function declarations and this

```javascript
function Person() {
  const that = this  // save this as a variable so it can be used in setInterval
  this.age = 0
  setInterval(function() {
    that.age++
  }, 1000)
}
const myPerson = new Person()

// wait a couple of seconds
myPerson.age === 3
window.age === undefined
```

---
# function declarations and this

```javascript
function Person() {
  this.age = 0
  setInterval(() => {
    this.age++ // no need to use that hack
  }, 1000)
}
const myPerson = new Person()

// wait a couple of seconds
myPerson.age === 3
window.age === undefined
```

---
# functions in js

```javascript
function myFunction { console.log('hallo') } // function declaration
const myArrowFunction = function () { console.log('hallo') } // function expression
const myFunction = () => { console.log('hallo') } // arrow function
```

---
# function default values

- since es6 functions accept default values

```javascript
function myFunction (a = 1) {
  //                   ^^^
  // define a default value for your function
  console.log(a)
}
myFunction() // 1
myFunction(2) // 2
```

---
# function default with named arguments

```javascript
function myFunction ({ a = 1, b = 2}) {
  //                   ^^^
  // define a default value for your function
  console.log(a + b)
}
myFunction() // 3
myFunction({ a: 2 }) // 4
myFunction({ b: 3 }) // 4
myFunction({ a: 2, b: 3 }) // 5
```

---

# rest parameters

- The rest parameter syntax allows us to represent an indefinite number of arguments as an array.

```javascript
function myFunction (...values) {
  //                 ^^^
  // all arguments will be available as values
  console.log(values)
}
myFunction() // [] 
myFunction(1) // [1]
myFunction(1, 2, 3, 4, 5, 6) // [1, 2, 3, 4, 5, 6]
```

---

# Template literals

- es6 enhances strings with a completely new syntax 
  - called template literals
- they make it possible to
  - interpolate strings
  - multiline strings
  - embed expressions
  - string formatting [^2]
  - string tagging [^2]

[^2]: see https://developers.google.com/web/updates/2015/01/ES6-Template-Strings for more info
  
---
# Template literals
## String interpolation

```javascript
const university = 'FHS'
const myString = `My University is ${university}`
//               ^                              ^
// template literals are using back-ticks ``
```

---
# Template literals
## Embedded expressions

```javascript
const myUniversity = () => 'FHS'
const myString = `My University is ${myUniversity()}`
//               ^                              ^
// functions and methods can be called ``
```

---
# Template literals


```javascript
const myUniversity = () => 'FHS'
const myString = `My University is ${myUniversity()}`
//               ^                              ^
// functions and methods can be called ``
```




---
# Template literals
## Multi line strings

```javascript
const greeting1 = "Hello \
World";
//                       ^
// use backslash \ to start a new line

const greeting2 = "Hello " +
"World";
//                       ^
// use backslash + to concat 2 strings

const greeting3 = `Hello 
World`;
//                       ^
// with template literals new lines
// will be put into one line
```


---
# Array methods

---
# Array.prototype.map

- Array.prototype.map
- creates a new array populated with the result of the provided function

```javascript
const myArray = [1,2,3,4,5]
myArray.map((item) => item * 2) 
// [2, 4, 6, 8, 10]
```

---

# Array.prototype.filter

- Array.prototype.filter
- creates a new array with all elements that pass the given function

```javascript
const myArray = [1,2,3,4,5]
myArray.filter((item) => (item % 2) === 0)
// [2, 4]
```

---

# Array.prototype.reduce

- executes a reducer function on each element of the array
- results in a single value

```javascript
const myArray = [1,2,3,4,5]
const sumOfArray = myArray.reduce((result, item) => {
  return result + item
}, 0)
// 15
```

---
# Array.prototype.reduce

```javascript
const myArray = [1,2,3,4,5]
const sumOfArray = myArray.reduce((accumulator, item) => {
  //                            1) ^^^^^^^^^^^^^^^^^
  //                            2) ^^^^^^
  //                            3)              ^^^^
  // 1) reducer function
  // 2) accumulated value of previous iterations
  // 3) the current value of the iteration (1, 2, 3, ...)

  return accumulator + item
  // 4)  ^^^^^^^^^^^^^^^^^^
  // 4) return the result for the next iteration
}, 0)
// ^
// define initial value
```

---
# Array methods can be combined

```javascript
const makeSmoothie = (ingredients) => {
  return ingredients
    .filter((ingredient) => ingredient.rotten === false)
    .map((ingredient) => ingredient.slice())
    .reduce((smoothie, ingredient) => smoothie.add(ingredient), new Smoothie())
}
```

---
# Array.prototype.find

- finds the first matching element in an array

```javascript
const myArray = [1,2,3,4,5]
myArray.find(((item) => (item % 2) === 0) // 2
```

---
# Array.prototype.flat

- The find() method returns the value of the first element in the provided array 
- that satisfies the provided testing function.

```javascript
const myArray = [1,2,3,4,5]
myArray.find(((item) => (item % 2) === 0) // 2
```




---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- [Feedback Link](https://de.surveymonkey.com/r/8TW92LL)

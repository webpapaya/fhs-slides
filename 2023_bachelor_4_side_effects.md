footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true
### Side Effects


![cover, filtered](./assets/background_8.jpg)

---

### Roadmap

- Functional Programming 101
- Side Effects with React
- State Management

---
# Functional Programming 101

![cover, filtered](./assets/background_11.jpg)

---

### Functional Programming 101

- Immutability
- Pure functions
- Side Effect

---
## Functional Programming 101
### What is functional programming

> Applications developed in a functional style use side-effect free functions as their main building blocks. (Made up definition by myself)

---
## Functional Programming 101
### Why functional programming

- More testable
  - pure functions simplify testing
- Declarative APIs which are easier to reason about
- Easy concurrency because of statelessness and immutability
  - State is pushed out of the application core to the boundaries
- Simple caching
  - pure functions easy to cache

---
## Functional Programming - Immutability

> An immutable data structure is an object that doesn't allow us to change its value. (Remo H. Jansen)

---
## Functional Programming - Immutability
### Immutable objects in JS

````js
const immutableObject = Object.freeze({ test: 1 })
immutableObject.test = 10
console.log(immutableObject) // => { test: 1 }
````

---
## Functional Programming - Immutability
### Changing an immutable value

````js
const immutableObject = Object.freeze({ a: 1, b: 2 })
const updatedObject = Object.freeze({ ...immutableObject, a: 2 })
console.log(updatedObject) // => { a: 2, b: 2 }
````

---
## Functional Programming - Immutability
### Unfreeze an object

````js
const immutableObject = Object.freeze({ test: 1 })
const unfrozenCopy = { ...immutableObject }
````

---
## Functional Programming - Immutability
### Why immutability

- race conditions impossible
- state of the application is easier to reason about
- easier to test

---
## Functional Programming - Side Effects

> A side effect is a change of system state or observable interaction with the outside world that occurs during the calculation of a result. (Chris Barbour)

---
## Functional Programming - Side Effects
### Some side effects

- DB/HTTP calls
- changing the file system
- querying the DOM
- printing/logging
- accessing system state (eg. Clock, Geolocation,...)

---
## Functional Programming - Side Effects
### Where to deal with side effects

- Moved to the boundaries of the system
- Business logic stays pure functional

![inline](assets/side_effects.png)

---
## Functional Programming - Pure Functions


- A function is considered pure when:
  - for the same input it always returns the same output
  - it has no side effects
    - no mutation of non-local state

```js
const add = (a, b) => a + b
```

---
## Functional Programming - Pure Functions
### Pure or in-pure

```js
const array = [1, 2, 3, 4, 5, 6]
const fn1 = (array) => array.slice(0, 3)
const fn2 = (array) => array.splice(0, 3)
const fn3 = (array) => array.shift()
const fn4 = (array) => array.pop()
const fn5 = (array) => array.sort((a, b) => a - b)
const fn6 = (array) => [...array].sort((a, b) => a - b)
const fn7 = (array) => array.map((item) => item * 2)
const fn8 = (array) => array.forEach((item) => console.log(item))
```

---
## Functional Programming - Pure Functions
### Pure or in-pure

```js
const array = [1, 2, 3, 4, 5, 6]
const fn1 = (array) => array.slice(0, 3) // ✅
const fn2 = (array) => array.splice(0, 3) // 🚫
const fn3 = (array) => array.shift() // 🚫
const fn4 = (array) => array.pop() // 🚫
const fn5 = (array) => array.sort((a, b) => a - b) // 🚫
const fn6 = (array) => [...array].sort((a, b) => a - b) // ✅
const fn7 = (array) => array.map((item) => item * 2) // ✅
const fn8 = (array) => array.forEach((item) => console.log(item)) // 🚫
```

---
## Functional Programming - Pure Functions
### Pure or in-pure

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

---
## Functional Programming - Pure Functions
### Pure or in-pure

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge

```
---
## Functional Programming - Pure Functions
### Pure or in-pure


```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

```js
// both are not pure. const saves the pointer. config is still mutable
isAllowedToDrink(18) // true
config.minimumAge = 19
isAllowedToDrink(18) // false
```

---
## Functional Programming - Pure Functions
### Pure or inpure? 2/2

```js
const config = Object.freeze({ minimumAge: 18 })
const isAllowedToDrink = (age) => age >= config.minimumAge
```

```js
// freezing the config makes the function pure
isAllowedToDrink(18) // true
config.minimumAge = 19
isAllowedToDrink(18) // true
```

---
## Functional Programming - Summary

- Immutability
  - Object can't be changed after its creation
- Side-Effects
  - Communication with the outside world (eg. db, http, ...)
- Pure-Functions
  - returns the same output for the same input
  - simple mapping from a to b

---

# Side Effects with React

![cover](./assets/background_5.jpg)

---

# Side Effects with React
## Recap useEffect

> The Effect Hook lets you perform side effects in function components

---
# Side Effects with React
## Recap useEffect

```js
// Executed on every rerender
useEffect(() => {})

// Executed when component rendered initially
useEffect(() => {}, [])

// Executed when component rendered initially
// and when variable changes.
useEffect(() => {}, [variable])

// Cleanup when component unmounts (eg. eventHandlers, setInterval/setTimeout)
useEffect(() => {
  // do something fancy
  return () => { console.log('cleanup') }
}, [variable])
```

---

# Side Effects with React
## Recap useEffect

```js
export const MoneyTransactions = () => {
  const [moneyTransactions, setMoneyTransactions] = useState([])
  useEffect(() => {                                  // 1)
    fetch("http://localhost:3001/money-transaction") // 2)
      .then((response) => response.json())           // 3)
      .then((json) => setMoneyTransactions(json))    // 4)
  }, [])                                             // 5)

  // 1) define the useEffect hook
  // 2) make the HTTP request to the backend
  // 3) get the JSON from the response
  // 4) set the response as state
  // 5) call the effect when the component is mounted

  // ... remaining component
}
```

---

# Side Effects with React
## extract into custom hook

```js
const useHTTPEffect = (endpoint) => {
  const [response, setResponse] = useState([])
  useEffect(() => {
    fetch(endpoint)
      .then((response) => response.json())
      .then((json) => setResponse(json))
  }, [endpoint])

  return response;
}

export const MoneyTransactions = () => {
  const moneyTransactions = useHttpEffect("http://localhost:3001/money-transaction")
  const users = useHttpEffect("http://localhost:3001/user")

  // ...
}
```

---

# Task (30 min)
## Fetch and display money-transactions

- `npm i json-server -D`
- add npm script
  - `"start:server": "json-server --watch db.json --port 3001",`
- add [db.json](https://gist.github.com/webpapaya/c419f8d3fa1ba9649c5afdb89e695b76)
- fetch/display `money-transactions` and `users`

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>

[^1]: whole code <https://gist.github.com/webpapaya/2a751dd740ee932bd9f348d780edc518>


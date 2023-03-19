footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Functional Programming and State Managment in React

---

# Functional Programming

---

## Functional Programming
### What is functional programming

> Applications developed in a functional style use side-effect free functions as their main building blocks. (Made up definition by myself)

---

## Functional Programming
### FP vs. OOP

> Object-oriented programming makes code understandable by encapsulating moving parts. Functional programming makes code understandable by minimizing moving parts. (Michael Feathers)

---

## Functional Programming
### Why functional programming

- More testable
  - pure functions simplify testing
- Declarative APIs which are easier to reason about
- Easy concurrency because of statelessness and immutability
  - State is pushed out of the application core to the boundaries
- Simple caching
  - pure functions easy to cache (we'll see this in an example)

---

## Functional Programming
### Immutability

> An immutable data structure is an object that doesn't allow us to change its value. (Remo H. Jansen)

---

## Functional Programming
### Immutable objects in JS

````js
const immutableObject = Object.freeze({ test: 1 })
immutableObject.test = 10
console.log(immutableObject) // => { test: 1 }
````

---

## Functional Programming
### Changing an immutable value

````js
const immutableObject = Object.freeze({ a: 1, b: 2 })
const updatedObject = Object.freeze({ ...immutableObject, a: 2 })
console.log(updatedObject) // => { a: 2, b: 2 }
````

---

## Functional Programming
### Unfreeze an object

````js
const immutableObject = Object.freeze({ test: 1 })
const unfrozenCopy = { ...immutableObject }
````

---

## Functional Programming
### Object.freeze is mutable

````js
const object = { test: 1 }
Object.freeze(object)
object.test = 10
console.log(object) // => { test: 1 }
````

---

## Functional Programming
### Why immutability

- race conditions impossible
- state of the application is easier to reason about
- easier to test

---

## Functional Programming
### Mutable bug

```js
const users = []
const loadUsers = async () => {
  const result = await fetchUsers('/users')
  users.push(...result)
  return users
}

loadUsers()
loadUsers()
```

---

## Functional Programming
### Immutable version

```js
const loadUsers = () => {
  return fetchUsers('/users');
}

const result1 = await loadUsers();
const result2 = await loadUsers();
```

---

## Functional Programming
### Higher Order Functions

> A higher order function is a function that returns a function.

---

## Functional Programming
### Higher Order Functions

```js
const buildCreateUser = (dbAdapter) => {
  return (user) => {
    if (!isValid(user)) { throw new Error('User Invalid') }
    return dbAdapter.create(user)
  }
}
const createUserInPG = buildCreateUser(postgresAdapter)
const createUserInMemory = buildCreateUser(inMemoryAdapter)
```

---

# Global State Management


![cover, filtered](./assets/background_9.jpg)

---

## Application State
### What is application state

> An application's state is roughly the entire contents of its memory. ([sarnold](https://stackoverflow.com/a/8102731))

---

## Application State
### State in Redux terms

> Every bit of information the application needs in order to render.


---

## Application State
### React component tree

![inline](assets/react_component_tree.png)

---

## Application State
### Storing state in components

![inline](assets/local_state_tree.png)

---

## Application State
### Storing state in components

- Pros
  - Components are independent
    - eg. "Navigation" doesn't know about "User Update"
- Cons
  - User data needs to be fetched multiple times
  - If UserUpdate component changes name of user
    - Navigation needs to refetch user data

---

## Application State
### Storing state in the root component

![inline](assets/global_state_tree.png)

---

## Application State
### Storing state in the root component

- Pros
  - User data could be fetched only once
  - If UserUpdate component changes name of user
    - navigation component is automatically updated
- Cons
  - State needs to be passed down to every component
  - (Root component contains all state logic)

---

## Application State
### Storing state in the root component

![inline](assets/twitter_component_tree.gif)

---

## Application State
### Storing state globally

![inline](assets/connected_tree.png)

---

## Application State
### Storing state globally

- Global state which acts like local state
- Pros:
  - Components are independent
    - eg. Navigation doesn't know about UserUpdate
  - State changes are synchronised with the whole app
  - State doesn't need to be passed down the tree

---

## Custom State Management

---

## Framework agnostic state MGMT
### Creating a library agnostic store

- Create a library agnostic store to be used by any framework

```ts
// Interface
type CreateStore = <T>(stateFactory: () => T) => {
  set: (updateFn: (state: T) => T) => unknown
  get: () => T
}
```

---
## Framework agnostic state MGMT
### Task 

- Create an implementation for CreateStore type
- Add unit tests to your implementation
- Usage:


```ts
const store = createStore(() => { someValue: 1 })

store.get() // 1
store.set((current) => ({ someValue: current.someValue + 1}))
store.get() // 2
```


--- 
## React State MGMT Adapter

- Interface for wrapping the agnostic state management

```ts
type UseReactStore<T> = () => [
  T,
  (updateFn: (state: T) => T) => unknown
]

type CreateReactStore = <T>(stateFactory: () => T) => UseReactStore<T>
//                                                    ^^^^^^^^^^^^^^^^
// higher order function which returns a hook
```

--- 
## React State MGMT Adapter
## Task 

- create an implementation for `CreateReactStore`
- Usage:

```ts
const useMyStore = createReactStore(() => ({ someValue: 1 }))
//    ^^^^^^^^^^
// useMyStore is defined globally/outside of the react context

const MyComponet = () => {
   const [myState, setMyState] = useMyStore()
   // ...
}
```

---

## Build adapter for React
### Issue 

- Did you encounter any issues?

---

## Build adapter for React
### Issue 

- state is not updated in components 

---

## Build adapter for React
### useSyncExternalStore

> useSyncExternalStore is a React Hook that lets you subscribe to an external store.

```ts
const todos = useSyncExternalStore(store.subscribe, store.getSnapshot);
// 1)                              ^^^^^^^^^^^^^^^
// 2)                                                ^^^^^^^^^^^^^^^^

// 1) callback function which adds the current component to the list of components 
//    to be updated in case the state changes. (similar to observer pattern)
// 2) callback function which returns an immutable snapshot of the current state.
//    This function is used to get the state into the react component.
```

---
## Build adapter for React
### subscribe function

- callback function which adds/removes components to be notified on state changes

```ts
type Listener = () => unknown
const createMyStore = () => {
  // ...
  const listeners: Listener[] = []

  return () => {
    const subscribe = (listener: Listener) => {
      listeners.push(listener)
// 1)           ^^^^^^^^^^^^^^
      return () => listeners.splice(listeners.indexOf(listener), 1)
// 2)        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      }
  }
}
// 1) adds listener to be called when the store changes
// 2) removes listener when component gets unmounted
```

---
## Build adapter for React
### notify components about changes

- notify react about state changes

```ts

const createMyStore = <T>() => {
  const listeners: Listener[] = []
  const emitChanges = () => listeners.forEach((listener) => listener())
//                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
// notify components about state changes
  
  return () => {
    const set = (updateFn: (state: T) => T) => {
        store.set(updateFn)
        emitChanges()
    }
    // ...
  }
}
```

---
## Build adapter for React
### getSnapshot function

- callback function which adds/removes components to be notified on state changes

```ts

const createMyStore = () => {
// ...

  const getSnapshot = () => store.get()
//                    ^^^^^^^^^^^^^^^^^
// function which returns the current snapshot of the store                            
}
```

---
## Build adapter for React
### combine callback functions

```ts
const createMyStore = () => {
// ... listeners, emitChanges
  return () => {
    // ...
    const state = useSyncExternalStore(subscribe, getSnapshot);

    return [
      state,
      set
    ]
  }
}
```

--- 
## Build adapter for React
### Task

- Add `useSyncExternalStore` to your state management solution
- Verify that connected components are updated


---
## Functional Programming
### Pure Functions

- A function is considered pure when:
  - for the same input it always returns the same output
  - it has no side effects
    - no mutation of non-local state,

```js
const add = (a, b) => a + b
```

---

## Functional Programming
### Attributes of pure functions

- They are idempotent
- They offer referential transparency
  - calls to this function can be replaced by the value without changing the programs behaviour
- They can be memoized (or cached)
- They can be lazy
- They can be tested more easy

---

## Functional Programming
### Pure or inpure? 1/3

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

## Functional Programming
### Pure or inpure? 2/3

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

---

## Pure or inpure? 3/3

```js
const isIndexPage = () => window.location.pathname === '/';
const isIndexPage = (pathname) => pathname === '/';
```

---

## Functional Programming
### Memoization

> `Memoizing' a function makes it faster by trading space for time. It does this by caching the return values of the function in a table. (<https://metacpan.org/pod/Memoize)>

---

## Pure functions recap

- A pure function returns for the same input the same output
- simple mapping from value a to value b

![inline](assets/pure_function.png)


---

## Functional Programming
### Task

- Memoize the fibonacci sequence
- Compare results with and without memoize

```js
const memoize = () => {} // TODO: implement me
const fibonacci = memoize((num) => {
  if (num <= 1) return 1
  return fibonacci(num - 1) + fibonacci(num - 2)
})
```

- helper to measure time <https://bit.ly/2UOFgAE>

---

## Functional Programming
### Possible Implementation

- Will be added after the lecture

<!--
```js
const memoize = (fn) => {
  const cache = {}
  return (...args) => {
    const key = JSON.stringify(args)
    cache[key] = cache[key] || fn(...args)
    return cache[key]
  }
}
``` -->

---

## Functional Programming
### React and Memoization

![inline](assets/tree.png)

---

## Functional Programming
### Where is memoization used?


```jsx
import React, {useMemo} from 'react'

const MyComponent = ({ users }) => {
  const activeUsers = useMemo(
//                    ^^^^^^^^
// add the useMemo hook
    () => users.map((user) => !user.inactive), 
//  ^^
// define a callback so react can decide when to filter
    [users])
//  ^^^^^^
// define the "dependencies" 
// = the filter function should be executed
//   when users array changes
  
  return (
    <Users users={activeUsers} />
  )
}
```

---

## Functional Programming in React
### How to use `useMemo` in global State MGMT

- Extracting/Transforming state from our state management
  - new value won't be recalculated on every rerender

```ts
type UseReactStore<T> = <O>(transform: (state: T) => O) => [
//                         ^^^^^^^^^^^^^^^^^^^   
// add possibility to add a transform function
  O, // Instead of T the transformed type O is returned
  (updateFn: (state: T) => T) => unknown
]

type CreateReactStore = <T>(stateFactory: () => T) => UseReactStore<T>
```

---

## Functional Programming
### Task

- Add transformer to your state mgmt solution
  - make sure the value is not recalculated on every rerender (use Reacts `useMemo`)
  - value should only be recalculated when state changes
- Throw/log error when state update is not immutable in generic store
  - `previousState` !== `updatedState`

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>


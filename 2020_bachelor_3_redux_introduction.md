### State Management with Redux

## (MMT-B2018)

---

### Roadmap

- Recap of homework
- Functional Programming 101
- State Management with Redux

---

### Recap of Homework

----

### Template literals in JSX

```tsx
<label className={`${styles.label}`}>{labelText}</label>
//              ^^^^^^^^^^^^^^^^^^^^
// Only necessary when you'd like to interpolate strings. The following is enough
<label className={styles.label}>{labelText}</label>
```

----

### Routing I

- Regular links instead of react router links
- Router Links have a couple of additional feature
  - activeClassName
  - no hard reload of pages
  - ...

```ts
import { Link } from "react-router-dom";

<a href="#">Link</a> // 🚫
<Link to="/whatever">Link</Link> // ✅
```

----

### Routing II

- Storybook breaks after routing added
- Error: `You should not use <Link> outside a <Router>`
- Wrap the component in a router
- Or use decorator

```js
// .storybook/config.js
import { configure, addDecorator } from '@storybook/react'
import StoryRouter from 'storybook-react-router'

addDecorator(StoryRouter())
```

----

### import mock data in components

- Try to import data only in the .stories.js file
- Redux will pass the data from a server to the components
  - in storybook we don't want any server interaction

![redux overview](assets/pass_mocked_data.png)

----

### DRY components

- DRY (don't repeat yourself)
- SignIn/SignUp combined into an AuthenticationForm component
  - As those are organisms repeating yourself is fine here

----

### Unnecessary state

```js
const debts = [/* ... */]
const user = [/* ... */]

const List = () => {
    const [list, setList] = useState(debts);
    const [users, setUser] = useState(user);
    // ...
```

----

### Deleting Code is fine

- Nobody wanted to delete my button 😊
  - If some code is not used, just remove it
  - it's in git anyways

---

### Functional Programming 101

- Immutability
- Pure functions
- Side Effect

---

## What is functional programming

> Applications developed in a functional style use side-effect free functions as their main building blocks. (Made up definition by myself)

---

## Why functional programming

- More testable
  - pure functions simplify testing
- Declarative APIs which are easier to reason about
- Easy concurrency because of statelessness and immutability
  - State is pushed out of the application core to the boundaries
- Simple caching
  - pure functions easy to cache

---

## Immutability

> An immutable data structure is an object that doesn't allow us to change its value. (Remo H. Jansen)

----

## Immutable objects in JS

````js
const immutableObject = Object.freeze({ test: 1 })
immutableObject.test = 10
console.log(immutableObject) // => { test: 1 }
````

----

## Changing an immutable value

````js
const immutableObject = Object.freeze({ a: 1, b: 2 })
const updatedObject = Object.freeze({ ...immutableObject, a: 2 })
console.log(updatedObject) // => { a: 2, b: 2 }
````

----

## Unfreeze an object

````js
const immutableObject = Object.freeze({ test: 1 })
const unfrozenCopy = { ...immutableObject }
````

----

## Why immutability

- race conditions impossible
- state of the application is easier to reason about
- easier to test

---

## Side-Effects

> A side effect is a change of system state or observable interaction with the outside world that occurs during the calculation of a result. (Chris Barbour)

----

## Some side effects

- DB/HTTP calls
- changing the file system
- querying the DOM
- printing/logging
- accessing system state (eg. Clock, Geolocation,...)

----

## Where to deal with side effects

- Moved to the boundaries of the system
- Business logic stays pure functional

![Side effects](assets/side_effects.png)

---

## Pure Functions

- A function is considered pure when:
  - for the same input it always returns the same output
  - it has no side effects
    - no mutation of non-local state

```js
const add = (a, b) => a + b
```

----

## Pure or in-pure

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

----

## Pure or in-pure

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

----

## Pure or in-pure

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

----

## Pure or in-pure

```js
const config = { minimumAge: 18 }
const isAllowedToDrink = (age) => age >= config.minimumAge
```

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

----

## Pure or inpure? 2/2

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

## Summary FP

- Immutability
  - Object can't be changed after its creation
- Side-Effects
  - Communication with the outside world (eg. db, http, ...)
- Pure-Functions
  - returns the same output for the same input
  - simple mapping from a to b

---

### State Management with Redux

----

### What is application state

> An application's state is roughly the entire contents of its memory. ([sarnold](https://stackoverflow.com/a/8102731))

----

### State in Redux terms

> Every bit of information the application needs in order to render.

----

### What information do we need to render this page

![app wireframe](assets/app_wireframe.png)

----

### What information do we need to render this page

| Question?                                  | State Name           |
|--------------------------------------------|----------------------|
| Is the user authenticated?                 | authenticationStatus |
| Is a form already filled with values?      | formValues           |
| Is the input field hovered/focused/filled? | inputStatus          |
| Am I owning money to somebody?             | moneyTransactions    |
| Is somebody owning me some money?          | moneyTransactions    |
| Which users can I owe some money?          | users                |
| Which components should be rendered?       | url                  |

----

### Categorising different types

- Relevant for other parts of the application?
  - add to global state
- Irrelevant for other parts of the application?
  - use component state (useState or setState)
  - also known as UI State

----

### Global/Local/URL

| State Name           | State Type |
|----------------------|------------|
| authenticationStatus |            |
| moneyTransactions    |            |
| users                |            |
| formValues           |            |
| inputStatus          |            |
| url                  |            |

----

### Global/Local/URL

| State Name           | State Type |
|----------------------|------------|
| authenticationStatus | global     |
| moneyTransactions    | global     |
| users                | global     |
| formValues           | local      |
| inputStatus          | local      |
| url                  | url        |

----

### Global State

- relevant for other components
- could be seen as a client side database
  - or a cached version of the server data
- domain object should be stored here
  - eg. users, money transactions, authentication token

----

### UI State

- irrelevant for other parts of the application
  - or state which shouldn't be shared with others
- What to store in UI state?
  - Form states
  - visual enhancements

----

### URL State

- defines which set of components should be rendered
- persists on page reloads
- What to store in URL state?
  - the current route
  - the current page of a paginated list

---

### React component tree

![Component Tree](assets/react_component_tree.png)

----

### Storing state in components

![Local state tree](assets/local_state_tree.png)

----

### Storing state in components

- Pros
  - Components are independent
    - eg. "Navigation" doesn't know about "User Update"
- Cons
  - User data needs to be fetched multiple times
  - If UserUpdate component changes name of user
    - Navigation needs to refetch user data

----

### Storing state in the root component

![Global state tree](assets/global_state_tree.png)

----

### Storing state in the root component

- Pros
  - User data could be fetched only once
  - If UserUpdate component changes name of user
    - navigation component is automatically updated
- Cons
  - State needs to be passed down to every component
  - (Root component contains all state logic)

----

### Storing state in the root component

![twitter component tree](assets/twitter_component_tree.gif)

----

### Storing state in redux

![connected tree](assets/connected_tree.png)

----

### Storing state in redux

- Global state which acts like local state
- Pros:
  - Components are independent
    - eg. Navigation doesn't know about UserUpdate
  - State changes are synchronised with the whole app
  - State doesn't need to be passed down the tree
- Cons:
  - "Complex" architecture for small apps

---

### Redux

![redux overview](assets/redux_overview.png)

----

### Why Redux

- Managing state in react can be challenging
  - How to synchronize state between distant UI parts
- Redux provides a predictable way to manage state
- State can only be changed by dispatching an action
- Each action might change the previous state to a new updated state
- Works with react, vue, angular, ...

---

### Actions

![Redux Actions](assets/redux_action_highlight.png)

----

### Actions

> Something happened in the app which might be interesting.

----

### Actions

- An action is data from the application which might be relevant for the store
- Information is sent to the store via store.dispatch

```js
const signInAction = {
  type: 'signIn',
  payload: {
    username: 'peter',
    password: 'the clam'
  }
}

store.dispatch(signInAction)
```

----

### Action Creators

- A functions which creates actions
- With redux-thunk action creators can dispatch itself
  - This is where side effects are handled

```js
const actionCreator = () => (dispatch) => {
  dispatch({ type: 'action1', payload: {} })
  dispatch({ type: 'action2', payload: { something: 'random' } })
  dispatch({ type: 'action3', payload: { something: 'random' } })
  // ...
}

store.dispatch(actionCreator())
```

----

#### Action creator with params

```js
const actionCreatorWithData = ({ username, password }) => (dispatch) => {
  dispatch({ type: 'action1', payload: { username, password } })
  dispatch({ type: 'action2/success', payload: { something: 'random' } })
}

store.dispatch(signInAction({ username: 'Mike', password: '1234' }))
```

----

### Async action creators

```js
const createMoneyTransaction = ({ creditorId, debitorId, amount }) =>
  async (dispatch) => {
    dispatch({ type: 'createMoneyTransaction/initiated', payload: {} })
    try {
      const moneyTransaction = await fetch('/money-transaction/', {
        creditorId,
        debitorId,
        amount
      })
      dispatch({
        type: 'createMoneyTransaction/success',
        payload: moneyTransaction
      })
    } catch (e) {
      dispatch({ type: 'createMoneyTransaction/error', payload: e })
    };
  }

store.dispatch(createMoneyTransaction({ creditorId: 1, debitorId: 2, amount: 10.3 }))
```

---

### Reducers

![redux overview](assets/redux_reducer_highlight.png)

----

### Reducers

> Reducers specify how the application's state changes in response to actions sent to the store. ([Source](https://redux.js.org/basics/reducers))

----

### Reducers

- Specify how state changes in response to actions.
- Pure function
  - input appState and action
  - output next application state

```js
const initialState = {}
const reducer = (previousState = initialState, action) => {
  // do something with the state
  return nextState
}
```

----

### Reducers

```js
const initialState = []
const moneyTransactionReducer = (previousState = initialState, action) => {
  switch (action.type) {
    case 'createMoneyTransaction/success':
      return [...previousState, action.payload]
    case 'reset':
      return initialState
    default:
      return previousState
  };
}
```

---

### Container components

![redux overview](assets/redux_container_highlight.png)

----

### Container components

- Glue between react and redux
- Provides data from the global store to the components
- Provides "callbacks" to trigger actions

----

### Container Components

```js
import { createMoneyTransaction } from '../action-creators/money-transactions'
const mapStateToProps = (state, props) => {
  return {
    moneyTransactions: state.moneyTransactions
  }
}

const mapDispatchToProps = (dispatch, props) => {
  return {
    createMoneyTransaction: (payload) =>
      dispatch(createMoneyTransaction(payload))
  }
}

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(MoneyTransactionList)
```

----

### mapStateToProps

- extract data from the store and provides it to a component
- data filtering can be done here
- `function mapStateToProps(state, ownProps?)`
  - state -> the entire application state
  - ownProps -> properties which are passed from other components
- [Docs](https://react-redux.js.org/using-react-redux/connect-mapstate)

----

### mapDispatchToProps

- binds actions with the store and provides those actions to a component
- `function mapStateToProps(dispatch, ownProps?)`
  - dispatch -> the stores dispatch function
  - ownProps -> properties which are passed from other components
- [Docs](https://react-redux.js.org/using-react-redux/connect-mapdispatch)

----

### Example from lecture

[Example](https://gist.github.com/webpapaya/acd14d815bd4090502aa39fb54c3fa35)

Might contain syntax error =)

---

## Task 1 (Actions)

- Download
  - [React dev tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
  - [Redux dev tools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=de)
- in src/store.js
  - add `window.store = store`;
- `npm run start:app`
- go to localhost:3000
- open dev tools/redux tab
- `store.dispatch({ type: 'signIn', payload: { name: 'name', password: 'password' } })`

---

## Task 2 (Action creator)

- Create an action creator which calls a fake api
- eg: const user = await fetch({ id: 1, name: 'sepp' })

----

## Task 3 (Reducer)

- Add a userReducer reducer
  - entry point: `src/reducer/index.js`
  - listen to 'fetchUser/success'
    - try to populate the redux store with a new user

----

## Task 4 (Container)

- Try to connect your moneyTransactionCreate dropdown with users from the store

----

## Task 5 (connect to backend)

- Try to connect fetchUsers action creator to your rails backend

---

# Further Links

- [Redux Tutorial](https://redux.js.org/basics/basic-tutorial)
- [Mostly adequate guide to FP](https://github.com/MostlyAdequate/)
- [Hands-On Functional Programming with TypeScript](https://www.amazon.com/Hands-Functional-Programming-TypeScript-applications/dp/1788831438)
- [Immutable Data Structures](https://hackernoon.com/how-immutable-data-structures-e-g-immutable-js-are-optimized-using-structural-sharing-e4424a866d56)

---

# Feedback/Questions

- <https://de.surveymonkey.com/r/J6693VN>
- tmayrhofer.lba@fh-salzburg.ac.at

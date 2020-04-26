### State Management with Redux

## (MMT-B2018)

---

### State Management with Redux

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

----

## Homework until 4.5. 06:00 in the morning
- connect Money-Transacton-List to backend
  - display Users/Money-Transactions
- connect Create-Money-Transacton to backend
  - read users and display them in the dropdown
  - on submit the Transaction list should update
  - handle both cases:
    - somebody owes me
    - i owe somebody
- please make sure your deployed backend works
  - otherwise I can't check the exercise

----

# Feedback/Questions

- <https://de.surveymonkey.com/r/J6693VN>
- tmayrhofer.lba@fh-salzburg.ac.at

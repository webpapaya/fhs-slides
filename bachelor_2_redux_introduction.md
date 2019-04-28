# State Management with Redux
## (MMT-B2017)

---

# Recap of Homework

----

TODO: add reviews/pitfalls

---

# React Hooks

- makes it possible to use state in functional components
- prefixed with `use`
- Can't be called inside loops, conditions or nested functions
- Can only be called from a React Component


----
# useState

```js
const SimpleForm = ({ onSubmit }) => {
  const [firstName, setFirstName] = useState("");
  return (
    <input
      type="text"
      name="firstName"
      value={firstName}
      onChange={evt => setFirstName(evt.target.value)}
    />
  );
};
```
----


# Why Redux

![app wireframe](assets/app_wireframe.png)

----
# Why Redux

![react component tree](assets/react_component_tree.png)

----
# Why Redux

![twitter component tree](assets/twitter_component_structure.gif)

----

# Why Redux

- Distant parts of the application need to communicate

![component tree](assets/react_component_tree.png)

---

# Architecture of Redux

![redux overview](assets/redux_overview.png)

---

# Actions

----

> Something happened in the app which might be interresting.

----

- Payloads of information which send data from the application to the store
- Sent to the store via store.dispatch

```js
const signInAction = {
  type: "signIn",
  payload: {
    username: "peter",
    password: "the clam"
  }
}

store.dispatch(signInAction);
```
----

# Action Creators

- A functions which creates actions
- With redux-thunk action creators can dispatch itself
  - We might discuss redux-thunk next time

```js
const signIn = ({ username, password }) => (dispatch) => {
  return fetch('/sign-in/', { username, password });
    .then(({ token }) => dispatch({ type: 'signIn/success', payload: { token }}))
    .catch(() => dispatch({ type: 'signIn/error', payload: {}}));
  });
};

store.dispatch(signInAction);
```

---

# Reducers

- Specify how state changes in response to actions.

```js
const reducer = (previousState, action) => {
  // some magic
  return nextState;
}
```

----

```js
const initialState = { token: null };
const reducer = (previousState = initialState, action) => {
  switch(action.type) {
    case 'signIn/success':
      return { ...state, token: action.payload.token }
    default:
      return previousState;
  }
}
```
----


---
# Download
- [React dev tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
- [Redux dev tools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=de)




# Store

----





# Action Creators

# Containers

# Reducers

# Store


---





---

# Types of State

----

# Global/Redux State

- Shared accross the whole application
- "Cached" version of the database

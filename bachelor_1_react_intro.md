# Introduction to React

---

## React

- Component based library to build composable UIs
- OpenSourced in 2013
- Implemented and Maintained by Facebook
- Learn once, write anywhere
  - React-Native
  - React-Native-Desktop
  - React-Native-Windows
  - React-VR
---

## React Components

- Main Building block of a React App
  - Describe the look and feel of one section in the UI

```js
const Button = () => {
  return (
    <button type="button">
      A button
    </button>
  );
}

// Usage
React.renderComponent(<Button />, document.body);
```

----

## JSX

- JavaScript XML
  - extension to write XML in JS
- Allows to combine data preparation with render logic

```js
const Button = () => {
  return (
    <button type="button">
      A button
    </button>
  );
}
```

----

## React without JSX

- React can be used without JSX

```js
const Button = () => {
  return React.createElement(
    'button',
    {type: 'button'},
    'A button'
  );
}
```

---
### Loop over arrays

```js
const Button = ({ users }) => {
  return (
    <ul type="button">
      { users.map((user) => {
        return (<li key={user.id}>{user.name}</li>)
      }) }
    </ul>
  );
}
```

----

### key property in loops

- Is required when interating over lists
- Helps react to decide if an element needs to be rerendered
- [Video explanation](https://www.youtube.com/watch?v=kFy5dpzdFsM)
- [Detailed explanation](https://dev.to/jtonzing/the-significance-of-react-keys---a-visual-explanation--56l7)


---

## Which components do you see?

![app](assets/sign_in_wireframe.png)

----

![app](assets/app_wireframe.png)

---

# Building the first react component  <!-- .element: class="color--white" -->

<!-- .slide: data-background="./assets/coding.gif" -->


---

## Component composition

- Components can be nested and composed together

![app](assets/tree.png)

----


## React props

- Possibility to customize components
  - Can be seen as component configuration
- Props are passed to the component
  - A component at a lower level of the tree can't modify given props directly


```js
const Button = ({ children, disabled = false }) => {
  return (
    <button disabled={disabled} className="button">
      { children }
    </button>
  );
}

const usage = <Button disabled>A button</Button>;
```

----

# React State (with Hooks)

- State allows components to by dynamic/interactive

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

# React State (without hooks)

```js
class SimpleForm extends React.Component {
  state = { username: '' };
  render() {
    return (
      <input
        type="text"
        name="firstName"
        value={this.state.userName}
        onChange={evt => this.setState({ username: evt.target.value }) }
      />
    );
  }
}
```


----

## State vs. Props

| _props_ | _state_ |
--- | --- | ---
Can get initial value from parent Component? | Yes | Yes
Can be changed by parent Component? | Yes | No
Can set default values inside Component?* | Yes | Yes
Can change inside Component? | No | Yes
Can set initial value for child Components? | Yes | Yes
Can change in child Components? | Yes | No

[source](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)

---

## Task

- Fork/clone the following https://github.com/webpapaya/fhs-react-redux-starter-kit
- npm install
- npm run start:storybook
- Reuse/Adapt existing Button
- Create a UserSignIn Form Component
  - Username
  - Password
- If you're done help others

---

## Homework

- Build the following components in Storybook
  - UserSignIn -> onSubmit => { username, password }
  - UserSignUp -> onSubmit => { username, password }
  - MoneyTransactionCreate
    - users => { id, name }
    - onSubmit => { debitorId, creditorId, amount }

  - MoneyTransactionList (Lists all Money Transactions)
    - moneyTransactions
    - onMoneyTransactionPaid => { id, paidAt: (new Date()).toISOSTring() }

- You probably need the following core components
  - `<TextInput {...} />`
  - `<DecimalInput {...} />`
  - `<SelectInput {...} />`
  - `<Button {...} />`
  - ...

- Allowed to use CSS Frameworks
- Not allowed to use Component Libraries
- You can use as a starting point https://github.com/webpapaya/fhs-react-redux-starter-kit
- Mock Data for the API https://gist.github.com/webpapaya/ba25ac39138b6f6a50a04f2b0820cf65

----

![sign_in](assets/sign_in_wireframe.png)

----

![sign_up](assets/sign_up_wireframe.png)

----

![app](assets/app_wireframe.png)

---

## Tools/Resources

- [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
- [React Docs](https://reactjs.org)
  - [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)
- [Overreacted](https://overreacted.io/)

---

## Feedback

https://de.surveymonkey.com/r/J6693VN
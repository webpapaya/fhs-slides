# Introduction to React

## (MMT-B2018)

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

## Components

> Components let you split the UI into independent, reusable pieces.
[React Docs](https://reactjs.org/docs/components-and-props.html)

---

## React Components

- Main Building block of a React App
  - Describe the look and feel of one section in the UI

```js
const Button = () => {
  return (
    <button type='button'>
      A button
    </button>
  )
}

// Usage
React.renderComponent(<Button />, document.body)
```

----

## JSX

- JavaScript XML
  - extension to write XML in JS
- Allows to combine data preparation with render logic

```js
const Button = () => {
  return (
    <button type='button'>
      A button
    </button>
  )
}
```

----

## React without JSX

- React can be used without JSX

```js
const Button = () => {
  return React.createElement(
    'button',
    { type: 'button' },
    'A button'
  )
}
```

---

## Which components do you see

![app](assets/sign_in_wireframe.png)

----

![app](assets/app_wireframe.png)

---

## Types of components

- Possible conceptual structure for components
  - `components/`
    - could be reused in other applications
    - don't have domain logic (eg. Button)
  - `container/<container_name>/organism/`
    - contain domain logic
    - can't be reused in a different application (eg. MoneyTransactionList/SignUp)

---

# Building the first react component  <!-- .element: class="color--white" -->

<!-- .slide: data-background="./assets/coding.gif" -->

---

### Embedding expressions

```js
const CurrentTime = () => {
  return (
    <h1>
      {(new Date()).toLocaleDateString()}
    </h1>
  )
}
```

----

## Conditional rendering

```js
const CurrentTime = () => {
  // ...
  return (
    <h1>
      {isToday
        ? 'Today'
        : 'Not Today' }
    </h1>
  )
}
```

----

### Loop over arrays

```js
const UserList = ({ users }) => {
  return (
    <ul>
      {users.map((user) => {
        return (<li key={user.id}>{user.name}</li>)
      })}
    </ul>
  )
}
```

----

### key property in loops

- Is required when interating over lists
- Helps react to decide if an element needs to be rerendered
- [Video explanation](https://www.youtube.com/watch?v=kFy5dpzdFsM)
- [Detailed explanation](https://dev.to/jtonzing/the-significance-of-react-keys---a-visual-explanation--56l7)

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
    <button disabled={disabled} className='button'>
      {children}
    </button>
  )
}

const usage = <Button disabled>A button</Button>
```

----

# React State (with Hooks)

- State allows components to by dynamic/interactive

```js
const SimpleForm = ({ onSubmit }) => {
  const [firstName, setFirstName] = useState('')
  return (
    <input
      type='text'
      name='firstName'
      value={firstName}
      onChange={evt => setFirstName(evt.target.value)}
    />
  )
}
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

### Fragments

- Groups a list of children without adding a dom element

```js
const AComponent = () => {
  return (
    <>
      <label>An input</label>
      <input type="text" />
    </>
  )
}
```

----

## Keyed Fragments

- Same as fragment but a key can be provided (eg.: definition list)

```js
const AComponent = ({ items }) => {
  return (
    <dl>
      {items.map(item => (
        // Without the `key`, React will fire a key warning
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  )
}
```

---

## Task

- Fork/clone the following <https://github.com/webpapaya/fhs-react-redux-starter-kit>
- npm install
- npm run start:storybook
- Reuse/Adapt existing Button
- Create a UserSignIn Form Component
  - Username
  - Password
- If you're done help others

---

## Tools/Resources

- [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
- [React Docs](https://reactjs.org)
  - [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)
- [Overreacted](https://overreacted.io/)

---

## Feedback/Questions

- <https://de.surveymonkey.com/r/J6693VN>
- tmayrhofer.lba@fh-salzburg.ac.at

slidenumbers: true

# React for BE Devs

## React

---

# React

- Component based library to build composable UIs
- OpenSourced in 2013
- Implemented and Maintained by Facebook
- Learn once, write anywhere
  - React-Native
  - React-Native-Desktop
  - React-Native-Windows
  - React-VR

![right, filtered](./assets/background_5.jpg)

---

## Components

> Components let you split the UI into independent, reusable pieces.[^1]

- Main Building block of a React App
  - Describe the look and feel of one section in the UI

![right, filtered](./assets/background_6.jpg)

---

## React Components

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

---

## React Class Components

- Alternative syntax for components

```js
class Button extends React.Component {
  render() {
    return (
      <button type='button'>
        A button
      </button>
    )
  }
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

![app, inline](assets/sign_in_wireframe.png)

----

## Which components do you see

![app, inline](assets/app_wireframe.png)

---

# Building the first react component

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

### Embedding expressions

```js
const FagoMenu = () => {
  return (
    <a href={`/menu/${(new Date()).toLocaleDateString()}`}>
      Go to todays menu
    </a>
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
        : 'Not Today'}
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
      {isToday && 'Today'}
      {!isToday && 'Not today'}
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

![app, inline](assets/tree.png)

----

## React props

- Possibility to customize components
  - Can be seen as component configuration
- Props are passed to the component
  - A component at a lower level of the tree can't modify given props directly

---

## React props

```js
const Button = ({ children, disabled = false }) => {
  //              ^^^^^^^^^^^^^^^^^^^^^^^^^^
  // props which are passed to the component

  return (
    <button disabled={disabled} className='button'>
      {children}
    </button>
  )
}

const usage = <Button disabled>A button</Button>
// 1)                 ^^^^^^^^
// 2)                         ^^^^^^^^^
// 1) shortcut for disabled={true}
// 2) child components/nodes passed to a component
```

---

# State in react

- What we've seen so far:
  - Components can render chunks of UI
  - Components can be nested

---

# State in react

> How can we interact with components?

---

# State in react

> The State of a component is an object that holds some information that may change over the lifetime of the component [^5]

[^5]: [geeksforgeeks.com](https://www.geeksforgeeks.org/reactjs-state-react/#:~:text=What%20is%20State%3F,the%20lifetime%20of%20the%20component.)

---

# React State (without hooks)

```js
class ToggleButton extends React.Component {
  state = { backgroundColor: 'red' };
  // define a default value for background color

  toggleBackgroundColor = () => {
    const nextBackgroundColor = backgroundColor === 'red' ? 'blue' : 'red'
    this.setState({ backgroundColor: nextBackgroundColor })
    //   ^^^^^^^^^
    // setState calls render method with updated state
  }
  render() {
    return (
       <button
        onClick={() => this.toggleBackgroundColor() }
        style={{ backgroundColor: this.state.backgroundColor }}
      >
        {children}
      </button>
    );
  }
}
```

---

# React State (with Hooks)

- Alternative syntax with hooks

```js
const ToggleButton = () => {
  const [backgroundColor, setBackground] = useState('red')
  // 1)                                    ^^^^^^^^
  // 2) ^^^^^^^^^^^^^^^^
  // 3)                   ^^^^^^^^^^^^^^
  // 1) define a state with a default value "red"
  // 2) the current value of the state
  // 3) function to set the state to something else

  return (
    <button
      onClick={() => setBackground(backgroundColor === 'red' ? 'blue' : 'red')}
      style={{ backgroundColor }}
    >
      {children}
    </button>
  )
}
```

---

# React Hooks

> Hooks allow you to reuse stateful logic without changing your component hierarchy. [React Docs](https://reactjs.org/docs/hooks-intro.html#its-hard-to-reuse-stateful-logic-between-components)

----

### React Hooks

- Introduced recently to reduce boilerplate
- Makes it possible to use state in functional components
  - Previously one had to convert between functional/class components when state introduced
- hooks are prefixed with `use`
- Can't be called inside loops, conditions or nested functions

----

### useState

```js
const App = () => {
  const [count, setCount] = useState(0)
  const handleIncrement = () => setCount(count + 1)

  return (
    <div>
      <div>{count}</div>
      <button onClick={handleIncrement}>Increment by 1</button>
    </div>
  )
}
```

---

# Unidirectional Dataflow

- Props only flow from parent to children
- Parent is responsible to update data
  - might provide callbacks to do so
- set state rerenders all children of component

----

# Unidirectional Dataflow

![inline](https://miro.medium.com/max/1100/1*PBgAz9U9SrkINPo-n5glgw.gif)
> [Source](https://medium.com/@alialhaddad/https-medium-com-alialhaddad-redux-vs-parent-to-child-2583c8e29509)

----

# Kata

- Build an online integer calculator in react [^1]
- Implement the following arithmetic operations
  - addition
  - subtraction
  - multiplication
- Use JS BigInt datatype
- Data input should be possible via:
  - Buttons (eg. +, - , *, 1, 2, 3,...)
  - Optional: Keyboard input
- Bonus: build a decimal calculator
  - build a BigDecimal object
    - use BigInt to represent the number 
  - use the BigDecimal object in your calculator

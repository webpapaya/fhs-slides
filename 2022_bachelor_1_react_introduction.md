footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Fullstack Development

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

## Components
### Functional Components

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

## Components
### Class Components

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

## Components
### JSX

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

## Components
### React without JSX

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

## Components
### Which components do you see

![app, inline](assets/sign_in_wireframe.png)

----

## Components
### Which components do you see

![app, inline](assets/app_wireframe.png)

---

# Building the first react component

![filtered](./assets/background_8.jpg)

---

## JSX
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

## JSX
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

## JSX
### Conditional rendering

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

## JSX
### Conditional rendering

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

## JSX
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

---

## JSX
### Fragments

- Groups a list of children without adding a dom element

```js
const AComponent = () => {
  return (
    <>
      <label>An input</label>
      <input type='text' />
    </>
  )
}
```

----

## JSX
### Keyed Fragments

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

----

## JSX
### key property in loops

- Is required when iterating over lists
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

# Exercise: 30min

- I'll create breakout rooms
- Fork/clone the following <https://github.com/webpapaya/fhs-react-redux-starter-kit>
- execute `npm run start:storybook`

---

# Exercise: 30min

- extend the sign in component to display
  - Input with username
  - Input with password
  - A Sign In Button (reuse existing button)
- Extract an Input Component
  - make it configurable via props

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

## React Hooks
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

----

## React Hooks
### Extract into custom hook

```js
const useCounter = () => {
  const [count, setCount] = useState(0);
  const handleIncrement = () => setCount(count + 1);
  return { count, handleIncrement };
}

const App = () => {
  const {count,handleIncrement} = useCounter();

  return (
    <div>
      <div>{count}</div>
      <button onClick={handleIncrement}>Increment by 1</button>
    </div>
  );
}
```

----

## React State
### State vs. Props

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

## React State
### Unidirectional Dataflow

- Props only flow from parent to children
- Parent is responsible to update data
  - might provide callbacks to do so
- set state rerenders all children of component

----

## React State
### Unidirectional Dataflow

![inline](https://miro.medium.com/max/1100/1*PBgAz9U9SrkINPo-n5glgw.gif)
> [Source](https://medium.com/@alialhaddad/https-medium-com-alialhaddad-redux-vs-parent-to-child-2583c8e29509)

---

# Virtual DOM

- makes DOM updates faster
- after setState subtree is rerendered in memory
- compares DOM to in memory representation
- applies DOM changes when needed

---

### Forms with react hooks

```js
const App = () => {
  const [username, setUsername] = useState('');
  //                               ^^^^^^^^^^^^
  // define a new state with an initial value of empty string

  return (
    <div>
      <input onChange={(evt) => setUsername(evt.target.value)} value={username}>
      { /*                                 ^^^^^^^^^^^^^^^^^^ */}
      { /* set the state of the username */}
      <button onClick={() => console.log({ username })}>Submit form</button>
    </div>
  );
}
```

---

# Exercise: 30min

- Fork/clone the following <https://github.com/webpapaya/fhs-react-redux-starter-kit>
- execute `npm run start:storybook`

---

# Exercise

- extend the sign in component to display
  - Input with username
  - Input with password
  - A Sign In Button (reuse existing button)
- Extract an Input Component
  - make it configurable via props

![right, filtered](./assets/background_7.jpg)
---

- Bonus: extract a `useForm` hook

  ```js
    const [values, setValue] = useForm({
      username: '',
      password: ''
    })
    return <Input onChange={setValue('username')} />
  ```

![right, filtered](./assets/background_7.jpg)

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>

[^1]: https://reactjs.org/docs/components-and-props.html

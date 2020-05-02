# Introduction to React

---

# What we'll build in the next 2 days

----

![sign_in](assets/sign_in_wireframe.png)

----

![sign_up](assets/sign_up_wireframe.png)

----

![app](assets/app_wireframe.png)

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
    <button disabled={disabled} className='button'>
      {children}
    </button>
  )
}

const usage = <Button disabled>A button</Button>
```

---

### React Hooks

- Introduced recently to reduce boilerplate
- Makes it possible to use state in functional components
  - Previously one had to convert between functional/class components when state introduced
- hooks are prefixed with `use`
- Can't be called inside loops, conditions or nested functions

----

### State without hooks

```js
class App extends React.Component {
  state = { count: 0 };

  handleIncrement = () => {
    this.setState({ count: this.state.count + 1 })
  };

  render() {
    return (
      <div>
        <div>
          {this.state.count}
        </div>
        <button onClick={this.handleIncrement}>Increment by 1</button>
      </div>
    )
  }
}
```

----

### State with hooks

```js
const App = () => {
  const [count, setCount] => useState(0);
  const handleIncrement = () => setCount(count + 1);

  return (
    <div>
      <div>{count}</div>
      <button onClick={handleIncrement}>Increment by 1</button>
    </div>
  );
}
```

----

### useEffect

```js
const SimpleForm = ({ onSubmit }) => {
  const [counter, setCounter] = useState(0);

  // Is executed when component is rendered for the first time
  // And when the counter variable changes.
  useEffect(() => {
    document.title = `Counter clicked ${counter} times`;
  }, [counter]);

  return (
    <Button onClick={() => setCounter(counter + 1)}>
      {counter}
    <Button>
  );
};
```

----

### useEffect

```js
// Executed on every rerender
useEffect(() => {})

// Executed when component rendered initially
useEffect(() => {}, [])

// Executed when component rendered initially
// and when variable changes.
useEffect(() => {}, [variable])
```

----

### Other hooks

- [API Reference](https://reactjs.org/docs/hooks-reference.html)
  - useReducer
  - useCallback
  - useMemo
  - useRef
  - useImperativeHandle
  - useLayoutEffect
  - useDebugValue

---

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

- Fork/clone the following <https://github.com/webpapaya/fhs-react-redux-starter-kit>
- npm install
- npm run start:storybook
- Reuse/Adapt existing Button
- Create a UserSignIn Form Component
  - Username
  - Password
- If you're done help others

---

### React Router

- dynamic routing library for
- react native
- react web
- [Documentation](https://reacttraining.com/react-router/web/guides/quick-start)

----

### Installation

```txt
npm install react-router-dom --save
```

----

### Usage

 ```js
import { BrowserRouter as Router, Route, Switch, Redirect } from "react-router-dom";
import Homepage from './components/homepage'
import SignIn from './components/sign-in'
 const App = () => {
  return (
    <Router> { /* creates a new routing context */ }
      <Switch> { /* render only one route */ }
        { /* define routes and pass component as prop to the route */ }
        <Route path="/sign-in" component={SignIn}>
        <Route path="/" component={Homepage}>
        { /* if no route matches redirect to 'Homepage' */ }
        <Redirect to='/'>
      </Switch>
    </Router>
  );
}
```

----

### Route priority (without exact)

```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes Homepage
const Routes = () => (
  <Switch>
    <Route path="/" component={Homepage}>
    <Route path="/sign-in" component={SignIn}>
  </Switch>
);
```

----

### Route priority (without exact)

 ```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes Homepage
const Routes = () => (
   <Switch>
     <Route path='/sign-in' component={SignIn} />
     <Route path='/' component={Homepage} />
   </Switch>
)
```

----

### Route priority (with exact)

 ```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes sign-in
const Routes = () => (
  <Switch>
    <Route exact path="/" component={Homepage}>
    <Route exact path="/sign-in" component={SignIn}>
  </Switch>
);
```

----

### Add Links from html

 ```js
 import { Link } from 'react-router-dom'

 const Routes = () => (
   <nav>
     <Link to='/'>Home</Link>
     <Link to='/sign-in'>Sign in</Link>
   </nav>
)
```

----

### Add redirects from JS

 ```js
 import { withRouter } from 'react-router-dom'

 const SignIn = withRouter(({ history }) => {
   const onSubmit = (evt) => {
     evt.preventDefault()
     history.push('/')
   }

   return (
     <form onSubmit={onSubmit}>
       {/* ... */}
     </form>
   )
})
```

----

### Task

- Add the following routes to your application
- /sign-in
- /sign-up
- /app

---

## Tools/Resources

- [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
- [React Docs](https://reactjs.org)
  - [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)
- [Overreacted](https://overreacted.io/)

---

## Feedback

<https://de.surveymonkey.com/r/J6693VN>

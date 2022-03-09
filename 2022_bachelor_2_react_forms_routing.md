footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Fullstack Development

## (MMT-B2018)

---

# React Hooks Recap

> Hooks allow you to reuse stateful logic without changing your component hierarchy. [React Docs](https://reactjs.org/docs/hooks-intro.html#its-hard-to-reuse-stateful-logic-between-components)

----

### React Hooks Recap

- Introduced recently to reduce boilerplate
- Makes it possible to use state in functional components
  - Previously one had to convert between functional/class components when state introduced
- hooks are prefixed with `use`
- Can't be called inside loops, conditions or nested functions

----

## React Hooks Recap
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

## React Hooks Recap
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

## Forms in React

---

## Controlled vs uncontrolled components

---

## Controlled Components

- HTML form elements maintain own state
  - eg. input, textarea, ...
- React usually keeps state in their own components
  - component state/HTML state can get out of sync
- in controlled components react is the single source of truth

---

## Controlled Components

- React has ownership of state
  - result: typing in the component does not have any effect

```js
const Input = () => {
  return <input name="username" value="" />
  //                            ^^^^^^^^
  // if value is defined input becomes controlled
}
```

---

## Controlled Components

```js
const Input = () => {
  const [username, setUsername] = useState('')
  return <input
    name="username"
    onChange={(evt) => setUsername(evt.target.value)}
    //                 ^^^^^^^^^^^
    // 1) whenever onChange setUsername is called with new value
    value={ username }
    // 2) setUsername triggers a rerender with the new username
  />
}
```

---

## Uncontrolled components

- the browser keeps ownership of form state

```js
const Input = () => {
  return <input name="username" />
  //            ^^^^^^^^
  // if no value attribute is defined on an input
  // the state is uncontrolled and managed by the
  // browser
}
```

---

## Handle errors in components

```js
const SignUpForm = ({ onSubmit }) => {
  const [username, setUsername] = useState('')

  return (
    <form>
      <input
        name="username"
        onChange={(evt) => setUsername(evt.target.value)}
        value={ username }
      />
      { username.length === 0 && ( // when username is 0 display error
        <span>Username can't be blank</span>
      )}
      <button type="submit">Sign up</button>
    <form/>
  )
}
```

---

## Task (15 minutes)

- adapt your sign-up form
  - convert your components to controlled components
  - display error messages when username or password is blank
- Do you find any issues in your code?

---

## Do you see any issues with the code

```js
const SignUpForm = ({ onSubmit }) => {
  const [username, setUsername] = useState('')

  return (
    <form>
      <input
        name="username"
        onChange={(evt) => setUsername(evt.target.value)}
        value={ username }
      />
      { username.length === 0 && (
        <span>Username can't be blank</span>
      )}
      <button type="submit">Sign up</button>
    <form/>
  )
}
```

---

## Do you see any issues with the code

### 🚨 Don't spoil yourself and look at the next slides 🚨

---

## Do you see any issues with the code

### I mean really, stop here 😄

---

## Do you see any issues with the code

### 🚨 Seriously 🚨

---

## Do you see any issues with the code

- errors are shown even if a user didn't focus the input
- form can be submitted even if it contains errors
  - sign-in button is not disabled
- adding complex validations is tedious

---

## Form libraries which make your life easier

- there are multiple libraries which help with validation
  - [formik](https://formik.org/)
  - [react-hook-form](https://react-hook-form.com/)
  - [react final form](https://final-form.org/react)

---

## formik

- Form library which can be used with hooks
- uses controlled components
- `npm install formik yup`

---

## Formik

### Example

```js
import { useFormik } from "react-hook-form";

const SignInForm = () => {
  const formik = useFormik({
    initialValues: { username: '' },
    onSubmit: values => console.log(values),
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <input
        name="username"
        onChange={formik.handleChange}
        value={formik.values.username}
      />
      {/* ... */}
    </form>
  )
}
```

---

## Formik

### With errors

```js
import { useFormik } from "react-hook-form";
import {object, string} from 'yup'

const validationSchema = object({
  username: string().min(3)
})

const SignInForm = () => {
  const formik = useFormik({
    initialValues: { username: '' },
    validationSchema: validationSchema,
    // verify form with schema  ^^^^^^^^^^^

  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <input
        name="username"
        onChange={formik.handleChange}
        value={formik.values.username}
      />
      { formik.errors.username }
      {/* display the error */}
    </form>
  )
}
```

---

# Task 20 minutes

- convert your Sign Up form to use react hooked forms

---

# Routing

---

### React Router

- dynamic routing library for
- react native
- react web
- [Documentation](https://reacttraining.com/react-router/web/guides/quick-start)

----

### Installation

 ```
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
     <Route path='/' component={Homepage} />
     <Route path='/sign-in' component={SignIn} />
   </Switch>
)
```

----

### Route priority (without exact)

 ```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes SignIn
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
     <Route exact path='/' component={Homepage} />
     <Route exact path='/sign-in' component={SignIn} />
   </Switch>
)
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

---

## Task 20 minutes

- Start the application `npm run start`
  - `npm install react-router-dom`
  - add 2 routes
    - sign-up/
      - renders the SignUp component
    - sign-in/
      - renders a SignIn component (needs to be built)

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>

[^1]: whole code <https://gist.github.com/webpapaya/2a751dd740ee932bd9f348d780edc518>

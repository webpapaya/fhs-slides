footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Fullstack Development

## (MMT-B2018)

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
  - [react-hook-form](https://react-hook-form.com/)
  - [formik](https://formik.org/)
  - [react final form](https://final-form.org/react)

---

## React Hook Form

- Form library which uses hooks
- in contrast to other libs uses uncontrolled components
  - minimizes rerenders
- `npm install react-hook-form @hookform/resolvers superstruct`

```js
import { useForm } from "react-hook-form";

const SignInForm = () => {
  const { register, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit(d => console.log(d))}>
      <input name="username" ref={register} />
      <button type="submit">Sign up</button>
    <form/>
  )
}
```

---

## React Hook Form

### Validations [^1]

```js
import React from 'react'
import { useForm } from 'react-hook-form'
import { superstructResolver } from '@hookform/resolvers/superstruct'
import { struct } from 'superstruct'

const nonEmptyString = define('NonEmptyString', (value) => { // Define a validator
  if (typeof value === 'string' && value.length > 0) {
    return true
  } else {
    return { code: 'non_empty_string' }
  }
})

const schema = object({
  username: nonEmptyString,
});

const SignInForm = () => {
   const { register, handleSubmit } = useForm({
    resolver: superstructResolver(schema), //
  });
  // ... remaining component
};
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

## Homework

---

## Homework 1

- Build the following components in Storybook
  - UserSignIn -> onSubmit => { username, password }
  - UserSignUp -> onSubmit => { username, password }
  - MoneyTransactionCreate
    - users => { id, name }
    - onSubmit => { debitorId, creditorId, amount }

  - MoneyTransactionList (Lists all Money Transactions)
    - moneyTransactions
    - onMoneyTransactionPaid => { id, paidAt: (new Date()).toISOSTring() }

----

## Homework 2

- You probably need the following core components
  - `<TextInput {...} />`
  - `<DecimalInput {...} />`
  - `<SelectInput {...} />`
  - `<Button {...} />`
  - ...

----

## Homework 3

- Allowed to use CSS Frameworks
- Not allowed to use Component Libraries
- You can use as a starting point <https://github.com/webpapaya/fhs-react-redux-starter-kit>
- Mock Data for the API <https://gist.github.com/webpapaya/ba25ac39138b6f6a50a04f2b0820cf65>

----

## Homework 4

- Add the following routes
  - /sign-in
    - Sign-In component is rendered
  - /sign-up
    - Sign-Up component is rendered
  - /money-transactions
    - money-transactions-create component is rendered
    - money-transaction-list component is rendered

----

## Homework 5

- No need to connect to the backend
- Form submissions (just log to the screen or alert them)
- Don't update UI on form submissions
  - eg.: when creating a transaction the list doesn't need to update
  - we'll do this together next time with redux

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://de.surveymonkey.com/r/8TW92LL>

[^1]: whole code https://gist.github.com/webpapaya/1748cec029578215d6c62d04844c3877

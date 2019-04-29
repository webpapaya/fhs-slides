

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
    <Router> /* creates a new routing context */
      <Switch> /* render only one route */
        // define routes and pass component as prop to the route
        <Route path="/sign-in" component={SignIn}>
        <Route path="/" component={Homepage}>

        // if no route matches redirect to 'Homepage'
        <Redirect to='/'>
      </Switch>
    </Router>
  );
}
```

---

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

---

### Route priority (without exact)

```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes Homepage
const Routes = () => (
  <Switch>
    <Route path="/sign-in" component={SignIn}>
    <Route path="/" component={Homepage}>
  </Switch>
);
```

---

### Route priority (with exact)

```js
// path === "/" => renderes Homepage
// path === "/sign-in" => renderes Homepage
const Routes = () => (
  <Switch>
    <Route exact path="/" component={Homepage}>
    <Route exact path="/sign-in" component={SignIn}>
  </Switch>
);
```

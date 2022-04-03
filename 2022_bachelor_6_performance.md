footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# React Advanced Topics

## (MMT-B2020)

---

# About/Contact

- Thomas Mayrhofer (@webpapaya)
- E-Mail: tmayrhofer.lba@fh-salzburg.ac.at

---

# Roadmap

- Performance in Tips
- Portals
- Refs
- Error Boundaries
- Code Splitting


---

# React Performance Tips

![cover](./assets/background_2.jpg)


---
# React Performance Tips
## Use the production build

- react adds development checks/warnings
- in the production build warnings are stripped
- with CRA [^6] run `npm run build`

---
# React Performance Tips
## Immutability

> An immutable data structure is an object that doesn't allow us to change its value. (Remo H. Jansen)


----
# React Performance Tips
## Immutable objects in JS

````js
const immutableObject = Object.freeze({ test: 1 })
immutableObject.test = 10
console.log(immutableObject) // => { test: 1 }
````

----
# React Performance Tips
## Changing an immutable value

````js
const immutableObject = Object.freeze({ a: 1, b: 2 })
const updatedObject = Object.freeze({ ...immutableObject, a: 2 })
console.log(updatedObject) // => { a: 2, b: 2 }
````

----
# React Performance Tips
## Unfreeze an object

````js
const immutableObject = Object.freeze({ test: 1 })
const unfrozenCopy = { ...immutableObject }
````

----
# React Performance Tips
## Why immutability

- race conditions impossible
- state of the application is easier to reason about
- easier to test
- easier to compare 
  - shallow compare enough

---

# React Performance Tips
## Memoization

> `Memoizing' a function makes it faster by trading space for time. It does this by caching the return values of the function in a table. (<https://metacpan.org/pod/Memoize)>

---

# React Performance Tips
## React Component Tree

![inline](./assets/tree.png)

---

# React Performance Tips
## React.memo

```jsx
import React from 'react'

const MyComponent  = React.memo(({ users }) => {
//                   ^^^^^^^^^^
// memoize the component, all sub-components will
// be rerendered initially and only when the pointer 
// to the user array changes

  return (
    <section>
      <Users users={users}>
    </section>
  )
})
```

---

# React Performance Tips
## Filtering large lists

```jsx
import React from 'react'

const MyComponent = ({ users }) => {
  const activeUsers = users.map((user) => !user.inactive)

  return (
    <Users users={activeUsers} />
  )
}
```

---

# React Performance Tips
## Filtering large lists (Memoization)

```jsx
import React, useMemo from 'react'

const MyComponent = ({ users }) => {
  const activeUsers = useMemo(
//                    ^^^^^^^^
// add the useMemo hook
    () => users.map((user) => !user.inactive), 
//  ^^
// define a callback so react can decide when to filter
    [users])
//  ^^^^^^
// define the "dependencies" 
// = the filter function should be executed
//   when users array changes
  
  return (
    <Users users={activeUsers} />
  )
}
```

---

# React Performance Tips
## Arrays/Objects as default values

```jsx
import React, useMemo from 'react'

const MyComponent = ({ users = []}) => {
  const activeUsers = useMemo(
    () => users.map((user) => !user.inactive), 
    [users])
  
  return (
    <Users users={activeUsers} />
  )
}
```

---

# React Performance Tips
## Arrays/Objects as default values

```jsx
import React, useMemo from 'react'

const MyComponent = ({ users = []}) => {
//                             ^^
// every time undefined is passed to the component
// a new array will be created.

  const activeUsers = useMemo(
    () => users.map((user) => !user.inactive), 
    [users])
//  ^^^^^^
// users change with each rerender 
// => empty array will be filtered all the time

  return (
    <Users users={activeUsers} />
  )
}
```

---

# React Performance Tips
## Arrays/Objects as default values

```jsx
import React, useMemo from 'react'

const DEFAULT_USERS = []
// ^^^^^^^^^^^^^^^^^^^^^
// this array gets instantiated once when the code 
// is parsed initially. The same array will be reused
// with each rerender.

const MyComponent = ({ users = DEFAULT_USERS}) => {
//                             ^^^^^^^^^^^^^
// use the global array

  const activeUsers = useMemo(
    () => users.map((user) => !user.inactive), 
    [users])

  return (
    <Users users={activeUsers} />
  )
}
```

---

# React Performance Tips
## Callbacks

- Do you find anything problematic here?

```jsx
import React, useMemo from 'react'

const Users = React.memo(({ users, onUserClick}) => {
   // ...
})

const MyComponent = ({ users = DEFAULT_USERS}) => {
  return (
    <Users 
      users={activeUsers} 
      onUserClick={() => console.log('test')}
    />
  )
}
```

---

# React Performance Tips
## Callbacks

```jsx
import React, useMemo from 'react'

const Users = React.memo(({ users, onUserClick}) => {
   // ...
})

const MyComponent = ({ users = DEFAULT_USERS}) => {
  return (
    <Users 
      users={activeUsers} 
      onUserClick={() => console.log('test')}
  //               ^^^^^^^^^^^^^^^^^^^^^^^^^
  // Each time the component is rerendered a new function
  // will be created. This makes React.memo think that the
  // prop changed and will trigger a rerender of the Users
  // component.
    />
    
  )
}
```


---

# React Performance Tips
## Callbacks

```jsx
import React, { useCallback } from 'react'

const Users = React.memo(({ users, onUserClick}) => {
   // ...
})

const MyComponent = ({ users = DEFAULT_USERS}) => {
  const onUserClick = useCallback(
  //                  ^^^^^^^^^^^
  // cache/memoize the callback function
    () => console.log('test'), 
    []
  //^^
  // dependencies (when should the function be recreated)

  )
  return (
    <Users users={activeUsers} onUserClick={onUserClick} />
  )
}
```

---
# Task (15 min)

- Go to your application
- Create a new branch
- Find performance optimizations
  - Default Arrays
  - Arrays which could be memoized


---

# React Portals[^1]

![cover](./assets/background_4.jpg)

---

# React Portals[^1]

> Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

---

# React Portals[^1]

![inline](./assets/react_component_tree.png)

---

# React Portals[^1]

- Components are rendered inside nearest parent DOM

```js
const MyComponent = ({ children }) => {
  return (
    <div>
      {children}
    </div>
  );
}
```

---

# React Portals[^1]

- Sometimes components need to render outside of DOM nodes
  - eg. modals

```js
const MyComponent = ({ children }) => {
  return ReactDOM.createPortal(
    children,
    // specify elements to be rendered

    document.getElementById("myComponentRoot")
    // children will be rendered inside #myComponentRoot
  );
}
```

---

# Refs

> Refs provide a way to access DOM nodes or React elements created in the render method.

![right, filtered](./assets/background_4.jpg)

---

# Refs

![inline](./assets/react_component_tree.png)

---

# Refs [^2]

- props are used to interact with child components
  - to modify children rerender with different props
  - might be triggered via setState/useState
- refs are used to access DOM elements directly
  - setting focus
  - trigger imperative animations
  - using 3rd party DOM librarys

---

# Refs [^2]

```js
const MyInputComponent = ({ autofocus }) => {
    const inputRef = useRef(null);
    //               ^^^^^^^^^^^^
    // specify a reference with an initial value of null

    useEffect(() => {
        inputRef.current?.focus();
        //       ^^^^^^^^
        // current might be <input /> or null
        // when <input /> then focus input
    }, [inputRef.current])


    return <input ref={inputRef} />
    //            ^^^^^^^^^^^^^^
    // specify to which element the ref should be bound
}
```

---

# Refs [^2]

- Don’t Overuse Refs
  - prefer using props and lift up state

![right, filtered](./assets/background_5.jpg)

---

# Error Boundaries [^3]

![cover](./assets/background_6.jpg)

---

# Error Boundaries [^3]

- an error in a component might break the whole app
  - react mixes up state
  - reloading page is the only way to recover
  - bad UX
- Error boundaries handle errors of components

---

# Error Boundaries [^3]

- `static getDerivedStateFromError`
  - invoked after an error in child has been thrown
  - should return new component state
- `componentDidCatch`
  - invoked after an error in child has been thrown
  - used to trigger side effects (eg. server logging)

---

# Error Boundaries [^3]

```js
class ErrorBoundary extends React.Component {
    state = { hasError: false }
    static getDerivedStateFromError(error) {
        // Update state so the next render will show the fallback UI.
        return { hasError: true };
    }

    componentDidCatch(error, errorInfo) {
        // You can also log the error to an error reporting service
        logErrorToMyService(error, errorInfo);
    }

    render() {
        if (this.state.hasError) {
            // You can render any custom fallback UI
            return <h1>Something went wrong.</h1>;
        }

        return this.props.children;
    }
}
```

---

# Error Boundaries [^3]
## Using error boundaries

- using boundaries is up to the developer
- place boundaries around use-cases [^4] eg:
    - sign-up form
    - create money-transactions
    - money-transaction list

---

# Code Splitting

![cover](./assets/background_7.jpg)

---

# Code Splitting
## Bundling

> combine multiple JS modules into a single bundle

- when apps grow the bundle will grow
- large libraries are added to bundle
    - increases download/parse time

---

# Code Splitting [^5]

> split bundle into multiple bundles which can be loaded lazily during runtime

![left, filtered](./assets/background_7.jpg)

---

# Code Splitting [^5]
## Dynamic imports

```js
const add = async (a, b) => {
    const math = await import("./math")
    //           ^^^^^^^^^^^^^^^^^^^^^^
    // load "./math" module only when `add` is executed
    // for the first time. Bundlers will split the bundle
    // automatically. (2 files will be generated)

    return math.add(a, b)
});
```

---

# Code Splitting [^5]
## `React.lazy`

> The `React.lazy` function lets you render a dynamic import as a regular component.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
//                                      ^^^^^^
// dynamically import `./OtherComponent` when the component is rendered
// for the first time. 
// `./OtherComponent` needs to be exported as default
```

---

# Code Splitting [^5]
## `React.Suspense`

- importing modules dynamically is an async operation
- a loading screen needs to be displayed

> `React.Suspense` lets components “wait” for something before rendering.

![left, filtered](./assets/background_7.jpg)

---

# Code Splitting [^5]
## `React.Suspense`

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
  );
}
```

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>


[^1]: source <https://reactjs.org/docs/portals.html>

[^2]: source <https://reactjs.org/docs/refs-and-the-dom.html>

[^3]: source https://reactjs.org/docs/error-boundaries.html

[^4]: personal opinion

[^5]: source https://reactjs.org/docs/code-splitting.html

[^6]: Create React App (https://create-react-app.dev/)

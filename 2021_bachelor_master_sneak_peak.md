footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Selected Chapters Web

---

# About/Contact

- Thomas Mayrhofer (@webpapaya)
- E-Mail: tmayrhofer.lba@fh-salzburg.ac.at

---

# Survey Results

![inline](./assets/survey_results.png)

---

# Roadmap

- Portals
- Refs
- Error Boundaries
- Code Splitting
- ~~Client Side Debugging~~
- ~~Testing Redux~~

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

---

# Error Boundaries [^3]

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

# Other Master Topics

- Deep Dive in Testing Client Side Apps
- Performance Tuning in React/Redux
- Functional Programming in JS
- Scaling React/Redux
- Debugging in JS
- Localization
- ...


[^1]: source <https://reactjs.org/docs/portals.html>
[^2]: source <https://reactjs.org/docs/refs-and-the-dom.html>
[^3]: source https://reactjs.org/docs/error-boundaries.html
[^4]: personal opinion
[^5]: source https://reactjs.org/docs/code-splitting.html

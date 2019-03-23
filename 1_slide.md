# Introduction to React

---

# What we'll be doing

- Introduction to React
- Introduction to State Management with Redux
- TDD Workshop
- TDD with React

---

# React

- Component based library to declaratively build user interfaces

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

---

# Props in React
- React promotes a unidirectional dataflow
- Props are passed down to the components
  - Data can only be modified via callbacks

```js
const Button = () => {
  return (
    <button onClick={ () => console.log('Heeeeyyyy!!!') } type="button">
      { children }
    </button>
  );
}

React.renderComponent(<Button />, document.body);
```

----

# How to add state

```js
const { useState } = React;

const HEY = 'Heeeeyyyy!!!';
const HOO = 'Hoooooooo!!!';

const Button = () => {
  const [message, setMessage] = useState(HEY);
  const nextMessage = message === HEY ? HOO : HEY;

  return (
    <button onClick={ () => setMessage(nextMessage) } type="button">
      { message }
    </button>
  );
}

React.renderComponent(<Button />, document.body);
```

#

- Create component with
  - increment button
  - decrement button
  - value
- When button is clicked value is changed accordingly

---

# Homework
- Build a calculator
  - The following operations should be supported
  -


```js
const Button = () => (
  <button onClick={() => console.log('Horray!!!')}>
    Click Me
  </button>
);
```
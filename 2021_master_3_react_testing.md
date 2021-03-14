footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Testing React Components

## (MMT-M2020)

---

## Recap homework

---

## Multiple assertions in one test

```js
it('score of a roll is stored into the score', () => {
    const game = new Game();
    game.roll(5);
    expect(game.score).toBe(5);
    game.roll('/');
    expect(game.score).toBe(10);
    game.roll('X');
    expect(game.score).toBe(30);
})
```

---

## Multiple assertions in one test

- usually a sign of a test doing to much
- split into multiple tests

```js
it('when 5 pins are hit, score is 5', () => {
    const game = new Game();
    game.roll(5);
    expect(game.score).toBe(5);
})

it('when a strike was hit, score is 10', () => {
    const game = new Game();
    game.roll('x');
    expect(game.score).toBe(10);
})

it('when a strike was followed by 5 pins, score is 20', () => {
    const game = new Game();
    game.roll('x');
    game.roll(5);
    expect(game.score).toBe(20);
})
```

---
## Extract constants

```js
// ...
    if (item === '-') {
        return accumulated;
    }
    if (item === '/') {
        let nextThrow = getNextThrows(frames, index, i, 1)[0];
        if (nextThrow === '-')
            nextThrow = 0;
        else if (nextThrow === 'X')
            nextThrow = 10;
        else
            nextThrow = Number(nextThrow);
        return 10 + nextThrow;
    }
```

---
## Extract constants

```js
const MISS = '-'
const SPARE = '/'
const STRIKE = 'X'

// ...
    if (item === MISS) {
//               ^^^^
// replace magic characters with constants
        return accumulated;
    }
    if (item === SPARE) {
        let nextThrow = getNextThrows(frames, index, i, 1)[0];
        if (nextThrow === MISS)
            nextThrow = 0;
        else if (nextThrow === STRIKE)
            nextThrow = 10;
        else
            nextThrow = Number(nextThrow);
        return 10 + nextThrow;
    }
```

---
## No Linting

- next time -2 points
- please run
  - `npx eslint --init`
    - install linter
  - `npx mrm lint-staged`
    - automatically verify code before each commit

---

## Good example:

> https://gitlab.mediacube.at/fhs41228/cswe_pircher_maislinger/-/merge_requests/1/diffs


---

### Tools

- Cypress (e2e testing "Software Quality Assurance")
- React Test Utils (Facebook, low level)
- Enzyme (AirBnB, more high level)
- React Testing Library (best of both worlds)

---

### Testing Pyramid

![testing pyramid, inline](assets/testing_pyramid.png)

---

# Testing components

---

# Testing components
## Verify that some text is rendered

```js
// button.spec.js
import React from 'react'
import { render, cleanup, queryByText } from '@testing-library/react'
import { Button } from './Button'

afterEach(cleanup)
describe('Button', () => {
  it('renders given text', () => {
    // Arrange
    const givenText = 'A Button'

    // Act
    const { container } = render(<Button>{givenText}</Button>)

    // Assert
    expect(queryByText(container, givenText)).toBeTruthy()
  })
})
```

---

# Testing components
## Verify that some text is rendered

- Run the test and watch it fail
- The test will guide you towards the implementation

---

# Testing components
## Verify that some text is rendered

```js
// button.js
import React from 'react'

const Button = ({ children }) => {
  return <>{children}</>
}
```

---

# Testing components
## verify that an HTML tag is rendered

```js
// button.spec.js
import React from 'react'
import { render, cleanup, queryByText } from '@testing-library/react'
import { Button } from './Button'

afterEach(cleanup)
describe('Button', () => {
  // ... other tests

  it('renders a button html tag', () => {
    const givenText = 'A Button'
    const { container } = render(<Button>{givenText}</Button>)

    expect(container.querySelectorAll('button'))
// 1)                ^^^^^^^^^^^^^^^^^
      .toHaveProperty("length", 1)
// 2)  ^^^^^^^^^^^^^^

// 1) get all button HTML tags from the rendered container
// 2) verify that there is exactly one button HTML tag
  })
})
```

---

# Testing components
## verify that an HTML tag is rendered

```js
import React from 'react'

export const Button = ({ children }) => (
  <button> { /* render a Button instead of a fragment */ }
    {children}
  </button>
)
```
---

# Testing user interactions

---

# Testing user interactions

- How can we test user interactions?
  - eg. when the button was clicked?

```js
import React from 'react'

const Button = ({ onClick, children }) => {
  //              ^^^^^^^
  // How can we verify that the onClick handler is
  // called when the button is clicked?
  return <button>{children}</button>
}
```

----

# Testing components user interactions

- Spy objects are stubs that also record the way they were called
- Useful when:
  - A hard to verify side effect is triggered (eg. E-Mail sending)

```js
it('sends an email on sign up', () => {
  const sendEmail = jest.fn()
  const signUp = signUp({ sendEmail }, username, password)
  expect(sendEmail).toHaveBeenLastCalledWith({ username, password })
})
```

----

# Testing components user interactions

- Spy objects can be used to verify user interactions in react components

```js
// button.spec.js

import React from 'react'
import { render, cleanup, queryByText, fireEvent } from '@testing-library/react'
import Button from './Button'

afterEach(cleanup)
describe('Button', () => {
  // ...

  it('when button was clicked, calls given onClick handler', () => {
    const onClick = jest.fn() // create function spy
    const { container } = render(<Button onClick={onClick}>My Button</Button>)
    fireEvent.click(container.querySelector('button'))
// 1)               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
// 2)         ^^^^^
// 1) select the DOM element to be clicked
// 2) fire the click event

    expect(onClick).toHaveBeenCalled()
//  verify that the onClick spy has been called at least once
  })
})
```

---

# Testing components user interactions

```js
import React from 'react'

const Button = ({ onClick, children }) => (
  <button onClick={onClick}>
{/* 1)             ^^^^^^^   */}
    {children}
  </button>
)
// 1) set click handler on the button DOM element
```

----

# Testing components user interactions

```js
import React from 'react'
import { render, cleanup, queryByText, fireEvent } from '@testing-library/react'
import Button from './Button'

afterEach(cleanup)
describe('Button', () => {
  // ...

  it('when button was clicked, calls given onClick handler', () => {
    const onClick = jest.fn() // create function spy
    const { container } = render(<Button onClick={onClick}>My Button</Button>)
    fireEvent.click(container.querySelector('button'))
// 1)               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
// 2)         ^^^^^
// 1) select the DOM element to be clicked
// 2) fire the click event

    expect(onClick).toHaveBeenCalled()
//  verify that the onClick spy has been called at least once
  })
})
```

---

### Exercise 1

- You're building an issue tracking system
- Build a component which displays the names of assignees
  - eg. `<Assignees assignees={['Mike', 'Sepp', 'David']} />`
  - build a simple ul
- WITH more than 3 assigness,
  - only display 3 assignees
  - display a show more button

----

### Exercise 2

- WHEN the show more button was clicked
  - display all assignees
  - a show less button is displayed instead of a show more button
  - AND the show less button was clicked, only displays 3 assignees
- WITH less than 4 assignees,
  - don't display a "show more" button

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://de.surveymonkey.com/r/8TW92LL>

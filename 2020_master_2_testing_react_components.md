# Testing React Components

## (MMT-M2019)

---

## React Native

- Create native apps for Android and iOS using React

----

### Key differences 1

- No html tags like div/h1/form/button/...
  - Native tags like View/Text/TouchableOpacity/...
- Subset of css supported
- Interop with native APIs possible
  - eg. push notifications
- No links between pages

----

### Key differences 2

- Text needs to be rendered inside a `<Text>My Text</Text>` component
- No onClick handlers on every dom element
  - Adding a onPress handler needs to use TouchableOpacity
- Deployment is much harder (expo helps with that)

---

### CSS in React native

```ts
const Title = (props: { children: ReactNode}) => (
  <Text style={styles.container} numberOfLines={1}>
    { props.children }
  </Text>
)

const styles = StyleSheet.create({
  container: {
    fontSize: 20, // density indepentent pixels
    color: '#00ff00', // regular hex value
  }
})

export default Title
```

----

# Styling :first-child in react native

```js
const Title = (users}) => {
  return (
    <>
     { props.users.map((user, idx) => {
       return (
        <Text style={[styles.listItem, idx === 0 && styles.listItemFirst]}>
          { user }
        </Text>
       )
     }) }
    </>
  )
}


const styles = StyleSheet.create({
  listItem: {
    color: 'green',
  },
  listItemFirst: {
    color: 'red',
  }
})
```

----

### CSS in React native

- only a subset of css is supported
  - no pseudo elements (eg: :nth-child, :first-child)
  - no css grids
- flexbox per default (no display: block)
- css: box-shadow: '10px 10px 5px 0px rgba(0,0,0,0.75);'
  - translates to:

```js
const styles = StyleSheet.create({{
  shadowColor: colors.main,
  shadowOffset: { width: 0, height: 2 },
  shadowOpacity: 0.15,
  shadowRadius: 6
})
```

----

### Personal recommendations

- if possible build a PWA or use react.js
  - deployment is much easier
  - deployment to ios test-flight takes up to 2h
    - testing a quick-fix is tough
- use expo
  - helps with setup and upgrades
  - helps with deployment (code signing is done by expo)
    - via: `expo deploy:ios`
- don't try to use the same components in web and native
  - share business logic via npm package and build components twice

----

### Downsides React Native

- e2e tests tougher to write
  - especially with react navigation
- CI is more expensive
  - osx server required (Github recently added a free one)
- no pseudo elements

----

### More infos

- Teams which use react native can come and ask

---

### Testing Pyramid

![testing pyramid](assets/testing_pyramid.png)

----

### Testing Pyramid

- Unit tests
  - lots of small and isolated tests which are fast to execute
- Integration tests
  - some integration tests which test external systems like databases
- E2E
  - few tests which test the whole system

----

### Enterprise test pyramid

![enterprise testing pyramid](assets/enterprise_testing_pyramid.png)

---

# What is an integrated test

> A test where the success or failure depends on many different bits of interesting behaviour at once. (@jbrains)

----

### What is an integrated test

> Any test where the reason of a failure is hard to track down. (@jbrains)

----

### How many code paths

![code paths](assets/code_paths.png)

----

### How many code paths

![code paths](assets/code_paths_hightlight.png)

----

### has authentication

- auth given
  - but expired?
  - user was deleted?
  - user was disabled?
- not auth given?

----

### create user auth

- email already taken?
- password to short?
- db/auth service down?
- ...

----

### create membership

- group does not exist anymore?
- group was disabled?
- invitation got revoked?
- user was already added from other device?
- ...

----

### How many integration tests to write

![code paths](assets/code_paths_hightlight.png)

----

### Integrated tests

- hasAuth (4 paths)
- create user auth (3 paths)
- create membership (4 paths)
- Exponential growth
  - `4 * 3 * 4 = 48 tests`

----

### Unit tests

- hasAuth (4 paths)
- create user auth (3 paths)
- create membership (4 paths)
- `4 + 3 + 4 = 11 tests + 2 contract tests`

----

# Unit tests only?  <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/d5ut1zCCPGta0/giphy.gif" -->

----

### Happy path tests

- 1 integrated test per use-case
  - check if the communication between components work
  - run against
    - controller
    - main function
    - ...

----

### We'll focus on

![Focus in this semester](assets/testing_pyramid_focus.png)

- Other parts in "Software Quality Assurance"

---

# Unit testing and TDD

- Test driven development (also known as TDD)
- Type of software development
- Introduced by Kent Beck
  - Author of [Extreme Programming](https://www.amazon.de/Extreme-Programming-Explained-Embrace-Change/dp/8131704513/ref=sr_1_1?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=kent+beck+extreme+programming+englisch&qid=1557045753&s=books&sr=1-1-catcorr)

----

### Why TDD

- early/fast feedback during development
- Driving the design of our application
  - Testing is a side-effect
- Possibility to refactor
  - Confidence that app is still working
- Break down large problems into small problems
  - Think about edge cases
- executable documentation
  - Can't get out of sync
  - [Docs for pomeranian-durations](https://github.com/webpapaya/pomeranian-durations)

----

### TDD to me

> Getting confidence that refactoring doesn't break a feature.

----

### TDD

> TDD doesn't drive good design. TDD gives you immediate feedback about what is likely to be bad design. (Kent Beck)
> I want to go home on Friday and don't think I broke something. (Kent Beck)

----

### What is TDD not

- Silver bullet for clean code
  - it eventually leads to better code
- Replacement for other testing strategies
  - TDD doesn't catch all bugs
  - Helps adding regression tests

----

> The best TDD can do, is assure that the code does what the developer thinks it should do. (James Grenning)

---

# TDD Cycle

![tdd cycle](assets/tdd_cycle.png)

----

### TDD Cycle

- Red: Write a test and watch it fail
- Green: Write just as much code to make the test pass
- Refactor: Clean up

----

### Red

- Think about the test description
- Descriptions should reflect the behaviour of the program

```js
it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$')
})
```

----

### Green

- Write just enough code to make the test pass
  - if there is only 1 product just return 3$

```js
function caluculatePrice () {
  return '3$'
};

it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$')
})
```

----

### Refactor

- Change the code without changing any of the behaviour
- "Clean the kitchen"

```js
const caluculatePrice = () => '3$'

it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$')
})
```

---

# Anatomy of a Test

- **A**rrange => test setup
- **A**act => call the unit to test
- **A**ssert => verify the result

----

### Anatomy of a Test

```js
it('returns a list of employees ordered by their name', () => {
  // Setup
  const employees = [
    { name: 'Sepp' },
    { name: 'Max' },
    { name: 'Anton' }
  ]

  // Act
  const result = employeeReport(employees)

  // Assert
  assertThat(result, orderedBy((a, b) => a.name < b.name))
})
```

---

# What makes a good test

- Deterministic
  - randomness hard to test
  - current date time hard to test
- Tests not affecting state of the system
  - changing global state (eg. database => without cleanup)
- no external systems
- little test setup
- behaviour is described and not implementation details

----

### TDD Trap

- Bad tests
- Endless discussions "How do good tests look like"
- Don't focus on implementation detail but behaviour
- Tests are getting in their way
- TDD is not easy to start
- Extremely hard to master
- Deleting tests is fine (if they're not required anymore)

---

### TDD React Components

---

### Tools

- Cypress (e2e testing "Software Quality Assurance")
- React Test Utils (Facebook, low level)
- Enzyme (AirBnB, more high level)
- React Testing Library (best of both worlds)

---

# Testing components  <!-- .element: class="color--white" -->

##### Form Component  <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/32mC2kXYWCsg0/giphy.gif" -->

----

# Result

<https://gist.github.com/webpapaya/f2912dd7f62462e8ca87c2241b95a8fc>

---

### Testing components

```js
import React from 'react'
import { render, cleanup, queryByText } from '@testing-library/react'
import Button from './Button'

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

### Testing callbacks

----

# Spy objects

- Are stubs that also record the way they were called
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

```js
import React from 'react'
import { render, cleanup, queryByText, fireEvent } from '@testing-library/react'
import Button from './Button'

afterEach(cleanup)
describe('Button', () => {
  // ...
  it('onClick calls given function', () => {
    const onClick = jest.fn() // Create a function spy
    const { container } = render(<Button onClick={onClick}>A Button</Button>)
    fireEvent.click(container.querySelector('button'))
    expect(onClick).toHaveBeenCalledTimes(1)
  })
})
```

---

# Testing forms

```js
import React from 'react'
import { render, cleanup, queryByText, fireEvent } from '@testing-library/react'
import Form from './Form'

afterEach(cleanup)
it('submits username as form value', () => {
  const username = 'a value'
  const onSubmit = jest.fn()

  const { container } = render(<Form onSubmit={onSubmit} />)

  fireEvent.change(
    container.querySelector('[name="username"]'),
    { target: { value: username } }
  )
  fireEvent.submit(container.querySelector('form'))

  expect(onSubmit).toHaveBeenLastCalledWith({ username })
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

# Homework

- can be done in pairs
- hand-in via email tmayrhofer.lba@fh-salzburg.ac.at
  - email contains link to git reporitory
  - name of students who worked on the assignment

----

# Things I will look at

- functionality
- naming
- duplications
- code consistency (linting)
- function/component length
- commits + commit messages

----

# Homework Task

- due date: 20.3. 23:59:50
- build and test 3-4 React Components
  - 1 component per team member
- eg. ChatList
  - possible tests:
    - without any messages displays 'no messages yet'
    - when message was not read displays 'not read yet'
    - ...

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://de.surveymonkey.com/r/XQ96YZX>

# Scaling CSS

---
# Problems with CSS

- Global Namespace
- Dead Code elimination
- Sharing Constants
- Non-deterministic Resolution

----
# Dead Code elimination
- Remove code which does not affect a programs result
- With SPAs CSS dead code elimination becomes tough
    - eg. button--disabled only used conditionally
    - compiler would need to check all possible app states
- CSS code is left

----
# Sharing constants
- Constants couldn't be shared
    - SASS/Less helped with that
    - now we have CSS variables
- IE11 can't handle CSS variables

----
# Theming during runtime
- Dynamically change the appearance of the app
- eg. twitters profile color

----
# Non-deterministic resolution
- Order of selectors decides what is rendered
- Hard to tell what overrules what
- in SPAs CSS might be loaded dynamically
- Race condition might occure

----
# FOUC
- Flash of unstyled content
- Web page shortly appears without styles
- https://www.youtube.com/watch?v=O_jsUI5JD8c

---
# Global
- Available to the whole site
- What if external scripts add css?
    - eg. FB share buttons?
- eg. Bootstrap adds 600 globals

```css
.button {
    background: blue;
}
.primary-button {
    background: green;
}
```

----
# Global with naming convention
- BEM -> Block element modifier
- Block:
    - Standalone entity that is meaningfull on its own
    - eg. Button/TextInput/...
- Element:
    - Part of a block
    - eg. label of input, icon in input,...
- Modifiers:
    - Flagging blocks or elements
    - Change the appearance of an item
    - eg. primary/disabled/...

----
# Global CSS
| Question?                                  | Possible  |
|--------------------------------------------|-----------|
| No naming clashes possible?                | no        |
| Selector minification?                     | no        |
| Deterministic-Resolution?                  | no        |
| Theming during runtime?                    | yes       |
| Debugging with dev-tools?                  | yes       |
| Dead code elimination?                     | no        |
| No flash of unstyled content?              | yes       |
| Can be used on landing page?               | yes       |
| Refactoring?                               | partially |


----
# CSS in JS
- JS is used to generate CSS during runtime
- CSS classes can be unique
- CSS is only loaded when required


----
# Styled components

```js
const Button = styled.a`
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;

  ${props => props.primary && css`
    background: white;
    color: palevioletred;
  `}
`;

<Button>
<Button primary>
```

----
# Styled components

```js
const Button = styled.a`
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;

  ${props => props.primary && css`
    background: white;
    color: palevioletred;
  `}
`;

<Button>
<Button primary>
```

----
# CSS-in-JS
| Question?                                  | Possible  |
|--------------------------------------------|-----------|
| No naming clashes possible?                | yes       |
| Selector minification?                     | yes       |
| Deterministic-Resolution?                  | yes       |
| Theming during runtime?                    | yes       |
| Debugging with dev-tools?                  | partially |
| Dead code elimination?                     | yes       |
| No flash of unstyled content?              | no        |
| Can be used on landing page?               | no        |
| Refactoring?                               | yes       |

---
# CSS Modules
- regular CSS file
- can be imported in JS
- Compiler makes classnames unique

----
# CSS Modules

```ts
import styles from './centered-panel.css';

const Component = ({ children }) => (
  <section className={styles.wrapper}>
    <div className={styles.centered}>
      { children }
    </div>
  </section>
);
```

----
# Angular

```ts
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
```

----
# CSS modules
| Question?                                  | Possible  |
|--------------------------------------------|-----------|
| No naming clashes possible?                | yes       |
| Selector minification?                     | yes       |
| Deterministic-Resolution?                  | yes       |
| Theming during runtime?                    | yes       |
| Debugging with dev-tools?                  | yes       |
| Dead code elimination?                     | partially |
| No flash of unstyled content?              | yes       |
| Can be used on landing page?               | yes       |
| Refactoring?                               | partially |


---
# Final Comparison

| Question?                     | global    | CSS-in-JS | modules   |
|-------------------------------|-----------|-----------|-----------|
| No naming clashes possible?   | no        | yes       | yes       |
| Selector minification?        | no        | yes       | yes       |
| Deterministic-Resolution?     | no        | yes       | yes       |
| Theming during runtime?       | yes       | yes       | yes       |
| Debugging with dev-tools?     | yes       | partially | yes       |
| Dead code elimination?        | no        | yes       | partially |
| No flash of unstyled content? | yes       | no        | yes       |
| Can be used on landing page?  | yes       | no        | yes       |
| Refactoring?                  | partially | yes       | partially |

---
# Feedback

https://de.surveymonkey.com/r/J6693VN

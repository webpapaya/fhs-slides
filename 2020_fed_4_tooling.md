footer: FHS
slidenumbers: true

# Frontend Development

### Wintersemester 2020

---

# Tooling

---

# Linting [^1]

> Linting is the automated checking of your source code for programmatic and stylistic errors

---

# Linting [^5]

![inline](./assets/tabs_spaces.png)

---

# Linting

- Prevent programming errors earlier
  - tells problematic sections of the code earlier
- Enforce code consistency in projects

---

# Linting in JS

- spaces vs. tabs (see: <https://www.youtube.com/watch?v=SsoOG6ZeyUI)>
- naming (camelCase vs. snake_case)
- prevent `==` vs `===` errors
- using single vs double quotes
- code formatting
- ...

---

# Linting Tools

- EditorConfig
- eslint
- prettier

---

# EditorConfig

> EditorConfig helps maintain consistent coding styles for multiple developers working on the same project across various editors and IDEs.

```bash
# Unix-style newlines with a newline ending every file
[*.{js,py}]
indent_style = space
indent_size = 4
end_of_line = lf
insert_final_newline = true
```

---

# eslint

- standard tool for linting
- easy to extend
- autofix option
- can be configured [^2]

---

# eslint

## Presets

- presets bundle rule-sets together
- well known presets are:
  - eslint-config-airbnb
  - eslint-config-standard
  - eslint-config-google
  - ...

---

# eslint

## Install eslint [^3]

```bash
npx eslint --init # wizard opens (answer questions)
npx eslint . # lints all the files
npx eslint . --fix # lints all the files and fixes most errors
```

---

# eslint

## Add eslint to package.json

```json
{
 // other contents of package.json
 "scripts": {
    "start": "http-server .",
    "lint": "npx eslint ."
  },
}

// npm run lint
// npm run lint -- --fix
//              ^^
// are needed so npm passes --fix to eslint
```

---

# eslint

## rule configuration

```js
// .eslintrc.js

module.exports = {
  rules: {
    "no-await-in-loop": "off", // Possible values are "off", "warn", "error"
  }
}
```

---

# eslint

## disable rules [^4]

- sometimes disabling rules is ok

```js
// eslint-disable-next-line max-len
const anExtremelyLongLine = "................"

const anExtremelyLongLine = "................" // eslint-disable-line max-len
```

---

# Verifying linting rules

- verify linting continuously
  - on a CI server (eg. github actions)
  - on a git hook (eg. pre-commit, pre-push hook)

---

# Git hooks

- are executed when something happens
- available hooks are

```
pre-commit // <- we'll be using this
pre-push
post-commit
post-push
//...
```

---

# Pre-Commit Hook

## with husky [^6]

- Husky adds git hooks during installation
- in package.json add the following:

```json
// run: `npm install husky --save-dev`

// add hooks to package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint",
      "...": "..."
    }
  }
}
```

---

# Task (20 minutes)

- Go into your homework group
- Add eslint and pre-commit hook to your homework
- Fix all errors in your code
- I'll try to join each room and help when I'm needed

---

# JS bundling

---

# JS bundling

> A bundler compiles small pieces of code into something larger and more complex, such as a library or application [^7]

# Why Modules [^1]

- Maintainability
- Namespacing
- Reusability

[^1]: https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/

---

# JS bundling

## Why bundling

- JS apps are a bundle of modules [^8]
- older browsers might not understand JS modules
- older node version might not understand JS modules

---

# JS bundling tools

- [Rollup](https://rollupjs.org/)
- [Parcel](https://parceljs.org/)
- [Webpack](https://webpack.js.org/)
- ...

---

# JS bundling tools

## with rollup

```js
// rollup.config.js
export default {
  input: "index.js",
  output: [
    { file: "./build/index.bundle.js", format: "cjs" },
  ],
};

// # add build directory to gitignore
// echo ./build > .gitignore

// # build the app
// npx rollup -c
```

---

# Tree-Shaking

- when adding a module everything gets added to bundle
  - large libraries could increase the bundle unnecessarily
- bundlers which support tree shaking remove unused parts

---

# Tree-Shaking

## Example

```js
// a.js
export const aFunction = () => { console.log('a') }
export const bFunction = () => { console.log('a') }

// b.js
import {aFunction} from './a.js'

// Result: b.bundle.js
const aFunction = () => { console.log('test'); };
aFunction();
```

---

# JS bundling tools

## with rollup (no config required)

```bash
npx rollup ./index.js -d build/

# bFunction won't be part of the bundle
```

---

# Minification [^9]

> Minification refers to the process of removing unnecessary or redundant data without affecting how the resource is processed by the browser.

---

# Minification

- decrease bundle/download size by removing
  - comments
  - formatting
  - unused code
  - shorten variable/function names
  - ...
- get better google page speed score

---

# Minification

- minification can be done to:
  - js
  - html
  - css
  - ...

---

# Minification

## with terser plugin

- npm i `rollup-plugin-terser`

```js
import { terser } from "rollup-plugin-terser";

export default {
  input: "index.js",
  output: [
    {
      file: "./build/index.bundle.js",
      format: "cjs",
      plugins: [terser()]
      //       ^^^^^^^^^^
      // add the terser plugin which will minify the sources
    },
  ],
};
```

---

# Sourcemaps

- debugging minified code is no fun
- Sourcemaps way to revert minimization of code
- are defined as a special comment at the end of the file
  - external
    - `//# sourceMappingURL=http://example.com/path/to/your/sourcemap.map`
  - inline
    - directly in source code
      - use for debugging only

---

# Code splitting

- split your JS bundle into multiple smaller bundles
  - eg. create a vendor bundle (with libraries)
    - this file can be cached by the browser
    - recurring users don't need to download libs twice

---

# Transpiling

- source to source translator
- takes code and converts it to code of a different language

---

# Transpiling

## with babel

- babel is a transpiler
  - makes it possible to write esnext and convert it to old browsers

---

# Task (20 minutes)

- Go into your homework group
  - on a dedicated branch
- Add rollup and build your app into one file
  - add minification
  - add babel
  - add sourcemaps

---

# Open Questions

---

# Homework

- Finish the quiz
- when not already done
  - finish linting your application
  - I'll test via `npm run lint`
  - any error will result in -2 points

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- [Feedback Link](https://de.surveymonkey.com/r/8TW92LL)

[^1]: https://www.perforce.com/blog/qac/what-lint-code-and-why-linting-important

[^2]: and is configured in every project differently

[^3]: see <https://eslint.org/docs/user-guide/getting-started> for up to date information

[^4]: see <https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments> for all options

[^5]: source <https://dev.to/_>_shadz_/tabs-vs-space-49l5

[^6]: see <https://www.npmjs.com/package/husky>

[^7]: source <https://rollupjs.org/guide/en/>

[^8]: see module slides

[^9]: source https://developers.google.com/speed/docs/insights/MinifyResources#:~:text=Minification%20refers%20to%20the%20process,specific%20optimizations%20to%20learn%20more.

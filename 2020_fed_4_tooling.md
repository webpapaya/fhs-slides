footer: FHS
slidenumbers: true

# Frontend Development

### Wintersemester 2020

---

# [fit] Tooling

![cover](./assets/background_1.jpg)

---

# Linting [^1]

> Linting is the automated checking of your source code for programmatic and stylistic errors

![right, filtered](./assets/background_4.jpg)

---

# Linting [^5]

![inline](./assets/tabs_spaces.png)

---

# Linting

- Prevent programming errors earlier
  - tells problematic sections of the code earlier
- Enforce code consistency in projects

![right, filtered](./assets/background_5.jpg)

---

# Linting in JS

- spaces vs. tabs (see: <https://www.youtube.com/watch?v=SsoOG6ZeyUI)>
- naming (camelCase vs. snake_case)
- prevent `==` vs `===` errors
- using single vs double quotes
- code formatting
- ...

![right, filtered](./assets/background_6.jpg)

---

# Linting Tools

- various tools exist to validate code consistency
  - EditorConfig
  - eslint (defacto standard)
  - prettier (more code formatter)

![right, filtered](./assets/background_7.jpg)

---

# EditorConfig

![cover, filtered](./assets/background_9.jpg)

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

![cover, filtered](./assets/background_10.jpg)

---

# eslint

- standard tool for linting
- easy to extend
- autofix option
- can be configured [^2]

![left, filtered](./assets/background_11.jpg)

---

# eslint

## Presets

- presets bundle rule-sets together
- well known presets are:
  - eslint-config-airbnb
  - eslint-config-standard
  - eslint-config-google
  - ...

![left, filtered](./assets/background_1.jpg)

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

![right, filtered](./assets/background_2.jpg)

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

# eslint
## autofix staged files before commit [^15]

- [lint-staged](https://github.com/okonet/lint-staged) only lints files which have changed
  - adds autofix possibility to githook
  - installation `npx mrm lint-staged`

---



# [fit] Task Time

![cover](./assets/background_3.jpg)

---

# Task (20 minutes)

- Go into your homework group
- Add eslint and pre-commit hook to your homework
- Fix all errors in your code
- I'll try to join each room and help when I'm needed

---

# [fit] JS bundling

![cover](./assets/background_4.jpg)

---

# JS bundling

> A bundler compiles small pieces of code into something larger and more complex, such as a library or application [^7]

---

# Why Modules [^1]

- Maintainability
- Namespacing
- Reusability

![right, filtered](./assets/background_4.jpg)

[^1]: https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/

---

# JS bundling

## Why bundling

- JS apps are a bundle of modules [^8]
- older browsers might not understand JS modules
- older node version might not understand JS modules

![right, filtered](./assets/background_5.jpg)

---

# JS bundling tools

- Tools which resolve modules locally and bundle them together into a bigger bundle
- Bundling tools are:
  - [Rollup](https://rollupjs.org/)
  - [Parcel](https://parceljs.org/)
  - [Webpack](https://webpack.js.org/)
  - ...

![right, filtered](./assets/background_6.jpg)

---

# JS bundling tools

## with rollup

```js
// rollup.config.js
export default {
  input: "index.js",
  output: [
    { file: "./build/index.bundle.js", format: "iife" },
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

![right, filtered](./assets/background_7.jpg)

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

![right, filtered](./assets/background_8.jpg)

---

# Minification [^9]

> Minification refers to the process of removing unnecessary or redundant data without affecting how the resource is processed by the browser.

![right, filtered](./assets/background_10.jpg)

---

# Minification

- decrease bundle/download size by removing
  - comments
  - formatting
  - unused code
  - shorten variable/function names
  - ...
- get better google page speed score

![right, filtered](./assets/background_9.jpg)

---

# Minification

- minification can be done to:
  - js
  - html
  - css
  - ...

![right, filtered](./assets/background_11.jpg)

---

# Minification

![cover, filtered](./assets/background_11.jpg)

---

# Minification

## with terser plugin

- npm i `rollup-plugin-terser`

```js
// rollup.config.js
import { terser } from "rollup-plugin-terser";

export default {
  input: "index.js",
  plugins: [terser()]
  //       ^^^^^^^^^^
  // add the terser plugin which will minify the sources
  output: [
    {
      file: "./build/index.bundle.js",
      format: "iife",
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

# Using ESNext Features in legacy browser

![cover, filtered](./assets/background_1.jpg)

---

# Transpiling

- source to source translator
- takes code and converts it to code of a different language
  - eg. convert ESNext for old browsers

![right, filtered](./assets/background_8.jpg)

---

# Transpiling

## with babel & Rollup

- babel is a transpiler [^10]
  - it converts new syntax for the use in old browsers

```js
// eg. es6 arrow functions
const myFunction = () => {}

// will become
const myFunction = function myFunction () {}
```

---

# Transpiling

## with babel & Rollup [^11]

```bash
# install babel transpiler
npm install --save-dev @babel/core @babel/preset-env

# install babel plugin for rollup
npm install --save-dev @rollup/plugin-babel
```

---

# Transpiling

## configure babel & rollup

```js
// rollup.config.js

import { terser } from "rollup-plugin-terser";
import babel from '@rollup/plugin-babel';

export default {
  input: "index.js",
  plugins: [
    babel({ presets: [['@babel/env', {}]] }),
//1)^^^^^
//2)                    ^^^^^^^^^^

//1) add the babel plugin
//2) define the @babel/env

    // ... other plugins
  ],
};
```

---

# Transpiling

## @babel/preset-env

- a preset is a collection of transformations
  - eg. arrow function to function expression
- preset-env is smart enough to only add transforms which are required
  - can be configured via `.browserslistrc`

```
// .browserlistrc

> 5% in AT
```

---

# Polyfills

![cover, filtered](./assets/background_8.jpg)

---

# Polyfills [^13]

> New language features may include not only syntax constructs and operators, but also built-in functions.

---

# Polyfills

- `Math.trunc` removes the decimal part of a number
  - `Math.trunc(1.23) === 1`
  - older browsers might not implement Math.trunc
- Polyfills patch the browser with new APIs

```js
Math.trunc = function trunc(it) {
  return (it > 0 ? floor : ceil)(it);
}
```

---

# Polyfills

## Bundle with our application

- Install dependencies

```bash
# dependencies needed for bundling polyfills
npm i @rollup/plugin-node-resolve @rollup/plugin-commonjs --save-dev

# install polyfills
npm i core-js@3 --save
```

---

# Polyfills

## Configure rollup [^14]

```js
// rollup.config.js

import { terser } from "rollup-plugin-terser";
import babel from '@rollup/plugin-babel';
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';

export default {
  input: "index.js",
  plugins: [
    babel({
      presets: [['@babel/env', {
        babelHelpers: 'bundled', // add require statements for polyfills
        exclude: 'node_modules/**',
        presets: [
          ['@babel/env', { "useBuiltIns": "usage", corejs: { version: 3 } }]
        ]
      }]]
    }),
    commonjs(), // signalize rollup that it should bundle commonjs modules
    resolve(), // inline libraries from node_modules
  ],
  // ...
};
```

---

# [fit] Task Time

![cover](./assets/background_3.jpg)

---

# Task

- Go into your homework group
  - on a dedicated branch
- Add rollup and build your app into one file
  - add minification
  - add babel
    - play around with different .browserlistrc configs and see the difference in bundle size

---

# Homework

- Finish the quiz
- when not already done during todays lecture
  - finish linting your application
  - I'll test via `npm run lint`
  - any error will result in -2 points

![left, filtered](./assets/background_2.jpg)

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

[^9]: source <https://developers.google.com/speed/docs/insights/MinifyResources#:~:text=Minification%20refers%20to%20the%20process,specific%20optimizations%20to%20learn%20more.>

[^10]: see [babel repl](https://babeljs.io/repl#?browsers=defaults&build=&builtIns=false&spec=false&loose=false&code_lz=MYewdgzgLgBAtgTwGIFczCgS3DAvDACgEo8A-GAbwF8g&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.12.7&externalPlugins=)

[^11]: installation instructions for other bundlers <https://babeljs.io/en/setup>

[^12]: full list of possible queries <https://github.com/browserslist/browserslist#queries>

[^13]: source <https://javascript.info/polyfills>

[^14]: complete configuration https://gist.github.com/webpapaya/45c5aae75bbe4e8eb72bc19c33e080bf

[^15]: docs https://github.com/okonet/lint-staged
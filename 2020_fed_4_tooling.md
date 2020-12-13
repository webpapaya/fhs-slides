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
- spaces vs. tabs (see: https://www.youtube.com/watch?v=SsoOG6ZeyUI)
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



---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- [Feedback Link](https://de.surveymonkey.com/r/8TW92LL)

[^1]: https://www.perforce.com/blog/qac/what-lint-code-and-why-linting-important

[^2]: and is configured in every project differently

[^3]: see https://eslint.org/docs/user-guide/getting-started for up to date information

[^4]: see https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments for all options

[^5]: source https://dev.to/__shadz_/tabs-vs-space-49l5

[^6]: see https://www.npmjs.com/package/husky
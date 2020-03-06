# Defactoring

## (MMT-M2018)

---

## Roadmap

- Code Smells
- Defactoring vs. Refactoring
- Mob Programming Session
- Short Recap

---

## Code Smells

> A code smell is a hint that something has gone wrong somewhere in your code. Use the smell to track down the problem. Kent Beck

----

# Dead Code

- Unused code in the codebase which might be usefull in the future.
- Possible refactoring:
  - Delete the code

----

# Duplicated Code

- A sequence of code which occures more than once in the code base.
- Possible refactorings:
  - combine this sequence into a function/class/method

----

# To many parameters

- A function/method has a long parameter list
- Possible refactorings:
  - Remove unused parameters
  - Combine those parameters into an object

----

## Exceptions for controll flow

- An exception is used for changing the behaviour of a procedure.
- Possible refactorings:
  - use if to determine the error case

```js
const formatISODate = (isoDate) => {
  try {
    return Date.parse(isoDate).forrmat('YYYY-MM-DD')
  } catch (_) {
    return 'Invalid Date'
  }
}
```

```js
const formatISODate = (isoDate) => {
  if (!date.canParse(isoDate)) { return 'Invalid Date' }
  return Date.parse(isoDate).format('YYYY-MM-DD')
}
```

----

# Large function/method

- A function/method which has to many responsibilities and does to much
- Possible refactoring:
  - split the function/method into multiple small methods

----

# Large class

- A class which has to many responsibilities
- also called God Object
- Possible refactoring:
  - split the class into multiple small classes

----

# Long code lines

- A line which is doing to much
- possible refactorings
  - Split the line into multiple lines
  - Extract function/method

----

# Comments

- Only if the comment describes what the code is doing.
- possible refactorings
  - make the code easier to understand and remove the comment

```java
/**
 * Always returns true.
 */
public boolean isAvailable() {
    return false;
}
```

---

# Refactoring vs. Defactoring

----

## Refactoring

> Refactoring is a controlled technique for improving the design of an existing code base. [Fowler]

<https://martinfowler.com/articles/refactoring-2nd-ed.html>

----

## Defactor

- Oposite of refactoring.
- Helps to remove inflexible abstractions

---

## Defactor session

- Mob programming session
- Timebox 45 minutes
- Use the following:
  - <https://github.com/webpapaya/fhs-defactoring>

---

## Feedback

<https://de.surveymonkey.com/r/J6693VN>

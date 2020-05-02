# TypeScript

---

# Roadmap for the following days

- Today:
  - Introduction to typescript
  - Introduction to domain modeling in ts
- Tomorrow:
  - Either type
  - Runtime type checking with IO-TS

---

# Roadmap for the following days


- Why ts
- features
  - type inference
  - const assertions
  - generics
  - type assertions

---

## About Typescript

- Superset of JavaScript
- Adds static type checking
  - compiles to JavaScript

---

# Optional type system

```ts
let someValue: number = 10
someValue = '10'
// ^^^^^
// Type '"10"' is not assignable to type 'number'.
```

---

# Data types in TS

- any
- primitives
  - number
  - boolean
  - string
  - Array
  - Tuple
  - Enum
  - Void (absence of a type)
  - null
  - undefined

----

# number

- Same data type as in JavaScript
- floating point numbers
  - supports decimal, hex, octal

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

----

# boolean

- Simple true/false values

```ts
let isDone: boolean = false;
let trueOnly: true = true;
let falseOnly: false = false;

trueOnly = false
// ^^^^^^^^^^^^^
// Error: Type 'false' is not assignable to type 'true'
```

----

# string

- same as in JS

```ts
let color: string = "blue";
color = "green";

color = 10;
// ^^^^^^^^
// Error: Type '10' is not assignable to type 'string'
```

----
# any

- opt out of type checking
  - might be useful when a type is unknown during development
    - eg. dynamic content from a 3rd party API
- used to gradually adapt TypeScript
  - is sometimes misused
  - might be valuable for generics

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
```

----

# arrays

- two ways to specify arrays
  - short notation `string[]`
  - as generic `Array<string>`

```ts
let colors1: string[] = ['green', 'blue'];
let colors2: Array<string> = ['green', 'blue'];
```

----

# tuple

- express array with fixed number of elements
  - eg. vectors

```ts
let vector: [number, number] = [1, 2];

// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

---

# Interfaces

- focus on the shape of types (eg.: duck typing)
  - if it quakes like a duck it is considered a duck

```ts
type User = {
  name: string,
}

const printUserName = (user: User) => console.log(user.name)
const user = {
  name: 'Sepp',
  age: 55
}
printUser(user)
//       ^^^^^^
// No compile error even if the `user` contains the
// property `age` which is not part of the User interface.
```

----

# Optional values 1
- not all properties of interfaces might be required
- TS allows to specify optional properties

```ts
type User = {
  name: string,
  age?: number
//   ^
// makes the property optional
}

const userWithoutAge: User = {
  name: 'Sepp',
};
```

----

# Optional values 2
- Optional values are type checked

```js
type User = {
  name: string,
  age?: number
}

const printUserAge = (user: User) => {
  return console.log(user.age.toString())
  //     ^^^^^^^^
  // Error: Object is possibly 'undefined'
}
```

----

# Unions

- a type in typescript can be of more than one type
  - this is called a union and a pipe `|` is used
- the `|` could be seen as an 'or' operator

```ts
type AnonymousUser = { id: number }
type RegisteredUser = { id: number, email: string, password: string }
type User = AnonymousUser | RegisteredUser
//                       ^^^
// A user is either an AnonymousUser OR a RegisteredUser
```

---

# Literals

- literal is subset of a more generic type
  - eg: "Hello MMT" is a string, but not every string is "Hello MMT"
  - allows to narrow types
- can be used to express constants
  - makes readability of code more expressive

```ts
type Lecture =
  | 'Fullstack Development'
  | 'Client side engineering'
  | 'Software Quality Assurance'

const assignToLecture = (email: string, lecture: Lecture) => { /* irrelevant */ }

assignToLecture('sepp@fh-salzburg.ac.at', 'Fullstack Development')
assignToLecture('sepp@fh-salzburg.ac.at', 'HCI')
//                                        ^^^^^
// Argument of type '"HCI"' is not assignable to parameter of type 'Lecture'.
```

---
# Generics



---

## Domain Modeling and TS

---

- Requirements:
  - A user needs to have a first and last name
  - A user needs to have exactly one contact
    - a contact is either:
      - address (contains street/zip code/country)
      - phone (contains phone)
      - email (contains email)
    - a contact can be verified

----

## Resulting Type

```ts
type User = {
  firstName: string,
  lastName: string,
  street?: string,
  zipCode?: string,
  country?: string,
  email?: string,
  isEmailVerified?: bool,
  phone?: string,
  isPhoneVerified?: bool,
}
```

----

> Can you spot issues with this model?

----

```ts
type User = {
  firstName: string,
  lastName: string,
  street?: string,
  zipCode?: string,
  country?: string,
  email?: string,
  isEmailVerified?: bool,
  phone?: string,
  isPhoneVerified?: bool,
}
```

```ts
const user = {
  firstName: 'Sepp',
  lastName: 'Dupfinger',
  street: 'Hinterholz 8',
};
```

----

## Issues

- 1 correct state and 7 falsy states

```ts
type User = {
  // ...
  street?: string,
  zipCode?: string,
  country?: string,
  // ...
}
```

----

## Requirements

- A user needs to have a first and last name
- A user needs to have exactly one contact
  - a contact is either:
    - address (contains street/zip code/country)
    - phone (contains phone)
    - email (contains email)
  - a contact can be verified

----

## Classify the type

```ts
type User = {
  firstName: string,
  lastName: string,

  // via post
  street?: string,
  zipCode?: string,
  country?: string,
  isAddressVerified?: bool,

  // via email
  email?: string,
  isEmailVerified?: bool,

  // via phone
  phone?: string,
  isPhoneVerified?: bool,
}
```

----

## Extract smaller bits

```ts
type PostContact = { street: string, zipCode: string, country: string, isVerified: bool }
type EmailContact = { email: string, isVerified: bool }
type PhoneContact = { phone: string, isVerified: bool }
type Contact = PostContact | EmailContact | PhoneContact

type User = {
  firstName: string,
  lastName: string,
  contact: Contact,
}
```

----

## Extract common properties

```ts
type Verifiable<T> = T & { isVerified: boolean }

type PostContact = Verifiable<{ street: string, zipCode: string, country: string }>
type EmailContact = Verifiable<{ email: string }>
type PhoneContact = Verifiable<{ phone: string }>
type Contact = PostContact | EmailContact | PhoneContact

type User = {
  firstName: string,
  lastName: string,
  contact: Contact,
}
```

----

## Use it

```ts
const user:User = {
  firstName: 'Sepp',
  lastName: 'Dupfinger',
  contact: { email: 'sepp@hinterholz.at', isVerified: true },
}
```

----

> Can you still spot issues with this model?

----

```ts
const user:User = {
  firstName: '',
  lastName: '',
  contact: { email: '', isVerified: true },
}
```

----

## Type aliases

```ts
type Email = string
```

----

## Verifying type aliases

```ts
type Maybe<T> = T | null
type Email = string

const validateEmail = (maybeEmail: unknown): Maybe<Email> => {
    if (typeof maybeEmail === 'string' && maybeEmail.match(/.@./)) {
        return maybeEmail as Email;
    }
    return null;
}
```

----

```ts
type Maybe<T> = T | null
type Verifiable<T> = T & { isVerified: boolean }

type Email = string
type Phone = string
type Street = string
type ZipCode = string
type Country = string

type PostContact = Verifiable<{ street: Street, zipCode: ZipCode, country: Country }>
type EmailContact = Verifiable<{ email: Email }>
type PhoneContact = Verifiable<{ phone: Phone }>
type Contact = PostContact | EmailContact | PhoneContact

//...
```


---
TODOs:
- unknown type
- explain generics
- io-ts slides
-

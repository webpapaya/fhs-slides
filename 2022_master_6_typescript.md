footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# Typescript

![cover, filtered](./assets/background_8.jpg)

---

# Roadmap next lectures

- 28.4. Exam & Project Coaching
- 2.5. Testing Side Effects

---

# Exam 
## Possible Questions 

- Write a component which displays the name of the following users?
- What is an action creator in redux?
- Given is the following redux state, write a mapStateToProps function for the following component
- Which of the following functions is pure? (explain why)
- ...

---

## About Typescript

- Developed by Microsoft
  - first public release 2012
- Superset of JavaScript
- Adds static type checking
  - compiles to JavaScript
- is turing complete
  - <https://github.com/microsoft/TypeScript/issues/14833>
  - <https://github.com/AvraamMavridis/Algorithms-Data-Structures-in-Typescript>

---

## Why Typescript

- Prevent runtime errors
- self documenting code
- IDE support
  - code completion
  - automated refactoring
- generate API docs from types
- easier code lookup

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
  - void (absence of a type)
  - null
  - undefined

---

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

---

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

---

# string

- same as in JS

```ts
let color: string = "blue";
color = "green";

color = 10;
// ^^^^^^^^
// Error: Type '10' is not assignable to type 'string'
```

---

# any

- opt out of type checking
  - might be useful when a type is unknown during development
    - eg. dynamic content from a 3rd party API
- used to gradually adapt TypeScript
  - is sometimes misused
  - might be valuable for generics

---
# any

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
```

---

# arrays

- two ways to specify arrays
  - short notation `string[]`
  - as generic `Array<string>`

```ts
let colors1: string[] = ['green', 'blue'];
let colors2: Array<string> = ['green', 'blue'];
```

---

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

---

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

---

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

---

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

---

# literals

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

- Task build an identity function
  - identity function returns the same value that it was given as argument

```js
const identityInJS = (arg) => arg
```

---

# Generics

- Possible implementation in TS

```ts
const stringIdentity = (arg: string) => arg
const numberIdentity = (arg: number) => arg
const booleanIdentity = (arg: boolean) => arg
// ...
```

---

# Generics

- Possible implementation with `any`

```ts
const identity = (arg: any) => arg
const value = identity(1)
value.toUpperCase()
//   ^^^^^^^^^^^^^^
// Will throw `value.toUpperCase is not a function`
```

---

# Generics

- possibility to reuse types with other types
- written in angle brackets `<NameOfTypeVariable>`
- I like to see them as:
  - type level functions

```ts
const identity = <T>(arg: T): T => arg
//             1)^^^    2)^  3)^
// 1) Define a type argument T
// 2) type of arg is assigned to the type argument T
// 3) the function returns the type argument T
```

---

# Generics

```ts
const identity = <T>(arg: T): T => arg

const stringValue1 = identity<string>('Hallo MMT')
//                                   ^^^^^^^^
// explicitly assign string as the type argument to the identity function
// string value1 will be of type string

const stringValue2 = identity('Hallo MMT')
// typescript automatically infers type of the type argument T
// string value2 will be of type string
```

---

# Generics for records

- Generics can be used to compose

```ts
type ServerResponse<ResponsePayload> = {
//                 ^^^^^^^^^^^^^^^^
// Define a type variable called `ResponsePayload`
  payload: ResponsePayload
// Use the type variable `ResponsePayload`
}

type UserResponse = ServerResponse<{ name: string }>
// resulting type: { payload: {name: string} }
```

---

# Utility Types

- `Pick<T>`
- `Omit<T>`
- `Partial<T>`
- `Required<T>`
- `Arguments<T>`
- `ReturnType<T>`
- `Readonly<T>`
- `ReadonlyArray<T>`
- `Record<T>`
- `Array<T>`
- ...

---

# Pick<T>

- Creates a subset of an existing record type
- Allows to specify a list of properties to extract
- similar to lodash pick, but on a type level

```ts
type User = {
  firstName: string,
  lastName: string,
  age: number
}

type UserName = Pick<User, 'firstName' | 'lastName'>
// type will be { firstName: string, lastName: string }
```

---

# Omit<T>

- Creates a subset of an existing record type
- Allows to specify a list of properties to remove
- similar to lodash omit, but on a type level

```ts
type User = {
  firstName: string,
  lastName: string,
  age: number
}

type UserName = Omit<User, 'age'>
// type will be { firstName: string, lastName: string }
```

---

# Partial<T>

- makes all properties of an object optional
- might be used to overwrite default configs etc.

```ts
const defaultTSConfig = {
  target: 'ES2015',
  declaration: true,
}
type TSConfig = typeof defaultTSConfig
//              ^^^^^^
// automatically infer the type of defaultTSConfig
// TSConfig will be of type { target: string, declaration: boolean }

const start = (options: Partial<TSConfig>) => { /* irrelevant*/ }
//                      ^^^^^^^^^^^^^^^^^
// options will be of type { target?: string, declaration?: boolean }

start({ target: 'es5' })
start({ declarations: false })
```

---

# Required<T>

- opposite of partial
- makes all values of a record required

```ts
type User = Required<{
  name: string,
  age?: number
}>
const user: User = { name: 'Sepp' }
//    ^^^^
// Error: Property 'age' is missing in type
```

---

# Arguments<T>/ReturnType<T>

- `Argument<T>` returns the type of the arguments of a function
- `ReturnType<T>` returns the return type of a function

```ts
const add = (a: number, b: number) => a + b
type AddArguments  = Arguments<typeof add> // [number, number]
type AddFirstArgument  = AddArgument[0]
type AddSecondArgument  = AddArgument[1]

type AddReturnType = ReturnType<typeof add> // number
```

---
# Access a subtype in object

```ts
type User = {
  firstName: string,
  lastName: string,
  age: number
}

type Age = User['age'] // => type: number
```

---
# Unwrap array

```ts
type MyArray = number[]

type ArrayItem = MyArray[number] // => type: number
```


---

# Type inference

- automatic deduction of a type from an expression

```ts
let mutableValue = 10 // => type number
const constantValue = 10 // => type 10
```

---

# Type inference
## Generics

- Some generics can be inferred automatically

```ts
const numberArray = [0,1,2,3] // => Array<number>
const stringArray = ['A','B','C','D'] // => Array<string>
const booleanArray = [true,false] // => Array<boolean>
const mixedArray = [1, 'A', true] // Array<number | string | boolean>
```

---

# Type inference
## const assertions

- JS values are mutable
- JS value can be altered despite being defined as const

```ts
const numberArray = [0,1,2,3] // => type Array<number>
numberArray[0] = 10;
```

---

# Type inference
## const assertions

- `const assertion` mark a value as immutable
- type is narrowed

```ts
const numberArray = [0,1,2,3] as const // => type readonly [0, 1, 2, 3]
numberArray[1] // => type 1
```

---

# Type inference
## const assertions objects

- `const assertion` works on objects as well

```ts
const myObject = { a: 1, b: 'one' } as const 
// => type readonly { readonly a: 1; readonly b: "one" }

numberArray[b] // => type 'one'
```

---

# React and TypeScript

- React integrates with TypeScript
- can replace prop types from React

```ts
type AvatarsProps = {
  images: string[]
}

const Avatars = (props: AvatarsProps) => (
    <div className={css(styles.wrapper)}>
      { props.images.map((imageUrl) => (
        <img key={imageUrl} src={imageUrl} className={css(styles.image)} />
      ))}
    </div>
)

<Avatars images={[1,2,3]} />
//                ^^^^^^
// Error: number is not assignable to string
```

---

## Exercise create a user type:

- Requirements:
  - a user needs to have a first and last name
  - a user needs to have exactly one contact
    - a contact is either:
      - address (contains street/zip code/country)
      - phone (contains phone)
      - email (contains email)
    - a contact can be verified

---

## Exercise create a user type
### Possible Solution

```ts
type User = {
  firstName: string,
  lastName: string,
  street?: string,
  zipCode?: string,
  country?: string,
  isAddressVerified?: bool,
  email?: string,
  isEmailVerified?: bool,
  phone?: string,
  isPhoneVerified?: bool,
}
```

---

## Exercise create a user type
### Can you spot issues with this model?

```ts
type User = {
  firstName: string,
  lastName: string,
  street?: string,
  zipCode?: string,
  country?: string,
  isAddressVerified?: bool,
  email?: string,
  isEmailVerified?: bool,
  phone?: string,
  isPhoneVerified?: bool,
}
```

---

## Exercise create a user type
### Can you spot issues with this model?


```ts
const user = {
  firstName: 'Sepp',
  lastName: 'Dupfinger',
  street: 'Hinterholz 8',
};
```

---
## Exercise create a user type
### Issues

- 1 correct state and 8 falsy states

```ts
type User = {
  // ...
  street?: string,
  zipCode?: string,
  country?: string,
  // ...
}
```

---
## Exercise create a user type
### Requirements

- A user needs to have a first and last name
- A user needs to have exactly one contact
  - a contact is either:
    - address (contains street/zip code/country)
    - phone (contains phone)
    - email (contains email)
  - a contact can be verified

---

## Exercise create a user type
### Classify the type

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

---

## Exercise create a user type
### Extract smaller bits

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

---
## Exercise create a user type
### Extract common properties

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

---

## Use it

```ts
const user:User = {
  firstName: 'Sepp',
  lastName: 'Dupfinger',
  contact: { email: 'sepp@hinterholz.at', isVerified: true },
}
```

---

> Can you still spot issues with this model?

---

```ts
const user:User = {
  firstName: '',
  lastName: '',
  contact: { email: '', isVerified: true },
}
```

---

## Type aliases

```ts
type Email = string
```

---

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

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>

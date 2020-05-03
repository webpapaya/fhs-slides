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

# Homework
- Build the domain model for your application in TS
- Try to make invalid states impossible
- Try to think about possible edge cases and how to prevent them



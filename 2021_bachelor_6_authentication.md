# Authentication in React/Redux

![cover, filtered](assets/background_10.jpg)

---

### Roadmap

- Authentication
- (Debugging)

![right, filtered](assets/background_11.jpg)

---
### Authentication

- Verify identity of something
  - Who is somebody
- Identity can be
  - User
  - Different Service
  - Pets
  - ...
- Sometimes refered to as Authn

![right, filtered](assets/background_8.jpg)

----

### Authentication

- identity is verified by credentials
- usually combination of username/password
- other data might be used for identification
  - IP Geolocation
  - 2 Factor Authentication
  - Security Keys

---

### Authorization

- what resources should somebody have access to
  - What am I allowed to do
    - eg. A person can only change its own password
- usually happens after authentication
  - Anonymous resources might be accessible without authz (eg. reading news articles)

![right, filtered](assets/background_7.jpg)

---

### Stateful Authentication

- Session data is stored in the backend [^1] [^2] [^3] [^4]


![inline](assets/jwt_traditional.png)

----
### Stateful Authentication

- Pros:
  - Revoke session anytime
  - Easy to implement
  - Session data can be changed anytime
- Cons:
  - Increasing server resources
  - Every session needs to hit db
  - hard to integrate 3rd party apps

---

### Stateless Authentication

- Session data is stored directly on the client
- Session data is signed and integrity can be verified
  - server only needs to verify validity
  - does not need to refetch data

![right, filtered](assets/background_7.jpg)

----

### Stateless Authentication

- Pros:
  - Lower server overhead
  - Easy to scale and integrate with 3rd party
    - 3rd party can read session data
- Cons:
  - Session can't be revoked anytime
  - More complex to implement
  - Session data can't be changed until it expires


----

### Stateless Authentication

![inline](assets/jwt_timeline.png)

---

### JWT (JSON Web Token)

> JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties.

- A signed JSON whose validity can be verified by others

----

### Anatomy of JWT

- Header Algorithm & Token type (red)
- Payload (purple)
  - whatever data needed for identification
- signature (blue)

![inline](assets/jwt.png)

----

### Header

- declare type JWT
- declare hashing algorithm to use
  - there are many others (see <https://jwt.io/)>

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

![right, filtered](assets/background_9.jpg)

----

### Payload

- Carries the information which we want to transmit
- Also called JWT claims
- can be read without the secret
  - don't store sensitive data in here!!!

```json
{
  "id": "1234567890",
  "name": "John Doe",
  "roles": ["Admin"]
}
```

![right, filtered](assets/background_6.jpg)

----

### Signature

- Hash of
  - header
  - payload
  - secret
- required for data verification


----

### Anatomy of JWT

![inline](assets/jwt_parts.png)

gets converted to:

![inline](assets/jwt.png)

----

### Pros

- Standard by IETF
- Scalable
  - no DB hit needed for subsequent requests
- Stateless
- Distributable
- Secure against CSRF

---

### JWT in React/Redux

![JWT Flow](assets/jwt_timeline.png)

----

### JWT in React/Redux

- Where would it fit best?

![Redux Overview](assets/redux_overview.png)

----

### JWT in React/Redux

![inline](assets/redux_action_highlight.png)

----

### Receiving a JWT

```ts
const signIn = ({ email, password }) => async (dispatch) => {
    const response = await fetch("/sign_in", {
      "method": "POST",
      "headers": {
        "Accept": "application/json",
        "Content-Type": "application/json"
      },
      "body": JSON.stringify({ user: { email, password }})
    });
    const token = response.headers.get('Authorization')
    dispatch({ type: 'auth/signed_in', payload: { token }}) // still needs to be written
})
```

----

### Sending JWT to backend

```ts
const getUsers = ({ email, password }) => async (dispatch, getState) => {
  const jwtToken = getState().auth.token
  //               ^^^^^^^^^^^^^^^^^^^^^
  // get JWT token from state
  if (!jwtToken) { redirect('/sign_in') }

  const response = await fetch("/users", {
    headers: {
      'Authorization': jwtToken
      // ^^^^^^^^^^^^^^
      // add JWT token to fetch call
    },
  });
  // ...
})
```

----

### Sign out

```ts
const signOut = ({ email, password }) => async (dispatch) => {
  // no http call required (token needs to be removed from state in reducer)
  dispatch({ type: 'auth/signed_out')
})
```

----

### Sign out caveats

- items in store need to be removed manually
  - eg. previous money transactions/users
  - otherwise there is a possible data leak

```ts
const initialState = {}
const userReducer = (previousState = initialState, action) => {
  switch(action.type) {
    // ...
    case 'auth/signed_out':
      return initialState
    // ...
  }
}
```


---

# Authentication with Firebase

![cover, filtered](assets/background_1.jpg)

---
# Firebase
## Install Firebase Tools

- `npm install -g firebase-tools`
- `firebase login`

![right, filtered](assets/background_6.jpg)

---
# Firebase
## Initialize project in firebase

- `firebase init`
- Select features
  - Database
  - Hosting
  - Emulators
- Create new project
  - select name + 5 random characters at the end

![right, filtered](assets/background_8.jpg)

---
# Firebase
## Initialize project in firebase

- What do you want to use as your public directory?
  - enter `build`
- Configure as a single-page app
  - enter `y`
- Set up automatic builds
  - enter `n` for now
- File build/index.html already exists. Overwrite?
  - doesn't matter

---
# Firebase
## Emulator setup

- Select
  - Authentication Emulator
  - Database Emulator

![right, filtered](assets/background_9.jpg)

---
# Firebase
## Emulator setup

```
Which Firebase emulators do you want to set up? Press Space to select
emulators, then Enter to confirm your choices. Authentication
Emulator, Database Emulator?
- Which port do you want to use for the auth emulator? 9099
- Which port do you want to use for the database emulator? 9000
- Would you like to enable the Emulator UI? Yes
- Which port do you want to use for the Emulator UI (leave empty to use any
  available port)?
- Would you like to download the emulators now? (y/N) y
```

---
# Firebase
# Emulator setup

- Adapt start scripts in package.json

```
"start:emulators": "firebase emulators:start",
"start": "concurrently -n firebase,app,storybook 'npm run start:emulators' 'npm run start:app' 'npm run start:storybook'",
```

---
# Firebase
## Get firebase config 1/3

![inline](assets/firebase_select_project.png)

---
# Firebase
## Get firebase config 2/3

![inline](assets/firebase_get_config.png)

---
# Firebase
## Get firebase config 3/3

- paste config in src/firebase.json [^6]

```js
// src/firebase.js

const firebaseConfig = {
  apiKey: "AIzaSyBaUKK6ikaiYN98xIiQiCV-wPIaPCNS26Y",
  authDomain: "full-stack-development-a2479.firebaseapp.com",
  projectId: "full-stack-development-a2479",
  storageBucket: "full-stack-development-a2479.appspot.com",
  messagingSenderId: "338202513827",
  appId: "1:338202513827:web:7ecd7edebc5641f9deca16",
  measurementId: "G-QZ6VWV6785"
};
```

---

# Firebase
# Install Firebase SDK

- `npm i firebase firebase-tools`

```js
// src/firebase.js
import firebase from 'firebase'
import 'firebase/auth'

const firebaseConfig = {
  apiKey: "AIzaSyBaUKK6ikaiYN98xIiQiCV-wPIaPCNS26Y",
  authDomain: "full-stack-development-a2479.firebaseapp.com",
  projectId: "full-stack-development-a2479",
  storageBucket: "full-stack-development-a2479.appspot.com",
  messagingSenderId: "338202513827",
  appId: "1:338202513827:web:7ecd7edebc5641f9deca16",
  measurementId: "G-QZ6VWV6785"
};

firebase.initializeApp(firebaseConfig)
```

---

# Firebase
## Install Firebase SDK

- `npm i firebase firebase-tools`

```js
// src/firebase.js
import firebase from 'firebase'
import 'firebase/auth'

const firebaseConfig = {
  apiKey: "AIzaSyBaUKK6ikaiYN98xIiQiCV-wPIaPCNS26Y",
  authDomain: "full-stack-development-a2479.firebaseapp.com",
  projectId: "full-stack-development-a2479",
  storageBucket: "full-stack-development-a2479.appspot.com",
  messagingSenderId: "338202513827",
  appId: "1:338202513827:web:7ecd7edebc5641f9deca16",
  measurementId: "G-QZ6VWV6785"
};

firebase.initializeApp(firebaseConfig)
```

---

# Firebase
## Initialize firebase auth [^7]

```js
// src/firebase.js
// ...

firebase.initializeApp(firebaseConfig)

export const auth = firebase.auth()

if (process.env.NODE_ENV === 'development') {
//  ^^^^^^^^^^^^^^^^^^^^
// when app is started in development mode
// use the local emulator
  auth.useEmulator('http://localhost:9099')
}
```

---

# Firebase
## Initialize firebase auth

```js
import { auth } from '../../firebase.js'

// with our auth instance we can
// signIn
auth.signInWithEmailAndPassword('email@mail.com', 'superSecret')

// signUp
auth.createUserWithEmailAndPassword('email@mail.com', 'superSecret')

// signOut
auth.signOut()
```

---

# Firebase and Redux


![inline](assets/redux_action_highlight.png)

---

# Firebase and Redux
# build an action creator for sign up

```js
// src/firebase.js
// ...

import { auth } from './firebase'
const signUp = ({ email, password }) => async (dispatch) => {
  const result = await auth.createUserWithEmailAndPassword(email, password)
  dispatch({
    type: 'user/signedUp'
  })
}
```

---

# Homework

- see [wiki](https://wiki.mediacube.at/wiki/index.php?title=Fullstack_Development_-_SS_2021#Friday_April_09.2C_2021_-_Thomas_Mayrhofer)

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://de.surveymonkey.com/r/8TW92LL>



[^1]: /money_transactions/ is called

[^2]: the session for the user is fetched from a db

[^3]: the session information is returned and verified

[^4]: result of /money_transactions/ is returned to client

[^6]: different for each user

[^7]: complete firebase.js https://gist.github.com/webpapaya/d317f23e993a29055766b00074a5a5eb

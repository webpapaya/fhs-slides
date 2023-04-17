# Firebase

![cover, filtered](assets/background_10.jpg)

---

# Firebase
## What is firebase [^5]

> Firebase is a platform developed by Google for creating mobile and web applications.

---

# Firebase
## What is firebase

- set of tools to develop applications
- apps can be created without server side code
- "Backend as a service"

![right, filtered](./assets/background_11.jpg)

---

# Firebase
## Tools

- Analytics
- Authentication
- Database
- Messaging
- Hosting
- File Storage
- ...

![right, filtered](./assets/background_9.jpg)

--- 
# Registration

- go to https://console.cloud.google.com
  - accept terms and services if popup appears
- go to https://console.firebase.google.com

---

# Setup

![inline](./assets/firebase_01.png)

---

# Setup

![inline](./assets/firebase_02.png)

---

# Setup

![inline](./assets/firebase_03.png)

---

# Setup

![inline](./assets/firebase_04.png)

---

# Setup

![inline](./assets/firebase_05.png)

---

# Setup

![inline](./assets/firebase_06.png)

---

# Setup

![inline](./assets/firebase_07.png)

---

# Setup

![inline](./assets/firebase_08.png)

---

# Setup

![inline](./assets/firebase_09.png)


---

# Setup/Deploye Application

```bash
npm install -g firebase-tools 
npx firebase-tools login # alternative without global install

firebase login
firebase init # watch https://asciinema.org/a/ZTa9tGUWAmQDnlWeKnT2n3lXd
```

---

# Setup/Deploye Application

- Firebase hosting makes it easy to deploy web-apps
- execute `firebase deploy`
- or add deploy script to `package.json`

```js
{
  "scripts": {
    // ... other scripts
    "deploy": "npm run build && firebase deploy"
  }
}
```

---

# Setup Webapp

![inline](./assets/firebase_app_01.png)

---

# Setup Webapp

![inline](./assets/firebase_app_02.png)

---

# Setup Webapp

![inline](./assets/firebase_app_03.png)

---

# Setup Webapp

![inline](./assets/firebase_app_04.png)

---

# Setup Auth

![inline](./assets/firebase_auth_01.png)

---

# Setup Auth

![inline](./assets/firebase_auth_02.png)


---

# Setup Auth

![inline](./assets/firebase_auth_03.png)


---

# Setup Auth

![inline](./assets/firebase_auth_04.png)


--- 
# Connect React with Firebase

```
npm i firebase               
npm i @react-firebase/auth
npm i @react-firebase/database
npm i react-firebase-hooks
```

---

# Connect React with Firebase

- Create a file `firebase.js` and add snippet from the UI:

```js
import { initializeApp } from "firebase/app";
import { getAuth } from 'firebase/auth';
import { getDatabase } from 'firebase/database';
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "....",
  authDomain: "....",
  projectId: "....",
  storageBucket: "....",
  messagingSenderId: "....",
  appId: "...."
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
export const auth = getAuth(app)
export const database = getDatabase(app)
```

--- 

# React Firebase Auth
## Sign up

```js
import { auth } from './firebase'
import { useCreateUserWithEmailAndPassword } from 'react-firebase-hooks/auth'

const SignUp = () => {
    const [signUp] = useCreateUserWithEmailAndPassword(auth)
    //               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    // use hook to create a user
    const formik = useFormik({
      initialValues: { email: '', password: '' },
      onSubmit: async (values) => {
        await signUp(values.email, values.password)
        //    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        // create a new user and wait until executed
        // after user was created -> redirect to sign-in (see routing slides)
      },
    });
  
    // ...
}
```

--- 

# React Firebase Auth
## Sign in

```js
import { auth } from './firebase'
import { useSignInWithEmailAndPassword } from 'react-firebase-hooks/auth'

const SignIn = () => {
    const [signIn] = useSignInWithEmailAndPassword(auth)
    //               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    // use hook to sign in with email and password
    const formik = useFormik({
      initialValues: { email: '', password: '' },
      onSubmit: async (values) => {
        await signIn(values.email, values.password)
        //    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        // create a new user and wait until executed
        // after user was signed in -> redirect to app (see routing slides)
      },
    });
  
    // ...
}
```

--- 

# React Firebase Auth
## Sign out

```js
import { auth } from './firebase'
import { useSignOut } from 'react-firebase-hooks/auth'

const SignOutButton = () => {
    const [signOut] = useSignOut(auth)
    //               ^^^^^^^^^^
    // use hook to get a sign out callback
    

    return (
      <button onClick={signOut}>Sign out</button>
    )
}
```

--- 

# React Firebase Auth
## Authentication status

```js
import { auth } from './firebase'
import { useAuthState } from 'react-firebase-hooks/auth'

const SignOutButton = () => {
    const [user, loading] = useAuthState(auth)
    //                      ^^^^^^^^^^
    // use hook to get the current authentication state
    
    console.log(user) // if user is undefined not authenticated
}
```

---

# Firebase Realtime Database

- NoSQL document store
- can sync data with devices automatically
- supports authorization via database rules
- ...

> Realtime Database is Firebase's original database. It's an efficient, low-latency solution for mobile apps that require synced states across clients in realtime.


---

# React Firebase Database
## Setup security rules

![inline](./assets/firebase_database_01.png)

---

# React Firebase Database
## Setup security rules

```json
{
		"rules": {
      // prevent read and write for all users
      ".read": false,
      ".write": false, 

      "users": {
        // create "scope" for each authenticated user
        "$uid": {
          ".write": "$uid === auth.uid", // prevent unauthorized access
          ".read": "$uid === auth.uid"
        }
      }
    }
}
```

---

# React Firebase Database
## Write data to realtime database


```js
import { ref, push } from 'firebase/database'
// ... 

const CreateMoneyTransaction = () => {
  const [user] = useAuthState(auth)
  
  // Get a reference to the document
  const transactionRef = ref(database, `users/${user?.uid}/transactions`)

  const formik = useFormik({
    initialValues: { email: '', password: '' },
    onSubmit: async (values) => {
      // push values to firestore
      push(transactionRef, values);
    },
  });
}
```

---

# React Firebase Database
## Read data from database


```js
import { ref, push } from 'firebase/database'
// ... 

const ListMoneyTransactions = () => {
  const [user] = useAuthState(auth)
  
  // Get a reference to the document
  const transactionRef = ref(database, `users/${user?.uid}/transactions`)

  const [transactions, loading, error] = useObjectVal(ref(database, rootPath))
  //                                     ^^^^^^^^^^^^
  // get object values from reference (will stream updates to UI)
}
```

---

# React Firebase Database
## Update data


```js
import { ref, update } from 'firebase/database'
// ... 

const ListMoneyTransactions = () => {
  const [user] = useAuthState(auth)
  
  const markTransactionAsPaid = (id) => {
    const transactionRef = ref(database, `users/${user?.uid}/transactions/${id}`)
    //                                                                    ^^^^^
    // get reference to transaction

    update(transactionRef, { paidAt: new Date().toISOString() });
    //                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    // update paidAt value
  }
}
```

---

# Firebase

- there are lots of other things to discover in firebase
  - cloud functions
  - advanced authorization of data (via database rules)
  - ...
- we'll look into necessary features for your MMP when required

---

# Links

- https://en.wikipedia.org/wiki/Firebase
- https://firebaseopensource.com/projects/csfrequency/react-firebase-hooks/
  - Auth hooks: https://github.com/csfrequency/react-firebase-hooks/tree/master/auth
  - Database hooks: https://github.com/csfrequency/react-firebase-hooks/tree/master/database

---

# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://s.surveyplanet.com/x1ibwm85>

# Introduction to React

---

# React

- Component based library to declaratively build composable UIs

```js
const Button = () => {
  return (
    <button type="button">
      A button
    </button>
  );
}

// Usage
React.renderComponent(<Button />, document.body);
```

----

# Which types of components do you see?

----

![app](assets/app_wireframe.png)

---

# Different Types of Components

----

- Presentational Components
  - Can be reused in other applications
  - eg. Buttons, Inputs, Headings, ...

----

- Container Components
  - Know about the application and can't be reused easily
    - UserSignIn, SettingsForm, ...


# Props in React

- React promotes a unidirectional dataflow
- Props are passed down to the components
  - State can only be modified via callbacks

```js
const Button = () => {
  return (
    <button onClick={ () => console.log('Heeeeyyyy!!!') } type="button">
      { children }
    </button>
  );
}
```

---

# How to add state

```js
const SimpleForm = ({ onSubmit }) => {
  const [firstName, setFirstName] = useState("");
  return (
    <input
      type="text"
      name="firstName"
      value={firstName}
      onChange={evt => setFirstName(evt.target.value)}
    />
  );
};
```

---

# Task 1

- Fork/clone the following https://github.com/webpapaya/fhs-react-redux-starter-kit
- npm install
- npm run start:storybook
- Reuse/Adapt existing Button
- Create a UserSignIn Form Component
  - Username
  - Password
  - Button is clicked (console.log values)
- If you're done help others

---

# Homework
- Try to identify the following
  - UserSignIn
    - onSubmit => { username, password }

  - UserSignUp
    - onSubmit => { username, password }

  - MoneyTransactionCreate
    - users => { id, name }
    - onSubmit => { debitorId, creditorId, amount }

  - MoneyTransactionList (Lists all Money Transactions)
    - moneyTransactions
    - onMoneyTransactionPaid => { id, paidAt: (new Date()).toISOSTring() }

- You probably need the following core components
  - <TextInput onChange={(value) => console.log(value) } />
  - <DecimalInput onChange={(value) => console.log(value) } />
  - <SelectInput onChange={(value) => console.log(value) } />
  - <Button onChange={(value) => console.log(value) } />
  - ...

- Allowed to use CSS Frameworks
- Not allowed to use

----

![sign_in](assets/sign_in_wireframe.png)

----

![sign_up](assets/sign_up_wireframe.png)

----

![app](assets/app_wireframe.png)

---

# Tools/Resources

- [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=de)
- [React Docs](https://reactjs.org)
- [Overreacted](https://overreacted.io/)


---

# Feedback

https://de.surveymonkey.com/r/J6693VN
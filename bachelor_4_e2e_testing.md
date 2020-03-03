# E2E tests
## (MMT-B2017)

----
### E2E tests
- Test the whole application from start to finish
- Ensure right information is passed between systems
- Hard to setup/maintain
  - Changing submit button label to sign-up might break tests

----
### E2E testing recommendation
- Find a couple of happy paths through the app
  - max 10
- Run those happy paths agains supported browsers
- Many things can go wrong
  - retry your tests

----
# Selenium

- Tool to automate browsers
- Can be used to write e2e tests
- Webdrivers for all major browsers
  - Chrome
  - Firefox
  - IE/Edge
  - Safari

----
### API

- Pretty low level
- Poor documentation
- build own API on top of Selenium API

----
# Setup a new driver

```js
// process.env.SELENIUM_DRIVER one of chrome, edge, safari, firefox
const driver = await new Builder().forBrowser(process.env.SELENIUM_DRIVER).build();

await driver.quit();
```

----
# Navigate somewhere

```js
await driver.get('https://compup.agilesoftware.dev');
```

----
# Click a link

```js
const clickLink = async (driver, href) => {
  const element = By.js(`return document.querySelector('[href="${href}"]');`)
  await driver.wait(until.elementLocated(element))
  await driver
    .findElement(element)
    .click()
}
```

----
# Fill a form field

```js
const fillFormField = async (driver, { name, value }) => {
  const element = By.js(`return document.querySelector('[name=${name}]');`)
  await driver.wait(until.elementLocated(element))
  await driver
    .findElement(element)
    .sendKeys(value) // Sends form values to field
}
```

----
# Click some text

```js
const clickText = async (driver, text) => {
  const element = By.xpath(`//*[text()[contains(.,'${text}')]]`)
  await driver.wait(until.elementLocated(element))
  await driver
    .findElement(element)
    .click()
}
```

----
# Hit Enter in text field

```js
const hitEnterInFormField = async (driver, { name }) => {
  const element = By.js(`return document.querySelector('[name=${name}]');`)
  await driver.wait(until.elementLocated(element))
  await driver
    .findElement(element)
    .sendKeys(Key.ENTER)
}
```

----
# E2E tests
- What would be good E2E tests for
  - https://compup.agilesoftware.dev

----
# Task 1/2
- clone https://github.com/webpapaya/fhs-e2e-tests
- Write the following tests
  - Lending money to somebody else
    - Sign up 2 different users
    - One is lending money to somebody
    - The other pays back
    - Verify sum at top changed to 0

----
### Task 2/2
- Write the following tests
  - Changing username
    - Sign up
    - change username
    - sign out
    - sign in again
    - go to settings
    - verify username

# Debugging JS

![cover filtered](assets/background_3.jpg)

----

# console.log debugging

- Add console.log statements
- Click through the app
- See the log statements in chrome dev tools
  - easy to get started
  - some problems might be tough to track down

![right, filtered](assets/background_1.jpg)

----

# Using Breakpoints

- Chrome/Firefox devtools allow breakpoints
- Breakpoints are a way to pause a programm
  - variables can be inspected
  - custom code can be executed

![right, filtered](assets/background_2.jpg)

----

# Using Breakpoints

- Chrome/Firefox devtools allow breakpoints
- Breakpoints are a way to pause a programm
  - variables can be inspected
  - custom code can be executed

![right, filtered](assets/background_3.jpg)

----

# How to add breakpoints

- Via EventListeners
  - DevTools > Sources > EventListener Breakpoints
- Via DOM events
  - eg. element is removed/added to dom
- In DevTools
  - Sources > find line of code > click on line number
- Via Code -> add debugger statement

----

# Debugging NodeJS

- <https://dev.to/john_papa/debug-your-nodejs-app-in-60-seconds-5cni>

---

# Debugging performance issues

![cover filtered](assets/background_4.jpg)

----

# Chrome performance tab

![inline](assets/flamegraph.png)

----

# Chrome performance tab

- Determine why CPU is busy
- Flame graph Visualisation of
  - call stack
  - duration of fn call

----

# Bottom up

- Which functions had took the longest to run

![inline](assets/bottom_up.png)

----

# CPU Performance

- Open dev tools
- Go to Performance > Record
  - Click stop

----

# Task 1

- clone: <https://github.com/webpapaya/compup>
- npm i && npm run start:development
- Users complain that some button clicks take very long
  - Don't look on commit history =)

---
# Feedback

- Questions: tmayrhofer.lba@fh-salzburg.ac.at
- <https://de.surveymonkey.com/r/8TW92LL>

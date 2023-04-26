footer: FHS (tmayrhofer.lba@fh-salzburg.ac.at)
slidenumbers: true

# MMP2b

![cover, filtered](./assets/background_8.jpg)

---
## User Stories 
### Introduction

- User stories are a tool used in agile software development 
- capture the user's perspective on what the software should do

![right, filtered](./assets/background_8.jpg)

---
## User Stories 
### What are user stories?

- brief description of a feature/functionality that a user expects
- used to communicate requirements between development team (incl. design)
- written from a users perspectice
  - focus on what the user wants to accomplish (problem space)
  - don't focus on how it is done (solution space)


---
## Problem Space vs Solution Space

- software development knows 2 spaces:
  - problem space (what is the problem?)
  - solution space (how do we solve this provlem?)
  
![right, filtered](./assets/background_9.jpg)

---
## Problem Space vs Solution Space
### Problem Space

- tries to understand the users needs, pain points, challanges 
- Questions to ask:
  - What problem are we trying to solve? 
  - Who is the user? 
  - What are their needs and pain points?
- User stories describe the problem space

---
## Problem Space vs Solution Space
###  Solution Space

- come up with ways to solve the problem defined in the problem space
- brainstorm designs/features/functionality of a product
- Questions to ask:
  - How can create a product that meets our users needs?
  - How can we present a feature to our users?
  - What technologies should we use?
  - Do we even need to write custom software?

---
## Problem Space vs Solution Space
### Balancing Problem and Solution Space

- healthy balance between problem/solution space required
- spending too much time in the solution space
  - can lead to features which the user does not need
- spending too much time in the problem space
  - can lead to analysis paralysis
  - no progress actually solving the problem

---
## User Stories 
### What are user stories?

- have 3 different components
  - The user: Who wants to use the software and what is their role?
  - The goal: What does the user want to accomplish with the software?
  - The benefit: Why does the user want to accomplish this goal?

---
## User Stories 
### Examples:

- As a student, I want to be able to search for books in the library so that I can find the materials I need for my research.

![right, filtered](./assets/background_4.jpg)

---
## User Stories 
### Examples:

- As a traveler, I want to be able to book flights and hotels online so that I can plan my trip more efficiently.

![right, filtered](./assets/background_10.jpg)

---
## User Stories 
### Acceptance Criteria:

- specific conditions which must be met for a story to be considered complete
- can be seen as a users expectation to a feature 
- help to prevent misunderstandings
- focus on the problem space

---
## User Stories 
### Acceptance Criteria:

> As a student, I want to be able to search for books in the library so that I can find the materials I need for my research.

- books can be searched by title, author, or subject.
- only available books are returned by the search
- the book location within the library is shown

---
## User Stories 
### Implementation details

- focus on the solution space and document decisions
  - data is fetched via postgres
  - book location needs to be queried by book location service
  - result needs to be paginated

---
## User Stories 
### Tips and tricks

- keep them simple
- focus on the problem space
- don't add implementation details to ACs
- split stories if they appear to take lots of time
  - ideally a story shouldn't take longer than 1 day
- prioritize stories properly 

---
## User Stories 
### Assignment (due date: 1.5 - 12pm)

- create 5-10 user stories (including acceptance criteria) for next week
  - should contain user story/acceptance criteria
  - optional: add implementation details/designs
    - can be added next week
- add them as tickets to your gitlab project
- You can send them to me earlier

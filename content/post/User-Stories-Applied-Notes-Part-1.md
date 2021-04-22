---
title: "User Stories Applied Notes Part 1"
date: 2020-11-08T00:30:14-06:00
lastmod: 2020-11-08T00:30:14-06:00
draft: false
keywords: []
description: ""
tags: ["notes", "user story"]
categories: ["Product Management"]
author: ""

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: false
autoCollapseToc: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false
---

<!--more-->



Notes from **User Stories Applied** for Agile Software Development

by **Mike Cohn**

## Part 1 Intro

### 1.1 An Overview

#### What is a user story?

A user story describes functionality that will be valueable to either a user or purchaser of a system or software. Use stories are composed of **three aspects**:

- A wrtten description of the story used for planning and as a reminder.
- Conversations about the story that serve to flesh out the details of the story.
- Tests that convey and document details and that can be used to determin when a story is complete.

Stories should be written so that the customer can value them.

#### Where are details?

Many of these details can be expressed as additional stories. In fact, it is bet- ter to have more stories than to have stories that are too large.

When a story is too large it is sometimes referred to as an *epic.* Epics can be split into two or more stories of smaller size. For example, the epic “A user can search for a job” could be split into these stories:

- A user can search for jobs by attributes like location, salary range, job title, company name, and the date the job was posted.
- A user can view information about each job that is matched by a search.
- A user can view detailed information about a company that has posted a job.

#### "How long does it have to be?"--How to know when you're done

If you’re using paper note cards, you can turn the card over and capture these expectations there.

- Try it with an empty job description. 
- Try it with a really long job description.
- Try it with a missing salary.
- Try it with a six-digit salary.

#### The customer team

The customer team includes those who ensure that the software will meet the needs of its intended users. This means the customer team may include 

- testers, 
- a product manager, 
- real users, and
- interaction designers.

On a story-driven project is that customers and users remain involved throughout the duration of the project.

#### The Process

- Brainstorms many stories
- Select an iteration length
- Developers estimate velocity
- Prioritize stories
- Make adjustments

#### Planning Releases and Iterations

A release is made up of one or more iterations. 

Release planning refers to deter- mining a balance between a projected timeline and a desired set of functionality. 

Iteration planning refers to selecting stories for inclusion in this iteration.

#### What are acceptance tests?

Acceptance testing is the process of verifying that stories were developed such that each works exactly the way the customer team expected it to work. 

A dedicated and skilled tester should be included on the customer team for the more technical of these tasks.

#### Why change?

- User stories emphasize verbal rather than written communication. 

- User stories are comprehensible by both you and the developers. 

- User stories are the right size for planning.

- User stories work for iterative development.

- User stories encourage deferring detail until you have the best understand-

  ing you are going to have about what you really need.

### 1.2 Writing Stories

A good story is

- Independent

  - avoid introducing dependencies between stories.
  - Combine the dependent stories into one larger but independent story. 
  - Find a different way of splitting the stories

- Negotiable

  - story card as con- taining:

    • a phrase or two that act as reminders to hold the conversation

    • notes about issues to be resolved during the conversation

- Valuable to users or customers

  - ßavoid are stories that are only valued by developers.
  - it is worth attempting to keep user interface assump- tions out of stories

- Estimatable

  - There are three common reasons why a story may not be estimatable:
  - Developers lack domain knowledge. 
  - Developers lack technical knowledge. 
  - The story is too big.

- Small

  - Epics typically fall into one of two categories: 
  - The compound story
  - The complex story
  - When possible, it works well to put the investigative story in one iteration and the other stories in one or more subsequent iterations.
  - Combine stories that are too small.

- Testable

  - Whenever possible, tests should be automated.

### 1.3 User Role Modeling

A **user role** is a collection of defining attributes that characterize a population of users and their intended interactions with the system. 

**Role Modeling Steps**

We will use the following steps to identify and select a useful set of user roles:

- brainstorm an initial set of user roles
- organize the initial set
- consolidate roles
- refine the roles
  - The frequency with which the user will use the software.
  - The user’s level of expertise with the domain.
  - The user’s general level of proficiency with computers and software.
  - The user’s level of proficiency with the software being developed.
  - The user’s general goal for using the software. Some users are after conve- nience, others favor a rich experience, and so on.

**Personas**

Creating a persona requires more than just adding a name to a user role. A persona should be described sufficiently that everyone on the team feels like they know the persona. 

**Extreme Characters**

Speficially, Djajadiningrat suggests designing the PDA for a drug dealer, the Pope, and a twenty-year-old woman who is juggling multiple boyfriends.



### 1.4 Gathering Stories

Some of the most valuable techniques for creating a set of stories are:

- User interviews
- Questionnaires
- Observation
- Story-Writing workshops
  - What will the user most likely want to do next?
  -  What mistakes could the user make here?
  -  What could confuse the user at this point?
  -  What additional information could the user need?



### 1.5 Working with User Proxies

When we can- not get as many users as we want to represent different perspectives of the product, we need to resort to *user proxies*, who may not be users themselves but are on a project to help represent users.

- In product companies, the customer frequently comes from the marketing group. Someone from the marketing group is often a good choice as a user proxy but must overcome the temptation to focus on the quantity rather than quality of features.
- Salespeople can make good customers when they have contact with a broad variety of customers who are also users. Salespeople must avoid the temptation to focus on whatever story could have won the last lost sale. In all cases, salespeople make excellent conduits to users.

### 1.6 Acceptance Testing User Stories

Testing is best viewed as a two-step process: First, notes about future tests are jotted on the back of story cards. This can be done any time someone thinks of a new test. Second, the test notes are turned into full-fledged tests that are used to demonstrate that the story has been been correctly and fully coded.

**“A company can pay for a job posting with a credit card”** may have the following written on the back of its card:

- Test with Visa, MasterCard and American Express (pass). • Test with Diner’s Club (fail).
- Test with good, bad and missing card ID numbers.
- Test with expired cards.
- Test with different purchase amounts (including one over the card’s limit).

**Types of Testing**

- User interface testing, which ensures that all of the components of the user interface behave as expected
- Usability testing, which is done to ensure an application that can be easily used
- Performance testing, which is done to gauge how well the application will perform under various workloads
- Stress testing, in which the application is subjected to extreme values of users, transactions, or anything else that may put the application under stress



### 1.7 Guidelines for Good Stories

- To identify stories, start by considering the goals of each user role in using the system.

- When splitting a story, try to come up with stories that cut through all lay- ers of the application.

- Try to write stories that are of a size where the user feels justified in taking a coffee break after completing the story.

- Augment stories with other requirements gathering or documenting tech- niques as necessary for the project’s domain and environment.

- Create constraint cards and either tape them to a shared wall or write tests

  to ensure the constraints are not violated.

- Write smaller stories for functionality the team will implement soon, and write broad, high-level stories for functonality further into the future.

- Keep the user interface out of the stories for as long as possible.

- When practical, include the user role when writing the story.

- Write stories in active voice. For example, say “A Job Seeker can post a resume” rather than “A resume can be posted by a Job Seeker.”

- Write stories for a single user. Instead of “Job Seekers can remove resumes” write “A Job Seeker can remove her own resumes.”

- Have the customer, rather than a developer, write the stories.

- Keep user stories short, and don’t forget their purpose as reminders to hold conversations.

- Don’t number story cards.




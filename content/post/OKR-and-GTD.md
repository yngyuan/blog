---
title: "OKR and GTD"
date: 2020-05-09T10:14:19-05:00
lastmod: 2020-05-09T10:14:19-05:00
draft: false
keywords: ["OKR","GTD","Productivity"]
description: "My guide to OKR and GTD"
tags: ["OKR","GTD"]
categories: ["Productivity"]
author: "Yang Yuan"

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

# OKR and GTD

During the COVID-19, our lives have changed and we are facing the unprecedented chanllenge of my generation. All my classes have moved online and I am stuck at home. It started fine and I kind of enjoyed a little more time with myself. However, as time goes by, I found that it is hard to keep motivated and focused when staying at home for the most of the time. 

To make matters worse, many companies have froze their hiring process and started layoff and furlough. There couldn't be a worse time for international students like me to graduate. I was depressed at this at first and then realized there really isn't much that I could do to change the whole situation. What I can do is to be better prepared and be the best I can so that when opportunity comes I could take it. 

I  find goal systems and task management frameworks inspiring and useful. I hope this is inspriring for you as well.

## What is OKR

OKR means Objectives and Key Results. OKR is a goal system used by google and others. It is a simple tool to create aligment and engagement around measurable goals.

**Objectives** are memorable qualitative descriptions of what you want to achieve. 

- short
- inspirational
- engaging
- challenging

**Key Results** are a set of metrics that measure your progress towards the Objective.

- 2-5 key results
- quantitative
- measurable
- limited time

## How to OKR

To achieve our Objective, we need to break it down to Key Results that we need. And the breaking down needs to follow the SMART principle so that your Key Results are good and you are on the right path. 

- Specific
- Measurable
- Attainable
- Relevant
- Time-Based

Also, don't have too many Objectives and Key Results. No more than 5 Objectives and no more than 4 Key Results for each of the Objective.

## My OKR

I would love to share my OKR with you and you can compare this with the principles above to see if this is a good example of OKR.

**Objective:** Find a SWE job in this trying times.

**Key Results:**

- Algorithms - done 500 leetcode questions with 50% of medium and hard questions in 4 months.

- Engineering - done 5 projects about distributed systems in 4 months. 

- Design - done 100 OOD and System Design problems in 4 months.

- Scrappy - applied to 150 companies, used referal for 50% of them before September. 

## What is GTD

Now we got our Objective and Key Result, we need to make sure that we excute accordingly and efficiently. GTD is just the right tool for us. 

"GTD means "Getting things done". It is a framework for orgnizing and tracking your tasks and projects. What GTD gives you—when understood and implemented properly—is a foolproof system for keeping track of what you need to do, should do, or should *consider* to do. "

For everything we do, there are 5 steps we must have done. For example we want to have a party.

- **Collect:** collect infor on how many people are coming, what do they like to eat
- **Judge:** judge how much food we need, what preferences we can meet
- **Arrange:** remember food and ingredients to get
- **Review:** check if we missed something
- **Excute:** excute accordingly

The first 4 steps are preparation period, the more we prepare, the better we could excute. GTD is here to help us make good preparations--so good that we trust it with our life and can act blindly in the final step.

## How to GTD

As mentioned above, GTD is here to optimize the first 4 steps. 

- **Collect:** we use one **in list**. This could be either on paper or on the notes app. We put everything and any information that needs to be dealt with on this "in list". This is different from a to-do list in that this lists EVERYTHING that could occupying your brain power, not just things to do.

  - Everything, literally everything
  - 2 seconds. It shouldn't be taking more than 2 seconds to write one item down in any way. 
  - Clear the list once in a while, ideally every day.

- **Judge:** 

  We judge those items and put labels on them. To better illustrate I use psudo python code.

  ```python
  labels = ["useless", "might do", "might use","2-min", 
            "multi-step", "others do", "I do",  ]
  def gtd(item):
  	for item in in_list:
    	read(item)          
      # have a glance
    	if not isExcutableNow(item):
      	item.label = "useless" or "might do" or "might use"
    	else:
        
      	if doneIn2mins(item):
        	item.label = "2-min"              
          # can be done in 2 minutes
      	else:
          
        	if not doneIn1step(item):
          	item.label = "multi-step"        
            # more than 2 minutes but 1 step, aka. project
        	else:
            # could be done in 1 step, also called a task
            
            if dobyme(item):
            	item.label = "I do"
          	else:
            	item.label = "others do"
  	return item
  ```

After this, we have judged and labeled eveything in our in-list.

- **Arrange:** In this step, we take items out of in-list and do stuff about them according to its label. For this step, we need:

  Before we do anything, clear everything tha is marked **"useless"**, delete it, never think about it.

  Next, just do the damn **"2-min"** action, it won't take you long!

     - Wish List: put **"might do"**. items that open with "I want to".
     - Project List: put **"multi-step"** , these are the complicated things. dedicate a new page for each item. also put relevent actions on Task List, Wait List and Calender under the new page.
       - further decompose the item, predict more actions required that could be done in 1 step, put to corresponding **Task List**,**Wait List** and **Calender**. 
       - Althought there are duplicate, put to project list helps you check if you missed anything in one project. 
     - Task List: put **"I do"**, and if there is a specific date, put on Calender.
     - Wait List: put **"others do"**, just keep track of things that depends on others so we may give a heads up.
     - Archive: put **"might use"**, information that we might use someday. put in things like evernote, or bookmark 
     - Calender

- **Review:** Review once a week.
  - if **in-list** is empty.
  - If things in **Wish List** could become actionable item.
  - check progress in **Project LIst**, make sure there is at list one task under a project
  - check if **Wait List** needs to be pushed, thus become an action.
  - cross the finished items and clear.

- **Excute:** Excute from Task List and Calender, Do Calender first and then any abrupt new tasks.



**Every day**, just put down anything that's on your mind, any new ideas or new information on **in-list**. And Excute frin Task LIst and Calender.

**Every night**, before go to bed, do the judge process.

**Every Weekend**, Review.

## From me

These frameworks provide a basic guideline. The implementation varies from person to person. We can all make adaptions that suit us best. I hope you find these useful. 

## References

https://felipecastro.com/en/okr/what-is-okr/

https://hamberg.no/gtd


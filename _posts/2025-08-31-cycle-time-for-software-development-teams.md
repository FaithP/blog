---
layout: post
title: "Cycle Time for Software Development Teams"
date: 2025-08-31
---

Flow metrics—cycle time and lead time—give teams immediate information and feedback they can use to improve their process. They show the effects of process experiments faster. And they encourage team behavior that leads to the results managers care about—deliver more, faster, and with production reliability.<!--more-->

Cycle time is how long it takes to finish something once work on it has begun. You can probably get this in the form of a control chart from your work tracking system "for free," just as a byproduct of using it to coordinate the team's work.

* Azure DevOps (ADO), for example, populates a cycle time widget with work item start and end data.
* Jira offers multiple ways to see the data or control chart, depending on your Jira version, integrations, and plugins.

Retrospectives are a great time to focus the team's attention on the chart. The game is to make cycle time trend lower. First, the team should determine what the theoretical lower bound is. In one team I worked with, for example, a routine change would be deployed to production in not less than 5 days from the time a developer started to work on it. The team can now talk about what they could do to bend their cycle time toward that minimum. The effects of their actions will be visible at the very next retrospective.

Two factors that typically inflate cycle time are rework time and wait time.

* **Rework Time** is time spent on do-overs, maybe because of code review feedback or QA observations. The later in the process the need for rework is found, the more time rework takes. For example, a QA finding remediation has not only a developer rework cost but also rework in QA. If a team can reduce rework, their cycle time will tend to shorten.
* **Wait Time** is time that work is either on hold or interrupted. Some of that shows up in Cycle Time. For example, let's say I've started work on a change but have to stop to fix the QA finding on the previous change. Or maybe I start a new change while I wait for an answer to a business question on my current work. Now the cycle time meter is running on both items, but I'm only actively working on one or the other. Cycle time for both increases.

There are several experiments the team could try. They often don't even need actual numbers. Once aware of these factors teams will notice when they happen and propose practical changes. It's more important for the team to try than to wait for detailed process analysis. (The team will be more invested in the success of experiments they *want* to try.)

"But won't developers just do all the work *before* they move their card to In Progress?" I hear you say. "Cycle time would be artificially low." A team *might* try that, but we have another tool in our metrics toolkit: Lead Time. I passively show lead time without discussing it the first time I introduce cycle time. If developers try to game the metric, the apparent cycle time improvement will not show a corresponding decrease in lead time. The static lead time metric reveals that whatever the team did to shorten the measured cycle time did *not* actually improve overall delivery time.

I like to make cycle time and lead time visible when I work with teams. They prompt the right questions and encourage the right behavior. They don't cost me or the team anything to produce, so why not put them out there and see what the team can achieve with them?

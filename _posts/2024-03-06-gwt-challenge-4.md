---
layout: post
title: "First, Solve the Right Problem"
date: 2024-03-06
---


# First, Solve the Right Problem 

#### _(Given-When-Then Challenge #4)_

[The latest SpecFlow Given-When-Then community challenge (#4 in the series)](https://specflow.org/2020/should-the-feature-description-look-like-a-user-story-givenwhenthenwithstyle/) asks about feature descriptions. The challenge asks three different questions:

1. Should the feature description look like a user story?
2. How should features, scenarios, and their descriptions evolve over time as new personas and needs surface?
3. How to write a good description for a feature or scenario? …. How to structure those descriptions? Should they look like user stories or something else? What should they contain?

By the time I had read to the end I knew I wanted first to explore the generic/vague anti-pattern, define “reports,” discuss finding the right problem to solve, and explore how to link solution ideas to a person's needs. Only after I fully understanding these problems and questions would I be ready to offer  thoughts on the challenge’s central theme of descriptions (which is in [Part 2 here](https://www.linkedin.com/pulse/given-when-then-challenge-4-part-2feature-scenario-faith-peterson)).

<!--more-->

# Recap the Challenge
Let's start with an short outline of how [the challenge](https://specflow.org/2020/should-the-feature-description-look-like-a-user-story-givenwhenthenwithstyle/) was presented:

1. People sometimes place user stories in Features as feature descriptions. (For those unfamiliar with this Gherkin capability, I can add any text immediately after a scenario or feature title. This helps human readers and automation ignores it.)
2. Over time, other personas and needs than were first described or envisioned surface. People add scenarios to the Feature even though they don't support the original user story.
3. Next the post offers an example, a pending invoice report for an account manager. We add more goals for the account manager. We also add goals for client managers, call center operators, and accountants. The user-story-as-feature-description becomes more generic, but ends up too vague.
4. Finally, the post asks a different question: "How to write a good description for a feature or a scenario" when a feature evolves.

_The source of this apparent feature-and-scenario-description problem lies upstream in how the business need was analyzed and solution identified._

# The Generic-and/or-Vague Anti-Pattern
As the challenge’s introduction implies, trying to write something [vague](https://www.ecosia.org/search?method=index&q=define%20vague) is often an indication that something has gone wrong with my analysis. It means I need to stop, back up, and either find a new way to organize the information or look for a useful abstraction that is itself specific. If my story is to be testable, it cannot also be unclear, ambiguous, or indefinite.

When “[generic](https://www.ecosia.org/search?q=define%20generic)” means “lacking specificity” it, too, illustrates this anti-pattern. The alternative definition of describing or relating to “an entire group or class” can be tempting. A group or class that would contain account managers, client managers, call center operators, and accountants, however would be pretty broad - Employees? I could try “Report Consumers” or “Report Readers” to make a story something like:
```
So that I can control client credit risk
Or Approve discount levels
     Or help customers faster
     Or prepare end-of-year tax returns
As a Report Reader
I want a pending invoice report
```
Some issues with this approach are:
* the attempted Report Reader generalization assumes the solution
* the “Or” clauses make the motivations in the “So that” clause less useful.

I could double down and make this clause more generic, too, by saying “So that I can do my work.”

But I also can’t tell from the story what elements of the report enable them to achieve those goals, so I might want to start “And”-ing elements on the “I want” clause:
```
So that I can do my work
As a Report Reader
I want a pending invoice report
     And the report should include subtotals and quarterly averages
     And the report should include (another thing)
     And the report should include (something else)
```
These contortions to find generic statements that allow in all the things I want to add to this feature file are evidence, like the [epicycles](https://faculty.wcas.northwestern.edu/infocom/Ideas/epicycle.html) of [the Ptolemaic universe](https://www.youtube.com/watch?v=EpSy0Lkm3zM), that I need to re-think my approach.
![Image](https://i.snap.as/CqEzxLxl.jpg)
_Kepler's diagram of retrograde motion of Mars illustrating epicycles_

# The Trouble With “Reports”
Let’s talk about what a “report” is. In the old days I think a report typically meant printed data distributed for use by people who didn’t have direct access to the system where the data was stored. That idea seems quaint today. So what does creating a report mean today? How is it different from a query result, or the result of filtering a view? Is it meant to be consumed on a screen or device? Is it meant to be exported for use in another program? Read as a PDF? Are people meant to interact with it? How current is the data in it expected to be?

I think of a report this way:

* A report is a view of data for more than one client or other entity that has been collected, aggregated, and organized for presentation, printing,  or export.
* This view might offer limited interactions (sorting and filtering, possibly column selection).
* The data in the report might not be completely current in that the report might pull data from a data store that is not updated in real time.
* A report typically does not offer any ability to edit the data it presents.

People rarely want a report when they need to perform day-to-day operational tasks. They might want a report-like view to help them locate and navigate to specific data they want to work with, or to give them an overview of the current state of play for a certain set of entities so they can decide what needs their attention first, or if something is ready for their action. In my practice I try to avoid the concept of reports (a solution) and instead focus on identifying the need: the views and data (and data combinations) that map best to the way people approach, make decisions about, and perform their work.

# What Account Managers Want
We're told that we implemented a Pending Invoice Report and we're told for whom and their goal. We could write it in Connextra format like this:
```
So that I can control client credit risk
As an account manager
I want a pending invoice report
```
Whatever this report is, it’s probably not actually what our account manager wants unless they’ve been conditioned to think of their needs as reports by the systems they use and the people who run those systems. Our first step should have been to understand what the account manager is doing when they need the pending invoice report by exploring questions such as:

* What is the activity called “control client credit risk”? Is this something that happens client-by-client or in batches?
* Is the account manager interested in whether a specific client has any unpaid invoices?
* What else are they looking at or referring to when they’re “controlling credit risk”? Maybe they are deciding whether to approve a client’s new purchase, or extend more credit. So they’re probably looking at a credit application or an order.
* Does “controlling credit risk” include making courtesy calls toward the end of the billing cycle if a payment hasn’t been received? If so, maybe they want to see a list of all their clients who have pending invoices along with client contact information.

Understanding what the account manager is really trying to do is essential to providing the correct solution. So I would first propose a stronger user story such as this example:
```
So that I can control client credit risk
As an account manager
I want to see whether the current line of credit increase applicant has any pending invoices.
```
Now we can see that maybe adding this information to some other view would have met the account manager’s needs better. But what about the new requests that come pouring in? Imagining the user stories I might write based on the information in the challenge suggests that the actual context of use and true need has been lost here, too.

<table>
<thead>
  <tr>
    <th scope="col" style="width: 40%">As described in challenge</th>
    <th scope="col">Proposed story</th>
   </tr>
</thead>
<tbody>
  <tr>
    <td style="vertical-align: top">A regular customer might be eligible for a discount, so client managers might want to know quarterly subtotals and averages to decide on discount amounts.</td>
    <td style="vertical-align: top">So that I can determine if my regular customer is eligible for a discount
And so that I can decide on the discount amount for my regular customer
As a client manager
I want to know quarterly subtotals and averages
</td>
  </tr>
  <tr>
    <td style="vertical-align: top">Call centre operators might need a few tweaks to solve client problems faster</td>
    <td style="vertical-align: top">So that I can solve client problems faster
As a call center operator
I want (some (as yet unknown) changes to the Pending Invoice Report
</td>
  </tr>
  <tr>
    <td style="vertical-align: top">Accountants might use it to prepare end-of-year tax returns</td>
    <td style="vertical-align: top">So that I can prepare end-of-year tax returns
As an accountant
I want to use the Pending Invoice Report ("use" subject to more discovery)
</td>
  </tr>
</tbody>
</table>

# Conclusion
If you got this far, thank you for accompanying me on this slightly rant-y exploration of some common analysis anti-patterns:

* **Vagueness:** don’t settle for unclear or undefined behavior or outcomes. Think about making testable assertions.
* **Genericization:** though useful, must be handled with care to find generalizations that are actually groups or classes that share something meaningful or useful in common - something other than your intention that they all use the solution you’re proposing!
* **Reports:** at best a “report” is a solution, not a need.
* **Solution-Before-Understanding:** use the journalistic “why”s. **Who** needs **what** data, **when** and **where** do they need it, and **why** do they need it (what will they do with it or because they’ve seen it).

Now, on to what I hope will be some constructive [suggestions for structuring descriptions for feature files and scenarios](https://www.linkedin.com/pulse/given-when-then-challenge-4-part-2feature-scenario-faith-peterson)!

---

_This article was first posted to LinkedIn on July 5, 2020._

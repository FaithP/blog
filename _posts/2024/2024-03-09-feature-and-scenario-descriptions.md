---
layout: post
title: "Feature and Scenario Descriptions"
date: 2024-03-09
---

#### _(Given-When-Then Challenge #4)_

TL;DR
* Frequently maintain and refactor
* Split feature files and organize in folders
* Don't limit yourself to Connextra user story format in the description
* The best description might be no description!
<!--more-->
## Introduction
Thanks to the #GivenWhenThenWithStyle community challenge I’ve been thinking more about what seem to have been some good practices, why they seem to have been useful, and how I’ve decided when to use various approaches or techniques in the business layer of my features. The fourth challenge, [_Should the feature description look like a user story?_](https://specflow.org/2020/should-the-feature-description-look-like-a-user-story-givenwhenthenwithstyle/), piqued my interest for a couple of reasons. First I reflected on what I think are some discovery anti-patterns, and wrote about that in [_First, Solve the Right Problem_](https://write.as/faithpeterson/given-when-then-challenge-4) Here I’ll offer my contributions to the discussion about feature and scenario descriptions. 

The challenge asks a couple of different questions:

1. How to write a good description for a feature or scenario? How to structure those descriptions? Should they look like user stories or something else? What should they contain”
2. How should features, scenarios, and their descriptions evolve over time as new personas and needs surface?

## How should descriptions evolve over time?
If you do nothing else, refer to [Kamil Nicieja](https://kamil.fyi/projects/)'s excellent [_Writing Great Specifications_](https://www.manning.com/books/writing-great-specifications) (Manning, 2017). The entire second half of the book deals with organizing and maintaining your library of business specifications. I'm indebted to Nicieja and his book for a great deal of what follows.

In our challenge, business needs for people not included in the original user story have surfaced. Our hypothetical analyst wants to know how to change the story, which they included as a feature description, to accommodate the new information. I think the answer here is not to change the description but to refactor the feature file.

I'll take as given that the challenge's starting point, a feature file that describes a Pending Invoice Report feature, is valid. (See [Part 1](https://write.as/faithpeterson/given-when-then-challenge-4) for the counterpoint to this.) That feature file probably had multiple scenarios already. The challenge wants us to imagine adding scenarios such as including quarterly subtotals and averages. (This is the only way the dilemma about changing the user story makes sense.) We would end up with something like this snippet:
```
Feature: Pending Invoice Report
So that I can control client credit risk
As an account manager
I want a pending invoice report

Scenario: I have permission to view the report
Scenario: I don't have permission to view the report
Scenario: There are no pending invoices to report
Scenario: The data source isn't available
Scenario: Report displayed with default columns
Scenario: Report displayed with personalized columns
New Scenario: Report includes quarterly subtotals
New Scenario: Report includes quarterly averages
```
One possibility might be to just add the new user stories at the top. But if this is the challenge's hypothetical, I would suggest refactoring the feature into multiple feature files. Various parts of the report can be represented as their own features. The result might resemble this:
```
|_ Features
  |_ Reports
    |_ Pending Invoice Report
      |_ report_request.feature
      |_ report_elements.feature
      |_ report_columns.feature
      |_ report_detail.feature
      |_ report_summary-or-aggregate_data.feature
      |_ report_filters.feature
      |_ report_sorting.feature
```
Each feature could then have its own description
## Description Structure
Sometimes include a user story as a feature or scenario description. In the first snippet above, a scenario description I might write for the second scenario (no permission) could be 

>As Acme Financial, I want to prevent unauthorized people from seeing pending invoice information so that I can protect customer privacy and comply with legal and regulatory requirements.

I've used alternative structures, too, though. Sometimes it's useful to combine these approaches. Here are some examples.

### 
```Narrative
Feature: Pending Invoice Report
Sabine, an account manager, maintains Acme Financial's credit risk exposure within defined levels. One way she does this is to closely monitor whether customers who have a history of slow payment have paid their current invoices or if those invoices are at risk of becoming past due. When she does this, she wants to see a list of all customers with billing dates within the next three business days with invoices that are pending. (Pending means the invoice has been issued and payment has either not been received or not been settled.)
```
### Definitions
```
Feature: Pending Invoice Report
* Account Manager: person who manages Acme's relationship with a specific set of customers
* Pending Invoice: invoice that has been issue but has not yet been paid
 ```    
### Business Rules
Listing business rules is often more useful in scenario descriptions.
```
Feature: Pending Invoice Report

* Rule: Account Managers may only see pending invoices for their assigned customers
* Rule: Account management supervisors may see pending invoices for customer assigned to the account managers they supervise

### Could The Best Description be No Description?
Every word and letter I add to the feature file creates risk. The less I write, the less risk. The more I write, the greater potential for conflicting or confusing specs. (And I, or my successors, have to maintain it all to be current, correct, and consistent). Ideally whatever I write in descriptions must be included in, or at the very least be consistent with, what I document in the Given-When-Then statements that will be automated. That creates the possibility of disconnect drift over time, but also creates an opportunity to push some of the description into the Scenario names and descriptions.

After the Given-When-Thens have been created with meaningful examples, the descriptions might have served their purpose, while Feature and its Scenarios tend to endure. 
* Write names with care. This will pay off as you maintain the contents—not least if you're able to evangelize "living documentation" where these names will form a table of contents. 
* In scenario outlines, use comment columns as suggested in the solution to last week's challenge. 

Use descriptions when
* business stakeholders or other people outside the delivery team need them to use the documents.
* you need to avoid maintaining information in more than one place. Dividing or duplicating information only creates overhead. You want to create one source of product truth. 
* you need to keep an informal record of decisions, new rules, or new information as understanding of the current request evolves, or as new needs surface.

## Conclusion
Feature and scenario descriptions have value, but they do have the potential to create problems if not handled carefully.

* Define a "feature" at the right level of granularity.
* Maintain and refactor aggressively. Split feature files as often as neeed.
* Organize feature files in folders.
* Use descriptions sparingly, or use them at specific times while the team's understanding is developing.
* Use a variety of styles for documentation to communicate as effectively as possible.
* Document less but make it count more by naming features and scenarios well.
* When in doubt, leave it out!
* Better yet, check out [Kamil Nicieja's book](https://www.manning.com/books/writing-great-specifications)!

# Acknowledgements
I remain grateful to Ryan Royal, Jeff Siver, and Nermin Dibek for supporting and encouraging my interest in behavior-driven-development and specification-by-example. Their collaboration has deepened and sharpened my understanding and practice.

---
_An earlier version of this article was first posted to LinkedIn on July 5, 2020._

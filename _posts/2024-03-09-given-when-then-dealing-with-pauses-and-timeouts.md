---
layout: post
title: "Given-When-Then: Dealing with pauses and timeouts"
date: 2024-03-09
---

[Gojko Adzic](https://gojko.net/about/) and the good people at SpecFlow published a series of posts on advanced topics in Given-When-Then scenario writing called _Given-When-Then With Style: The Community Challenge_. And, good news for us practitioners, the "community challenge" part means it's an all-skate! Each week's post describes a specific situation that some find difficult to express in scenarios. Anyone can propose a solution. The following week's post considers some of the proposals, describing drawbacks and benefits of each, and presenting a detailed exposition of a top solution. I highly recommend [the series](https://specflow.org/learn/given-when-then-with-style/).

This week's [Pauses and Timeouts challenge](https://specflow.org/2020/how-to-deal-with-pauses-and-timeouts-givenwhenthenwithstyle/) is on handling situations where there is a pause or timeout in the activity. I've dealt with many situations like this, and handled it in various ways in scenarios. In one case, a shopping checkout required payment processing which eventually delegated to external service provides. In another example, an application queued report requests and notified the requestor to retrieve their report once it was ready. More recently, I worked with a system in which data modified in a mobile app was expected to appear for workers in a different role who were using a Web application to manage operations. As Adzic notes in [the Pauses and Timeouts challenge](https://specflow.org/2020/how-to-deal-with-pauses-and-timeouts-givenwhenthenwithstyle/), this situation "is symptomatic of working with an asynchronous process, often an external system or an executable outside of your immediate control."

Here are a couple of approaches I've used. 
<!--more-->

## Option 1: Raise the level of abstraction and ignore the pause
This has worked for me when my primary interest is verifying the human-observable result of an integrated system, and I'm less interested in verifying internal operations or hand offs. For this to work, characteristics that lead to success or failure have to be in the Background or Givens. Using the challenge's example of a checkout process with a risk evaluation, it might look something like:
```
Scenario: Successful Checkout
  Given an order in checkout
      And a shopper with a satisfactory risk profile
      And a valid form of payment
  When the shopper submits the order
  Then the shopper sees the advice that their payment has been accepted
      And the system has sent a payment capture request
      And the system has sent the shopper's order for fulfillment. 

Scenario: Unsuccessful Checkout - not authorized

  Given an order in checkout
      And a shopper with an UNsatisfactory risk profile
      And a valid form of payment
  When the shopper submits the order
  Then the shopper sees the advice that the system can't accept their order
      And the shopper's payment method is not charged

Scenario: Unsuccessful Checkout - indeterminate order processing outcome 

  # External system is unavailable

  Given an order in checkout
      And a shopper with a satisfactory risk profile
      And a valid form of payment
  When the shopper submits the order
  Then the shopper sees the advice that the system can't complete their order
      And the shopper sees the customer service contact information
      And the shopper's payment method is not charged
```
## Option 2: Write separate features for the various elements of the process
I've done this when my foremost concern is testing at system or component boundaries, or when I'm interested in isolating the point of process failure when there is a problem. This approach also puts the focus on impediments to a valid request rather than on the overall integrated process. It does require many more features and scenarios, and it does require a shifting definition of what "externally observable behavior" means, but it remains human-readable and enables the features to be organized for use by different dev teams who might be collaborating on the overall solution. Testing with scenarios structured this way generally means inputs from systems outside the scope of the feature will have to be mocked, so it's no longer a true end-to-end acceptance test. But for dev teams who want to ensure that their applications still meet expectations of collaborating systems, this could be a useful approach that might complement contract tests or other assurance techniques.

### Feature 1: Submit Order at Checkout
```
Scenario: System Accepts Order

  Given an order in checkout
      And the order has a valid shipping address
      And the order has a valid form of payment
  When the shopper submits the order
  Then the system accepts the order for processing
      And the system sends a request for payment authorization
      And the shopper sees an advice that their order is waiting for payment authorization

Scenario: System Can't Accept Order

  Given an order in checkout
      And the order has no delivery destination
      And the order has a valid form of payment
  When the shopper submits the order
  Then the system does not initiate order processing
      And the shopper sees and advice that they must provide shipping information
```
### Feature 2: Authorize Payment
```
Scenario: Payment is Authorized

  Given a request for payment authorization
    And a valid and well-formed credit card number
    And a credit card expiration date in the future
    And the correct CVV code for the credit card
    And the correct postal code for the credit card account
  When the system submits the request for payment authorization
  Then the system receives an authorization code
    And the system advises the cart application that the payment has been authorized
```
### Feature 3: Initiate Fulfillment
```
Scenario: Order Satisfies Fulfillment Conditions

  Given an order waiting for payment authorization
  When the cart receives a payment authorization advice
  Then the cart application validates the payment auhtorization code
      And the cart submits the order for fulfillment
      And the shopper sees an order confirmation advice
      And the system sends an order confirmation email
```  

# Conclusion
So there you have it - two ways to describe behavior that necessarily includes either intentional or possible pauses and timeouts that don't embed the pauses or timeouts inside the scenario. What do you think? I'm interested in learning from your comments!

# Acknowledgements
I'm indebted to colleagues Jeff Siver, Nermin Dibek, Guillermo Diaz, Nicolas Perez, and Adrian Petras whose feedback and collaboration have meant so much.
---
_This article was first posted to LinkedIn June 25, 2020._

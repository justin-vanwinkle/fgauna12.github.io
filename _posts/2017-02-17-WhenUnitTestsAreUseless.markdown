---
layout: post
title:  "When Unit Tests are Useless"
date:   2017-02-19 10:00:00 -0500
categories: ide
comments: true
---

## My worst fear: Wasting time
When I'm working hard on a testing suite, my worst fear is it will be wasted work. _poof_ 30-50 tests on a feature... **gone**. Why? Because the feature is not wanted or needed. 
So why keep the feature? Why keep the tests? It's just more code to maintain.
While you might not agree that the tests are gone, we can agree that the tests will not prevent defects for features meaninful to the users.

We should not be writing tests in fear that they'll be wasted work. The answer lies in the infamous testing pyramid.

## The Testing Pyramid

![Testing Pyramid]({{site.url}}//assets/TestingPyramid.png)

The premise of the testing pyramid is to show which tests pay the most dividents when finding regressions. Being at the bottom of the pyramid, unit tests are foundational. Every developer knows that unit tests are their responsibility. 
It's not something that can be tasked to QA.

Integration tests are not as cookie-cut. These test are often misleading, brittle, and complicated. Personally, when an integration test breaks, it doesn't really mean that functionality broke. 

We kid ourselves when someone pitches us the benefit of coded UI tests. No they're not useless. They just **break** so easily. It cultivates ideas that change is bad, because it will break the tests and delay delivery. 
These tests are better spent on business critical user workflows.

Overall, the pyramid quantifies how much time we should dedicate to each type of test in our test suite. 
If you have more integration tests than unit tests, then you won't get immediate feedback on regressions or you'll be distracted by red herring issues. 
If you have more UI tests than integration tests, then regressions will be found later in the process.

## Is your pyramid stable?

Even if you had only unit tests as part of your test suite, you are capable of finding a sizable portion of regressions. 

### 1. Volatile Requirements

![Testing Pyramid]({{site.url}}//assets/TestingPyramidWithCustomerValue.png)

In larger organizations, this doesn't happen as often. Typically because there is someone in charge of distilling requirements. 
Typically in the form of Business Analysists or Scrum Masters. It does continue to happen and it's mitigated in various ways.

### 2. Testing Units that Violate YAGNI

### 3. The Tephone Game

## Some Solutions

### 1. Be Brave! Talk to the Customer!

### 2. Hallway Usability Tests

### 3. Make a POC

### 4. Release Early and often

### 5. Think Holistically 






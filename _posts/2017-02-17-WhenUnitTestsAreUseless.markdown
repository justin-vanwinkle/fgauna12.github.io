---
layout: post
title:  "When Unit Tests are Useless"
date:   2017-02-19 10:00:00 -0500
categories: ide
comments: true
---


I love writing unit tests... when I have the time. But, this post is not to share that it's ok not to write tests because you don't have time. Sometimes, there's a legitimate reason when you should avoid writing unit tests. 
I'll share when it's not a good idea to write tests.

## Proof of Concepts (POC) - Because it's already a test

Ever started a POC project without any tests? I have. But some say you should write tests for POC because they often make it to production.
_Sigh_... yes, there's no winning. You write a POC without tests because you're thinking it's just to prove an idea. On the other hand, there's a good chance your POC will make it to production if your client loves it.

**But**, think about it this way: the POC is a test in itself. If done properly, you're trying to build the Minimum Viable Product (MVP) so that you can test whether or not the solution will provide value to the users.

If you get great feedback, then it validates that this idea is worth persuing. If you don't get good feedback, it's time to iterate quickly. 
For a typical web application, writing unit tests on top of this will essentially slow down your test execution. 

## The Testing Pyramid


 ![Testing Pyramid]({{site.url}}//assets/TestingPyramid.png)

 The premise of the testing pyramid is to show which tests are best at finding regressions. Being at the bottom of the pyramid, unit tests are foundational. Every developer knows that unit tests are their responsibility. 
 It's not something that can be tasked to QA.

 But, when we talk about integration tests, it's not as straight forward. 
 These test are often misleading, brittle, and complicated. Personally, when an integration test breaks, it doesn't really mean that functionality broke. 
 I feel discouraged when I see more of these tests than unit tests themselves. The pyramid looks more like a diamond. No foundation.

 We kid ourselves when someone pitches us the benefit of coded UI tests. No they're not useless. They just **break** so easily. It cultivates ideas that change is bad, because it will break the tests and delay delivery. 

The pyramid quantifies how much effort we should be spending on creating our test suite. If you have more integration tests than unit tests, then your test suite is not stable and it will crumble. If you have more UI tests than integration tests, again, your test suite will crumble.
Thankfully, if we just worry on producing unit tests, the test suite will be solid and we're going to be extremely agile.

#### WRONG! - Unit tests should not be the first test 

Every write a test and then you are told that the customer no longer wants that feature you're testing? It **hurts** doesn't it?. You worked so hard on those beautiful tests that you can't bring yourself to deleting them. 
You just comment them out.

 ![Testing Pyramid]({{site.url}}//assets/TestingPyramidWithCustomerValue.png)

These scenarios can break your foundation:
- Volatile Requirements
- Unvalidated Customer Value

#### Volatile Requirements

In larger organizations, this doesn't happen as often. Typically because there is someone in charge of distilling requirements. 
Typically in the form of Business Analysists or Scrum Masters. It does continue to happen and it's mitigated in various ways.








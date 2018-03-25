---
title: observables vs promises
date: 2018-01-31 20:40:06
---

## Observables vs Promises
* A coworker of mine today asked me this splendid question and I thought it is worth to write down.

* A **Promise** handles only a single asynchronous event. When the asynchronous event is completed (success or fail), a callback is invoked.

* An **Observable** is based off of the *Observer Pattern*, which is when observers subscribe to a subject and are notified when the subject changes state so that they can react accordingly.
- More specifically, there can be zero to many observables that subscribe to an event, and when that event completes, (successfully or not) a callback is invoked for every event. 
- Basically it is a **function** that takes an **observer**, which defines the **next**, **complete**, or **error** callbacks. The *observable* invokes the callbacks based on whether the event is a succession, a completion, or an error.


## Conclusion:

An **observable** is much more powerful than a promise becuase it can handle more streams of events and data over a period of time, whereas a promise can only handle one asynchronous event.
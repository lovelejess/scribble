---
title: TEST DOUBLE TROUBLES
date: 2018-03-05 21:24:43
---

[Notes from Uncle Bob](https://8thlight.com/blog/uncle-bob/2014/05/14/TheLittleMocker.html)

## Dummy
- Returns null 

## Stub
- Returns stubbed values

## Spy
- Spies on the module being tested
- Records what the module was doing
- The more you spy, the tighter the implementation coupling

## Mock
- Mocks test behavior
- Not interested in return values of functions
- Interested in what functions were called, with what arguments, when, and how often
- Mock spies on behavior of the module being tested
- Mocks are spies
- Moves the expectation into the mock results in coupling
- Mock libraries are easy and fast to use that's why it's used a lot

## Fake
- Has real business behavior
- Fake out the data
- Fakes are extremely complicated 
- Sometimes need tests of their own
- Uncle bob doesn't use fakes


"Mock is a kind of spy, a spy is a kind of stub, and a stub is a kind of dummy. But a fake isn't a kind of any of them. It's a completely different kind of test double."

Uncle Bob almost always uses stubs and spies since they are very easy to write!
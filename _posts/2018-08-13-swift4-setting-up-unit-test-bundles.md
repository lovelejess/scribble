---
title: "swift4: setting up unit tests"
date: 2018-08-13 16:43:58
---

> Currently enrolled in <a href="https://www.raywenderlich.com/3530-testing-in-ios/" target="_blank">Ray Wenderlich's- Testing in iOS Course</a> Notes from Lesson 1 and 2


## Getting Started/Setting Up

### How to Set Up an iOS Unit Test Bundle:
  * Add new iOS Unit Testing Bundle Target
    * This will create a new folder with your unit test files

  ![sample image](../images/add-ios-unit-test-bundle.png)
  
  * Import your production code module at the top of your test file
    * `@testable import MyModule`

    ![sample image](../images/add-import-module.png)
  * Write your tests! 
    * Preceed all unit tests with `testMyTestName()`
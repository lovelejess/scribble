---
title: "swift4: deeper dive in swift"
date: 2018-08-21 10:36:42
---

> Currently enrolled in <a href="https://www.udemy.com/complete-ios-11-developer-course" target="_blank">Udemy's- The Complete iOS 11 & Swift Developer Course</a> Notes from Lesson 4

## What is Swift?
* used to develop iOS, macOS, watchOS, and tvOS
* runs on objective c runtime


## Using var vs let
* `var` is to indicate that the variable's value may change over it lifetime
* `let` is to indicate that the variable's value may NOT change over its lifetime
* best practice is to use `let` whenever possible, because it prevents unintended value changes


## Type Conversions
* can convert types into other types by `Type(yourVariable)`
* e.g.
  ```
  let myFloat1 = 9.02 
  print(String(myFloat1))
  ```

## Arrays
* cool things to note:
  * although not recommended, can have **mix typed arrays**, where its type is `Any`
    * example: `let mixArray: Any = ["hey", 0, false]`

  * initializing an empty array of a type:
  `let stringArray = [String]()`




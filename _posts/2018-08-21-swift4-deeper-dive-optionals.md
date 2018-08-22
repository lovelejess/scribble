---
title: "swift4: deeper dive- optionals"
date: 2018-08-21 14:56:30
---

> Currently enrolled in <a href="https://www.udemy.com/complete-ios-11-developer-course" target="_blank">Udemy's- The Complete iOS 11 & Swift Developer Course</a> Notes from Lesson 4

## What are Optionals?
* **optionals** are used when a value may be absent (nil)
* indicated by the `?` on the type of the variable
  * e.g. `var myOptional: Int?`
* to access the raw value from an optional, you unwrap it by `!`
  * e.g. `myOptional!`

### What's a Great Example of Where Optionals Occur?
  * a great example is using ui text fields because it is not always guaranteed that the user will enter a value. If you assume that the user will always input a value, the swift compiler will error.

  * this will error out because we're assuming that the `inputNumber` has a `text` value and then casting it to an Int
  * the error you will see is: 
  *Value of optional type 'String?' not unwrapped; did you mean to use '!' or '?'?*
  
    ``` 
    @IBOutlet weak var myUITextField: UITextField!

    @IBAction func myAction(_ sender: Any) {
        if let inputNumber = Int(myUITextField.text) {
            print("sum is: \(inputNumber + 100)")
        }
    ```


  * to fix this, use `if let` to check if the value exists before casting it to an Int
  
    ``` 
    @IBOutlet weak var myUITextField: UITextField!

    if let inputText = myUITextField.text {
      if let inputNumber = Int(inputText) {
        print("sum is: \(inputNumber + 100)")
      } else {
        print("Oops no input text to sum")
      }
    }
    ```

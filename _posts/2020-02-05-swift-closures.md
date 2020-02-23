---
title: "swift - closures"
date: 2020-02-05 18:35:13
---

## Closures

Closures are unnamed, self-contained blocks of code that can be passed around as arguments to function. They usually are in the following format:

    { (parameters) -> <return type>  **in**
        // code to execute
    }


### Named Closures

Even though closures are self-contained and can be unnamed, they still can be assigned to a variable and invoke with its name.

    let myClosure = {(name: String) in return "My name is \(name)"}
    myClosure("Jess")} // prints "My name is Jess"


Named closures are the same thing as a function. Can interchangably use them, so use whatever fits the context of your app.


The function below is the same as the closure above.

    func myClosure(name: String) -> String {
        return "My name is \(name)"
    }


## Array of Closures

Closures can be executed in an array, but they must be of the same return type.

    let myClosure = {(name: String) in return "My name is \(name)"}
    let anotherClosure = { (anotherName: String) in return "\(anotherName) another name"}
    let closureArray = [anotherClosure, myClosure]
    closureArray // returns [(String) -> String, (String) -> String]


## Typealias

Types that can be referenced as new names. 

    typealias NewInt = Int


## Variable Capture

A closure can capture constants and variables from the surrounding context in which it is defined. The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists. - [Capturing Values Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Closures.html#ID103)


    func increment(forIncrement amount: Int) -> () -> Int {
        var total = 0
        func incrementer() -> Int {
            total += amount
            return total
        }
        return incrementer
    }

    let incrementMe = increment(forIncrement:1)
    incrementMe() // returns 1
    incrementMe() // returns 2

    

The value `total` is referenced in both invocations because the closures has access and references `total`

let another = increment(forIncrement:1)
another() // returns 1
incrementMe() // returns 3


## Escaping vs Non- Escaping Closures

An **escaping closure** outlives the function it was passed to. Therefore the closure is executed *after* the function it was passed to returns.
    * The risk of this are **retain cycles** since the closure can outlive the function and maintain a reference to variables within the closure.
    * Any reference to self within a closure should be a reference to `unowned self` or `weak self` to prevent a reference cycle.

A **non-escaping closure** is a closure that’s called within the function it was passed into. Therefore the close is executed before the function returns.
    * A non-escaping closure can’t create a retain cycle, because all variables it references from the context it was created in will be removed after the closure completes execution.

 **Example of Escaping Closure**

    class func requestImageFile(url: URL, completionHandler: @escaping (UIImage?, Error?) -> Void) {

        // do stuff and return
        // then call the completion handler/ closure
         completionHandler(image, nil)
    }

**Example of Non-Escaping Closure**

    class func requestImageFile(url: URL, completionHandler: (UIImage?, Error?) -> Void) {

        // do stuff
        // then execute the completion handler/ closure
         completionHandler(image, nil)
    }
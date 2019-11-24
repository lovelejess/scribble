---
title: "intro to swift 5"
date: 2019-11-23 14:15:12
---

# Intro to Swift 5

## What is Swift? 

"Swift is a general-purpose, multi-paradigm, compiled programming language developed by Apple Inc. for iOS, iPadOS, macOS, watchOS, tvOS, Linux, and z/OS. Swift is designed to work with Apple's Cocoa and Cocoa Touch frameworks and the large body of existing Objective-C code written for Apple products" -  

### Type Safe

- Swift performs type checks when compiling code and flags any mismatched types.
- This encourages the developer to be intentional and clear with the types they declare for values. Once declared as a type, it cannot be changed.
- In other words, while we can change the value of a variable, we cannot change its type. 

```
var thisIsAString: String = "Cannot change to different type"
thisIsAString = 0
    error: cannot assign value of type 'Int' to type 'String'
```

### Type Inference

- Infers the type based on the value provided
- If it quacks like a duck, walks like a duck, then it must be a duck
- If there's double quotes around a variable's value, it assumes it's a `String`.
- If there are no quotes around a variable's value, it assumes it's a `Double`.


### Compiled

- The Swift compiler uses LLVM for optimization and binary generation.


### Statically Typed

- All types are known at compile time

### Open Sourced
- 


### Ranges

A **range** is a built-in Swift type which expresses a sequence of values between a start and an end value.
  * For example, 1...5 is a range, its values are: 1, 2, 3, 4, 5.

A **Half Open Ranges** are a sequence of values between a start and not inclusive of the end value, denoted by `start..<end`. 
  * For example, 1..5 is a half open range. It's values are:  1, 2, 3, 4.

#### Example:

To print all the numbers for the range: -3, -2, -1:

```
for i in -3..<0 {
    print(i)
}
```

However, the below does not work. XCode will error `Could be postfix '...' and binary '-'`

```
for i in -3...-1 {
    print(i)
}
```

Therefore, wrap the negative value in a parentheses `()`

```
for i in -3...(-1) {
    print(i)
}
```


### Repeat While

**Repeat while** guarantees that the body will execute at least oncel therefore the conditional is checked at the end of the loop. This is different from a **while loop** because in a while loop, it checks the conditional at the beginning of the loop, thus not guaranteeing that the body of the loop will be executed.


### Structs

- A **struct** is a useful way to group properties together to represent a common object
- A **struct** is a value type, which means the values are copied. They are independent instances with its own unique copy of its data.
- The properties in a **struct** cannot be modified within its instance methods by default
- If a method mutates a property on a **struct** then use the `mutating` keyword before the function declaration

```
struct Employee {
    let name: String
    var age: Int
    var salary: Int
    var bonus: Int
}

mutating func setBonusMultiplier() {
    bonus *= 0.50
}
```


### Computed Properties

- Useful when a property value depends on another property's value.
- **Computed Properties** act like methods such that it can compute the value and return a value for that property

```
struct Employee {
    let name: String
    var age: Int
    var salary: Int {
        salary = salary + bonus(0.10)
    }
    var bonus: Int
}

mutating func setBonusMultiplier() {
    bonus *= 0.50
}
```

### Enums

- An **enum** is a useful way to declare types for a group of related values.
- An **enum** is a value type, which means the values are copied. They are independent instances with its own unique copy of its data.
- Defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.

```
enum Direction: String {
    case north = "north"
    case east = "east" 
    case west = "west"
    case south = "south
}
```
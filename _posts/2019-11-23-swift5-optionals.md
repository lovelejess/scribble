---
title: "swift 5 - optionals"
date: 2019-11-23 18:16:59
---

# Swift 5 - Optionals

## What is an optional?

- An **optional** is used to handle the abscence of a value.
- An **optional** can indicate two things: 
  * There is a value, but you have to unwrap the optional to access the value
  * There isn't a value at all

- In Swift, assigning a value to `nil` is not allowed, unless it's an optional. This protects the app from crashing due to unintended side effects, such as accessing a property from a value that is `nil`. 
- Using `optionals` forces the developer to be more intentional on `nil` handling

Can set an optional type to `nil`:

```
let possibleString: String? = "An optional string."
possibleString = nil
// possibleString now contains no value
```
### Types of Optional Handling:

- forced unwrapping via `!` operator — unsafe
- implicitly unwrapped variable declaration — unsafe in many cases
- optional binding — safe
- optional chaining — safe
- nil coalescing operator — safe
- guard statement — safe
- optional pattern — safe

### Force Unwrapping

Only force unwrap an optional when you are confident that there will always be a value set, otherwise it crashes the app/ causes runtime error when the value is nil. It's like unwrapping a present and not knowing if there's a bomb in there or not!

```
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // requires an exclamation mark
```

### Implicitly Unwrapped Optional

An optional that can also be used like a non-optional value, without the need to unwrap the optional value each time it is accessed, because it's assumed to always have a value after that value is initially set. Try to avoid this and use optional values for safety.

```
let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // no need for an exclamation mark
```

### Optional Binding

Checks and unwraps an implicitly unwrapped optional

```
let assumedString: String! = "An implicitly unwrapped optional string."

if let definiteString = assumedString {
print(definiteString)
}
// Prints "An implicitly unwrapped optional string."
```

### Optional Chaining

Accessing properties of an optional value. Short circuits if the optional value is nil. Fails gracefully when the value is nil.

```
let john = Person()

if let roomCount = john.residence?.numberOfRooms {
print("John's residence has \(roomCount) room(s).")
} else {
print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."
```

### Nil Coalescing Operator

The nil-coalescing operator (a ?? b) unwraps an optional a if it contains a value, or returns a default value b if a is nil. The expression a is always of an optional type. The expression b must match the type that is stored inside a.

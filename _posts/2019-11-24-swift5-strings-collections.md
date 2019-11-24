---
title: "swift 5 - strings & collections"
date: 2019-11-24 08:03:58
---

# Swift 5 - Strings

## What is a String?

- Hey, did you know a **string** in swift is a **struct** under the hood?
- This means that **strings** are passed by value and have several properties and methods that we can use to manipulate our strings
- To instantiate a **string**, use parentheses, like `let myCoolString = String()`


### Mutating

- Many of the functions for string manipulation are mutating - they modify the string variable's contents directly, instead of returning a new string.
- You'll see that each of the below functions have a `mutating` keyword before it's function declaration.
- Examples:
    - `append()`
        - `mutating func append(_ other: String)`
    - `removeFirst()`
        - `@discardableResult mutating func removeFirst() -> Character`
    - `removeLast()`
        - `@discardableResult mutating func removeLast() -> Character`

# Swift 5 - Collections

## What is an Array?

- An **array** is list of ordered items

```
var numbers = Array<Double>()

//: This is syntactic sugar for the above
var moreNumbers = [Double]()

//: Array literal syntax. Uses type inference to determine the type is an array of doubles
let differentNumbers = [97.5, 98.5, 99.0]
```

## What is a Dictionary?

- An **dictionary** is a list of unique key,value pairs

```
var myDictionary = [String:String]()

//: This is syntactic sugar for the above
var addressBook = ["jess": "123 Drury Lane", "bob": "345 Candy Lane", "alice": "85 Yellow Brick Road"]
```

### Removing Items from Dictionary

```
var presidentialPetsDict = ["Barack Obama":"Bo", "Bill Clinton":"Socks",
    "George Bush":"Miss Beazley", "Ronald Reagan":"Lucky"]

presidentialPetsDict["George W. Bush"] = presidentialPetsDict["George Bush"] 
presidentialPetsDict["George Bush"] = nil 

//: Same as above
var oldValue = presidentialPetsDict.removeValueForKey("George Bush")
presidentialPetsDict["George W. Bush"] = oldValue
```

## What is an Set?

- An **set** is a list of unique, unordered items

```
var students = Set<String>()

//: This is syntactic sugar for the above
var students:Set = ["student1", "student2", "student4"]
```

### No Duplicates Allowed!

- You can try to add duplicates to the set, but Swift will not update the Set with the duplicate because sets have no duplicates

```
var students:Set = ["student1", "student2", "student4"]
// This does nothing
student.insert("student1")
```
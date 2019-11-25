---
title: "swift 5 - object oriented"
date: 2019-11-24 11:00:31
---

# Swift 5 - Object Oriented


### copy-on-write optimization

- **copy-on-write optimization** happens when one of its copies are modified. The copy that is being modified replaces its storage with a unique copy of itself, which is modified in place. The one that is modified incurs the cost of copying.

```
class Numbers {
    var value = 0
}

var originalArray = [Numbers(), Numbers()]
var copiedArray = originalArray

originalArray[0].value = 1
//: Updated value is reflected in both arrays because the line above only modifies the `value` of the object, `Numbers`
// originalArray = [1,0]
// copiedArray = [1,0]

originalArray[0] = Numbers()
//: Updated value is NOT reflected in both arrays because the line above modifies the entire object, `Numbers`. Now it references a different object for index 0
// originalArray = [0,0] 
// copiedArray = [1,0]

```

### inheritance

- In Swift, a class can only inherit from one parent class


```
class Shape {
    var height: Double = 0
    var width: Double = 0

    init(height: Double, width: Double) {
        self.height = height
        self.width = width
    }

    public func calculateArea() -> Double {
        return height * width
    }
}

//: Triangle inherits from Shape
class Triangle: Shape {
    var base: Double = 0

    init(height: Double, width: Double, base: Double) {
        //: Calls the parent class, Shape's init
        super.init(height: height, width: width)
        self.base = base
    }

    // overrides the parent class' function since it has a different implementation
    override public func calculateArea() -> Double {
        return (height * base) / 2
    }
}

var triangle = Triangle(height: 1.0, width: 2.0, base: 4)
print(triangle.calculateArea())

```

### polymorphism

- an object that can take on many forms

```

//: Triangle is of type Shape (the parent class) but it's also of type Triangle
var triangle: Shape
triangle = Triangle(height: 1.0, width: 2.0, base: 4)

print(triangle.calculateArea())

```

### protocols

- a **protocol** is similar to a contract. Any class that adheres to the protocol must implement every method on the protocol's list
- a **protocol** is a type and can be treated as such.

```
protocol Business {
    func sell() -> String
    func stockInventory() -> Bool
}

```

### extensions

- an **extension** enables you to add functionality to a class without ever modifying the original class definition.

```
extension GroceryStore: Business {
    func orderOnline() -> String {
        //...
    }
}
```

### pass by value vs pass by reference

#### pass by value

- an instance of a struct or a parameter
- does not share or reference the same memory address for that instance
- changing a property on the instance will only apply to the copy, and the original instance will remain unchanged.

#### pass by reference

- an instance of a class
- shares or references the same memory address for that instance
- changing a value on a copy will also modify the original instance, since the copy is referencing the original.

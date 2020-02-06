---
title: "swift - generics"
date: 2020-02-04 20:43:13
---

## Generics

Generic are very powerful and useful in Swift. Generics enables code to be flexible and reusable by allowing the function or parameter to take on different
types at different times. As a result, it eliminates code duplication.

Suppose we needed to print the types of Integers, Strings, and Booleans. We would have to create a function for each of the different parameter types.


    func printInt(_ argument: Int) {
        print(type(of: argument))
    }

    func printString(_ argument: String) {
        print(type(of: argument))
    }

    func printBoolean(_ argument: Bool) {
        print(type(of: argument))
    }

    but instead we could use generics to eliminate all the code duplication:

    func printType<T>(_ argument: T) {
        print(type(of: argument))
    }


## Contraining Generics to Concrete Types

Generics can be broad enough to allow for any type, but it can also be bound or constrained to a specifc concrete type or must adhere or implement some protocol
.
In the example below, `IndoorGym` is defined where its climber type property is generic and constrained to any type that implements the `Climber` protocol.

    protocol Climber {
        var name: String { get }
        static var type: String { get }
    }

    struct Boulder: Climber {
        let name: String
        static let type = "Boulder"
    }

    struct Sport: Climber {
        let name: String
        static let type = "Sport"
    }

    struct IndoorGym<ClimberType: Climber> {
        let climbers: [ClimberType]

        func letsClimb() {
            print("Today we have a lot of \(ClimberType.type) climbers!!")
            for climber in climbers {
                print("Welcome to the gym, \(climber.name).")
            }
        }
    }

    let climbers = IndoorGym<Boulder>(climbers: [Boulder(name: "Jess"), Boulder(name: "Adam")])
    climbers.letsClimb()

    // prints:
    // Today we have a lot of Boulder climbers!!
    // Welcome to the gym, Jess.
    // Welcome to the gym, Adam.

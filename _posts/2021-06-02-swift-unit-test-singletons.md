---
title: "swift - unit test singletons"
date: 2021-06-02 21:25:30
---

## How to Unit Test Singletons

* Sure singletons are an anti-pattern and not usually recommended, but sometimes we have a dependency on a singleton (e.g. Firebase). Therefore we need a way to unit test it, so this blog shows you how to do so!

* Link to [repo](https://github.com/lovelejess/SwiftSingletonUnitTestExample)


## Steps:

1. Create a protocol with the methods that are dependencies, which needed to be faked out
1. Create a wrapper around the singleton class that implements this protocol. It is a passthrough class
1. Dependency inject the singleton in your consumer with the type as the protocol and a default parameter of the wrapper
1. Create a fake that implements the protocol so you can dependency inject this fake in order to test the consumer of the singleton


### Singleton Example 

```
import Foundation
/// This is an example of a singleton that's a black box.
/// Pretend it comes from a third party library and we have no insight into what it does
class Singleton {
    class func run(with parameter: String) -> String {
        let run = "Singleton: Does something epic with: \(parameter)"
        print(run)
        return run
    }
}
````


### Singleton Wrapper
```
/// Create a protocol for all the public apis that we use for that singleton
protocol SingletonProtocol {
    func run(with parameter: String) -> String
}

/// A Wrapper around the Singleton
/// Need to create a wrapper around this singleton so that we can implement a protcol, which will be used to create our fake
/// This wrapper is a pass through and implements the protocol
class SingletonWrapper: SingletonProtocol {
    func run(with parameter: String) -> String {
        return Singleton.run(with: parameter)
    }
}

/// A sample class that has a dependency on a singleton
/// We will dependency inject the singleton and its type will be a protocol
/// This is the class that we will unit test
class SingletonConsumer {

    private var singleton: SingletonProtocol!

    var animal: String?
    init(singleton: SingletonProtocol=SingletonWrapper()) {
        self.singleton = singleton
    }

    func doSomethingCool(with parameter: String) {
        let value = singleton.run(with: parameter)
        if value.contains(Animal.cats.rawValue) {
            animal = Animal.cats.rawValue
        } else if value.contains(Animal.dogs.rawValue) {
            animal = Animal.dogs.rawValue
        } else if value.contains(Animal.elephants.rawValue) {
            animal = Animal.elephants.rawValue
        }
    }
}

enum Animal: String {
    case cats
    case dogs
    case elephants
}
```

### Testing Code

```
class FakeSingleton: SingletonProtocol {
    func doesSomethingEpic(with parameter: String) -> String {
        let epicness = parameter
        print(epicness)
        return epicness
    }
        
}

/// Tests`SingletonConsumer`
class SingletonConsumerTests: XCTestCase {
    func test_doSomethingCoolWithNonMatchingEpicnessValues_setsEpicValueTo_NotEpicYet() {
        let singletonConsumer = SingletonConsumer(singleton: FakeSingleton())
        singletonConsumer.doSomethingCool(with: "spiders")
        let actual = singletonConsumer.epicValue
        let expected = "not epic yet"
        XCTAssertEqual(actual, expected)
    }
    
    func test_doSomethingCoolWithCats_setsEpicValueToCats() {
        let singletonConsumer = SingletonConsumer(singleton: FakeSingleton())
        singletonConsumer.doSomethingCool(with: "cats")
        let actual = singletonConsumer.epicValue
        let expected = Animal.cats.rawValue
        XCTAssertEqual(actual, expected)
    }
}

```
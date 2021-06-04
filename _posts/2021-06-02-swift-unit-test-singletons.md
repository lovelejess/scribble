---
title: "swift - unit test singletons"
date: 2021-06-02 21:25:30
---

### How to Unit Test Singletons

* Sure singletons are an anti-pattern and not usually recommended, but sometimes we have a dependency on a singleton (e.g. Firebase). Therefore we need a way to unit test it, so this blog shows you how to do so!

* Link to [repo](https://github.com/lovelejess/SwiftSingletonUnitTestExample)

```
/// This is an example of a singleton that's a black box.
/// Pretend it comes from a third party library and we have no insight into what it does
class Singleton {
    class func doesSomethingEpic(with parameter: String) -> String {
        let epicness = "Singleton: Does something epic with: \(parameter)"
        print(epicness)
        return epicness
    }
}

/// Create a protocol for all the public apis that we use for that singleton
protocol SingletonProtocol {
    func doesSomethingEpic(with parameter: String) -> String
}

/// A Wrapper around the Singleton
/// Need to create a wrapper around this singleton so that we can implement a protcol, which will be used to create our fake
/// This wrapper is a pass through and implements the protocol
class SingletonWrapper: SingletonProtocol {
    func doesSomethingEpic(with parameter: String) -> String {
        return Singleton.doesSomethingEpic(with: parameter)
    }
}

/// A sample class that has a dependency on a singleton
/// We will dependency inject the singleton and its type will be a protocol
/// This is the class that we will unit test
class SingletonConsumer {

    private var singleton: SingletonProtocol!

    var epicValue: String = "not epic yet"
    init(singleton: SingletonProtocol=SingletonWrapper()) {
        self.singleton = singleton
    }

    func doSomethingCool(with parameter: String) {
        let value = singleton.doesSomethingEpic(with: parameter)
        if value.contains(Animal.cats.rawValue) {
            epicValue = Animal.cats.rawValue
        } else if value.contains(Animal.dogs.rawValue) {
            epicValue = Animal.dogs.rawValue
        } else if value.contains(Animal.elephants.rawValue) {
            epicValue = Animal.elephants.rawValue
        }
    }
}

enum Animal: String {
    case cats
    case dogs
    case elephants
}

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
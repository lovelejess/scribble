---
title: "iOS - delegate pattern"
date: 2019-12-26 11:58:00
---

## iOS Delegate Pattern

- Use the **delegate pattern** when an object needs to communicate with its owner in a decoupled manner. 

* **delegate** an object that executes a group of methods on behalf of another object
* **protocol** is a list of methods that a **delegate** must implement
    * any object that fulfills the **protocol** can become the **delegate**

* **View**
    * The object that will have the delegate
* **ViewController**
    * Can implement the **delegate** or another class can
* 

### Examples of Protocols
* Climbing Communication Protocols When Belaying
  - [American Alpine Club](https://americanalpineclub.org/resources-blog/2017/1/19/4xm1fcsag6b7xqf1p1w1qp7vdpp1ha)


#### ClimberBelayer Protocol

    
    public protocol ClimberBelayerProtocol: class {
        /**
        Implement this protocol method when climber is ready to start climbing
        */
        func onBelay()
        /**
        Implement this protocol method when climber is starting to climb
        */
        func climbing()
        
        /**
        Implement this protocol method when climber is about to fall
        */
        func falling()
    }
    

#### ClimberBelayerDelegate

    class ClimbingSession: ClimberBelayerProtocol {
        var climber: Climber?
        var belayer: Belayer?

        init() {
            climber = Climber(name: "Jess", weight: 115)
            belayer = Belayer(name: "Hannah", weight: 105)
            climber.climberBelayerDelegate = self
        }
        
        func onBelay() {
            print("Belay is on")
        }
        
        func climbing() {
            print("Climb on")
        }
        
        func falling() {
            print("I've got you")
        }
    }

    public class Climber {
        public var name: String?
        public var weight: Float?
        public weak var climberBelayerDelegate: ClimberBelayerProtocol?
        
        init(name: String, weight: Float) {
            self.name = name
            self.weight = weight
        }
        
    }

    public class Belayer {
        public var name: String?
        public var weight: Float?
        
        init(name: String, weight: Float) {
            self.name = name
            self.weight = weight
        }
    }
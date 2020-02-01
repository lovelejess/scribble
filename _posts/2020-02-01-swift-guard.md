---
title: "swift - guard"
date: 2020-02-01 13:00:13
---

# What is a Guard Statement?

**Guard Statements** are used to check if preconditions are satisfied before continuing. Guard statements...

* check for conditions that must be held for execution to continue
* can simultaneously check for conditions while safely unwrapping optionals
* must exit scope if conditions are not held
* use commas to separate multiple conditions

`guard <condition-list>  else <code-block>`


    func canClimberClimb(belayerReady: Bool, climberReady: Bool, safetyChecks: Bool) {
        guard belayerReady else { return }
        guard climberReady else { return }
        guard safetyChecks else { return }
        print("Ready to Climb!")
    }

    canClimberClimb(belayerReady: true, climberReady: true, safetyChecks: true) // prints "Ready to Climb!"
    canClimberClimb(belayerReady: true, climberReady: false, safetyChecks: true) // exits early. nothing is printed


## Guard vs If

**Guard** is used to check preconditions and allow for early exits, whereas **if** statements are used to change condition paths based on conditions

## Guard with Optionals

**Guard** statements are great for working with optionals.


    func canClimberClimb(belayerReady: Bool, climberReady: Bool, safetyChecks: Bool, spotterName: String?) {
        guard let spotterName = spotterName else { return } // guard with optional
        guard belayerReady, climberReady, safetyChecks  else { return }
        print("Ready to Climb!")
    }

    canClimberClimb(belayerReady: true, climberReady: true, safetyChecks: true, spotterName: "Joe" ) // prints "Ready to Climb!"
    canClimberClimb(belayerReady: true, climberReady: false, safetyChecks: true, spotterName: nil) // exits early. nothing is printed

## Guard Let/Var vs Guard If

When optional values are used with `guard let` (or `guard var`) they are bound as non-optional values and available in the rest of the scope where the guard statement appears, wheras with `if let`, optionals are bound as non-optional constants. They are only available in the body of the if let statement.

### Guard Let/Var

    func canClimberClimb(belayerReady: Bool, climberReady: Bool, safetyChecks: Bool, spotterName: String?) {
        guard belayerReady, climberReady, safetyChecks  else { return }
        guard var spotterName = spotterName else { return } // guard with optional
        
        spotterName = spotterName.uppercased() // spotterName is available outside the guard block
        
        print("\(spotterName) says you're ready to climb!")
    }

    canClimberClimb(belayerReady: true, climberReady: true, safetyChecks: true, spotterName: "Joe" ) // prints "JOE says you're ready to climb!"

### Guard If

    func canClimberClimb(belayerReady: Bool, climberReady: Bool, safetyChecks: Bool, spotterName: String?) {
        guard belayerReady, climberReady, safetyChecks  else { return }

        if let spotterNameUpperCased = spotterName?.uppercased() {
            print("\(spotterNameUpperCased) says you're ready to climb!") // spotterNameUpperCased is available only inside the if let else block
        }
    }

    canClimberClimb(belayerReady: true, climberReady: true, safetyChecks: true, spotterName: "Joe" ) // prints "JOE says you're ready to climb!"
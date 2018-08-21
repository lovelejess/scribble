---
title: "swift4: setting up ui tests"
date: 2018-08-17 13:49:43
---

> Currently enrolled in <a href="https://www.raywenderlich.com/3530-testing-in-ios/" target="_blank">Ray Wenderlich's- Testing in iOS Course</a> Notes from Lesson 21-29

 * iOS UI Test Recordings are super neat! It records your every move in the simulator and translates it into code for you. All you have to do is write the assertion! Here's how to get started:

## Getting Started/Setting Up

### How to Set Up an iOS UI Test Bundle:
  * Add new iOS UI Testing Bundle Target
    * This will create a new folder with your UI test files

  ![sample image](../images/add-ui-test-bundle.png)

  * Add a test method preceeded with the word `test`
  * Move your cursor within that function
  * Start recording your UI unit tests by: 
    * Have your selected simulator running
    * Click on the red circle record button to begin recording, like so:

![sample image](../images/sample-ui-test-recording.png)

  * When you're done recording, hit the button again and watch it populate your screen with code
  * Write your assertion based on what you recorded!

### Running Your UI Tests:

* Run the UI tests the same way as you would for a unit test
* If a test fails, the default behavior is to take a screenshot of the simulator at each step of the test. It's a great way for debugging
* To find the screenshots and ui test logs:
  * Navigate to the "Report Navigator" (Far right Icon in your left navigation panel)
  * Expand the most recent test run
    * You will see all the steps and the attached screenshots

![sample image](../images/ios-ui-test-failure-log.png)
![sample image](../images/ios-ui-test-failure-screenshot.png)

  * If you look closely, you can see that I wrote my expectation wrong. The result should be 10, and not 0 since `9 + 1 = 10`. Oops, let's change that to make our test pass!


### Example Refactored UI Tests
* It's best to refactor your UI Tests so that the shared components of your tests are only ran once. This will make the tests more performant and run faster!
* Make sure that you set up your tests to the assumed starting position. For instance, I was testing the InfoViewController, and I just assumed that my test knew where to start, but it loaded at the calcuator view from the previous run. Therefore I needed to set up my test to start at InformationView. Sample code below:

    ```
    class InfoViewUITests: XCTestCase {
        private var app: XCUIApplication!

        override func setUp() {
            super.setUp()
            continueAfterFailure = false
            // UI tests must launch the application that they test. 
            // Doing this in setup will make sure it happens for each test method.
            app = XCUIApplication()
            app.launch()
            
            app.navigationBars["Master"].buttons["Menu"].tap()
            app.tables.staticTexts["Information"].tap()
        }
        
        override func tearDown() {
            super.tearDown()
        }
        
        func testExample() {
            let informationNavigationBar = app.navigationBars["Information"]
            
            XCTAssertTrue(informationNavigationBar.otherElements["Information"].exists)
            
        }
    }

    ```
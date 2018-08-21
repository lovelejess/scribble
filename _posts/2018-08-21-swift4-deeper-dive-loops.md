---
title: "swift4: deeper dive- loops"
date: 2018-08-21 14:32:44
---

> Currently enrolled in <a href="https://www.udemy.com/complete-ios-11-developer-course" target="_blank">Udemy's- The Complete iOS 11 & Swift Developer Course</a> Notes from Lesson 4

## While Loops
* use while loops when you want the same lines of code executed when the boolean condition is satisifed
* syntax:
    ```
        while (true) {
            //do something here
        }
    ```

## For Loops
* used to loop over collections
* traditional syntax:  
    ```
        for item in collection {
            //do something here
        }
    ```
* enumerating syntax:
    ```
    for (index, value) in numbers.enumerated() {
        numbers[index]+= value+=1
    }
    ```
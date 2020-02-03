---
title: "swift - defer"
date: 2020-02-02 21:25:39
---

## Defer

Uncommonly used, but typically used to execute code withinin a function everytime it exits. 


For example the function below will attempt to write "Hello World" to the file. Whether or not this function fails, will still print "Exiting."

    func writeHelloWorldToFile() {
        defer { print("Exiting." }

        let contents = "Hello World"
        let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
        let filename = path.appendingPathComponent("helloWorld.txt")

        do {
            try str.write(to: filename, atomically: true, encoding: String.Encoding.utf8)
        } catch {
            // Catch Error
        }
    }


### Ordering of Defer

If there are multiple usages of `defer`, the order is important. The `defer` statements are executed in reverse order of appearance, similar to how a stack works. (Last In First Out)

    func printNumbers() {
        defer { print("1" }
        defer { print("2" }
        defer { print("3" }

        print("A")
    }

    printNumbers() // prints "A" "3" "2" "1"
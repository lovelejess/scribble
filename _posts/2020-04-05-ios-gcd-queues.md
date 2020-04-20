---
title: "iOS - gcd queues"
date: 2020-04-05 16:04:41
---

## Threads vs Processes

### Threads
* Threads are used for smaller tasks
* Threads within the same process share the same address space

### Processes
* Processes are used for more heavier tasks – basically the execution of applications. 
* Processes within processes do not share the same address space

## Grand Central Dispatch

* An abstracton layer built on top of the original Grand Central Dispatch queue that makes it easier for developers to understand and create more efficient applications
* A **dispatch queue** performs tasks asynchronously and concurrently in the order they that are added to the queue
* A **serial dispatch queue** only performs tasks synchronously. The former task must execute and complete before the latter.
    * Use **serial queues** when you need to synchronize values


Which one will execute first? 


let q = DispatchQueue.global(qos: .userInteractive)

q.async { () -> Void in
    print("tic")
}

print("tac") 

> prints "tac" first because the queue will immediately return and execute the next line `print("tac")` and then sometime in the future `print("tact")` will execute.


How about this one?

let q1 = DispatchQueue(label: "queue1")
let q2 = DispatchQueue(label: "queue2")
let q3 = DispatchQueue(label: "queue3")

q1.async { () -> Void in
    print(1)
}

q2.async { () -> Void in
    print(2)
}

q3.async { () -> Void in
    print(3)
}

print("end")

> Who knows? Because each of the print statements are in three different, concurrent queues, the ordering is unknown.


### Beware of UIKit and CoreData

* `UIKit` and `CoreData` are examples of two iOS frameworks that are not thread safe.
* `UIKit` can only be execute on the `Main` thread
* `CoreData` objects can only be executed and accessed on the thread that they are created.

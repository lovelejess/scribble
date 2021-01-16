---
title: "swiftui - lazy stacks versus lists"
date: 2020-11-17 21:57:11
---

Notes from [Ray Wenderlich's Swift UI - Layout Interfaces](https://www.raywenderlich.com/17314449-swiftui-layout-interfaces/lessons/2)

Hacking with Swift has a great [article](https://www.hackingwithswift.com/quick-start/swiftui/how-to-lazy-load-views-using-lazyvstack-and-lazyhstack) as well

### Lists 
 
* Similar to `UITableView` and `UITableViewCell` in UIKit
* A specialized version of lazy v stacks
* They are more common to use
* They have built in functionality, such as swiping to delete, reordering, and styling (similar to table views)
* Lazy loads the data, but the data is loaded up front


```
struct ContentView: View {
    var body: some View {
        List {
            let formatter: DateFormatter = {
                let formatter = DateFormatter()
                formatter.timeStyle = .medium
                return formatter
            } ()
            ForEach(1...10000, id: \.self) { item in
                Text(
                    formatter.string(from: Date())
                )
            }
        }
    }
}
```


### Lazy Stacks

* Lazy loads the data, but the data is loaded just in time (aka when it comes into view)
* Use Lazy Stacks when you notice a performance issue


```
struct ContentView: View {
    var body: some View {
        ScrollView {
            LazyVStack {
                let formatter: DateFormatter = {
                    let formatter = DateFormatter()
                    formatter.timeStyle = .medium
                    return formatter
                } ()
                ForEach(1...10000, id: \.self){ item in
                    Text(
                        formatter.string(from: Date())
                    )
                }
            }
        }
    }
}
```
---
title: "swiftui - scrollviewreader"
date: 2020-11-18 08:35:18
---

Notes from [Ray Wenderlich's Swift UI - Layout Interfaces](https://www.raywenderlich.com/17314449-swiftui-layout-interfaces/lessons/3)


## ScrollViewReader

* Enables the user to navigate to a specific section in a scroll view
* Can encompass a scroll view

```
struct ContentView: View {
    var body: some View {
        ScrollViewReader { proxy in
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
}
```


but can also work as the content of the scroll view


```
struct ContentView: View {
    var body: some View {
        ScrollView {
            ScrollViewReader { proxy in
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
}
```
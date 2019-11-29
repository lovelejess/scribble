---
title: "application lifecycle"
date: 2019-11-29 09:32:41s
---

## Five States of Application Lifecycles

1. Not Running
2. Inactive
3. Active
4. Background
5. Suspended

* App can only run code when in foreground (active or inactive) or background (background)

## ViewController Lifecycle

```
viewDidLoad()
viewWillAppear()
viewDidAppear()
viewWillDisappear()
viewDidDisappear()
```

* `viewDidLoad`  - called when the view controller is created and loaded in memory. Usually called only once
* `viewWillAppear` - called right before the content view is presented onscreen and before the content view is added to the view hierarchy
* `viewDidAppear` - called when the content view is added to the view hierarchy. Use this method to trigger any events that need to occur as soon as the view is presented on screen, such as fetching and loading data or showing animation
* `viewWillDisappear` - called right before the content view is removed from the view hierarchy. Use this method to trigger any clean up events, such as resigning first responder
* `viewDidDisappear` - called when the content view is removed from the view hierarchy

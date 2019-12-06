---
title: "iOS - stack views"
date: 2019-12-01 08:05:23
---

Stack views create their own auto layout constraints for the elements contained within the stack views based on the alignment, distribution, and spacing


### Adding a Vertical Stack View

* To add a vertical stack view that fills the size of the parent view:
    * Leading Space to Safe Area
    * Trailing Space to Safe Area
    * Top Space to Safe Area
    * Bottom Space to Safe Area
* However, you'll notice that the stack view doesn't fill the size of the parent view since the constant size was automatically set when you added the above constraints. In order to do so, we must set the constant value for each of the constraints to 0.

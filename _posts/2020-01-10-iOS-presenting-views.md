---
title: "iOS - presenting views"
date: 2020-01-10 21:33:46
---

### Present Views Programmatically

* In Interface Builder:
    * Set the Custom Class to <VIEWCONTROLLER NAME>
    * Set Storyboard ID <VIEWCONTROLLER NAME>

    ```
    let storyboard = UIStoryboard (name: "Main", bundle: nil)
    let viewController = storyboard.instantiateViewController(withIdentifier: "ViewController") as! ViewController
    self.present(viewController, animated: true, completion: nil)
    ```
    See how the Rock View is presented in [Example in Roshambo](https://github.com/lovelejess/ios-udacity-nanodegree/blob/master/Roshambo/Roshambo/RoshamboViewController.swift#L69)

### Present Views Programmatically plus Segue

* In Interface Builder:
    * Add Segue from Origin View Controller to Destination View Controller
    * Add Segue Identifier

    ```
    performSegue(withIdentifier: "throwDownPaper", sender: self)
    ```
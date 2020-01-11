---
title: "iOS - table views"
date: 2020-01-10 18:39:44
---


### Simple TableViewController

#### ViewController + UITableViewDataSource
    //
    //  ViewController.swift
    //  SampleTableView
    //
    //  Created by Jessica Le on 01/10/20.
    //  Copyright (c) 2014 lovelejess. All rights reserved.
    //

    import UIKit

    // MARK: - ViewController: UIViewController, UITableViewDataSource

    class ViewController: UIViewController, UITableViewDataSource {
        
        // MARK: Properties
        
        // Use this string property as your reuse identifier, 
        // Storyboard has been set up for you using this String.
        let cellReuseIdentifier = "MyCellReuseIdentifier"
        
        // Choose some data to show in your table
        
        let model = [
            ["text" : "Apples", "detail" : "Green"],
            ["text" : "Oranges", "detail" : "Orange"],
            ["text" : "Bananas", "detail" : "Yellow"],
            ["text" : "Grapes", "detail" : "Purple"],
            ["text" : "Strawberries", "detail" : "Red"],
        ]
        
        // MARK: UITableViewDataSource
        
        // Add the two essential table data source methods here
        
        func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            return self.model.count;
        }
        
        func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
            let cell =  tableView.dequeueReusableCell(withIdentifier: self.cellReuseIdentifier)!
            
            let dictionary = self.model[(indexPath as NSIndexPath).row]
            
            cell.textLabel?.text = dictionary["text"]
            cell.detailTextLabel?.text = dictionary["detail"]
            
            return cell
        }
    }

#### Main.storyboard

1. Add `TableView` to `View`

    ![tableview-constraints](../images/tableview-constraints.png)  

    * Add the constraints

        ![tableview-constraints-specific](../images/tableview-constraints-specific.png)

    * Connect the `TableView`s `dataSource` property to the `view` 

        ![tableview-datasource](../images/tableview-datasource.png)

1. Create a prototype cell via `TableViewCell`
    * **Style:** Subtitle
    * **Identifier:** MyCellReuseIdentifier

    ![tableview-reuseidentifier](../images/tableview-reuseidentifier.png)     

### Result

![tableview-simple-table-view](../images/tableview-simple-table-view.png)  
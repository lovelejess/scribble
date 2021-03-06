---
title: angular databinding
date: 2018-01-07 19:35:31
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 1 and 2

<a href="https://github.com/lovelejess/angular-udemy/tree/master/basics-assignment-2-start" target="_blank">Link to Assignment 2 - Databinding</a>

## Databinding

### Output Data to Browser:

- **String Interpolation**

  {% raw %}
  `{{ data }}`
  {% endraw %}

  - Example: 

    {% raw %}
    `<p>{{ propertyText }}</p>`
    {% endraw %}

- **Property Binding**

  `[html property] = "data"`

  - Example: 
    ```
    # html inner text
    <p [innerText]="myElement"></p>
    
    # button example
    <button class="btn btn-primary" 
    [disabled]="conditionalBoolean">Add Server</button>
    ```

### React to User Events from Browser

- **Event Binding**

  `(event) = "expression"`
  
  - Example: 
    ```
    # button example
    <button class="btn btn-primary" 
    [disabled]="conditionalBoolean"
    (click)="onMethodEvent()">Add Server</button>

    # input example
    <input
    type="text"
    class="form-control"
    (input)="onMethodEvent($event)">
    ```
  

### Two Way Binding
  `[(ngModel)] = "data"`

  - Example: 
    ```
    # input example
    <input
    type="text"
    class="form-control"
    [(ngModel)]="propertyName">
    ```
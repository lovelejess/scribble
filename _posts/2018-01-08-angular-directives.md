---
title: angular directives
date: 2018-01-08 17:45:10
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 1 and 2

<a href="https://github.com/lovelejess/angular-udemy/tree/master/basics-assignment-3-start" target="_blank">Link to Assignment 3 - Directives</a>

## What are Directives?

- A **directive** is an instruction for a DOM element
- It is a marker on a DOM element (attribute, element name, CSS class) that Angular uses to compile to the HTML (`$compile`) and attach the instruction to the DOM element and its children


### Structural Directives

- annotated by the *
- `ngIf` and `ngFor` are most commonly used

- Example:

  `<p *ngIf="expression">Show me!</p>`

### ngIf 

- directive that conditionally adds or remove content from the DOM based on whether or not the expression evaluates to true or false

- `*ngIf` else
    -  can use the `ng-template` directive with a marker for `else` conditions

 {% raw %}
```
<p *ngIf="conditional expression; else markerThatMatchesngTemplate">I satisfied the if statement</p>
{% endraw %}

<ng-template #markerThatMatchesngTemplate>
  <p>I satisfy the else statement</p>
```

### Attribute Directives

- **attribute directives** only change the element that they were placed on. They don't add or remove elements, unlike structural directives.


### ngStyle && ngClass

- directives that allow you to set CSS styling on an HTML element conditionally.

 {% raw %}
  ```
  <p 
   [ngStyle]="{cssStyle: conditonalMethod()}"
   [ngClass]="{cssClass: conditionExpressionOrMethod}"> 
   This text will be styled according to the above expressions
  </p>
  ```
  {% endraw %}

### ngFor

- directive that displays an iterable data structure.
- remember to use the `let oneItem of iterable` syntax!
- remember to use a directive that can render iterables e.g. `<li></li>`.
  - `<p></p>` doesn't work

`<li *ngFor="let item of items"></li>`
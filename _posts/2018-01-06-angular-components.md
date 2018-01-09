---
title: angular basics & components
date: 2018-01-06 17:35:31
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 1 and 2

<a href="https://github.com/lovelejess/angular-udemy/tree/master/basics-assignment-1-start" target="_blank">Link to Assignment 2 - Databinding</a>

### How Does an Angular App Get Loaded and Started? (Lecture 13)
- Import main module (`app.module.ts`) into main.ts
- In the app module, bootstrap the component (`app.component.ts`)
- Import styling in component.


### Component Selectors (Lecture 14)

- There are many ways you can create and use component selectors. 

**Element Selector** 

```
# app.component.ts

@Component({
    selector: 'app-component'
})

# app.component.html

<app-component></app-component>

```

**Attribute Selector** 
- can use any html element
- in the angular component, the selector is denoted by the `[]`
- in the html, the selector is preceeded by a html element 

```
# app.component.ts

@Component({
    selector: '[app-component]'
})

# app.component.html

<div app-component></div>

```

**Class Selector** 
- in the angular component, the selector is denoted by the `.`
- in the html, the selector is preceeded by the html class tag

```
# app.component.ts

@Component({
    selector: '.app-component'
})

# app.component.html

<div class="app-component"></div>

```

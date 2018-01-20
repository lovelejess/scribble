---
title: angular binding - property, events, & input
date: 2018-01-19 21:17:15
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 5

## Property & Event Binding
- can use property binding on:
  - **HTML elements**
    - Native properties & events 
  - **Directives**
    - Custom properties & events
  - **Components**
    - Custom properties & events

## Input Binding
- enables you to pass data from parent to child component via **input properties** denoted by `@Input()` decorator

{% raw %}

  **child.component.ts**
  
    ```
    @Component({
      selector: 'app-child',
      template:
      `
      <p> I'm in the child component and my name is: {{ child }} <p>
      `
    })
    export class ChildComponent {
      @Input() child: string;
    }
    ```


  **parent.component.ts**

    ```
    @Component({
      selector: 'app-parent',
      template: 
      ` 
      <h3> Family </h3>
        <app-child 
          *ngFor="let parent of parents"
          [child]="parent">
        </app-child>
      `
    })

    export class ParentComponent {
      public parents: string[] = ['parent1', 'parent2'];
    }
    ```

  **the above code displays:**

{% endraw %}


    ```
    Family
  
    I'm in the child component and my name is: parent1

    I'm in the child component and my name is: parent2

    ```

## Input Binding Alias

- can give an alias to an input binding for clearer naming. 

- in the below example:  
  - our input binding alias is named `parentAlias`, which will be used in the parent component in the property binding: `[parentAlias]`



  **child.component.ts**
  
    ```
    export class ChildComponent {
      @Input('parentAlias') child: string;
    }
    ```

  **parent.component.ts**

    ```
    @Component({
      selector: 'app-parent',
      template: 
      ` 
      <h3> Family </h3>
        <app-child 
          *ngFor="let parent of parents"
          [parentAlias]="parent">
        </app-child>
      `
    })
    ```

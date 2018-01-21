---
title: angular custom event binding
date: 2018-01-20 10:13:15
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 5

## Output (Event) Binding
- enables the parent to listen for child events that is emitted (`EventEmitter`) via **output properties** denoted by `@Output()` decorator

- in the below example, when we toggle the 'Is Child Crying? button, it will display the name of the child and whether or not it is crying.
- The parent binds to the event `childrenCrying` and when the event is invoked, the `@Output` decorator in the `ChildComponent` named `childrenCrying` emits a notification to the parent. The parent then invokes the `onChildrenCrying($event)` event handler.



{% raw %}

  **child.component.ts**
  
    ```
    @Component({
      selector: 'app-child',
      template: 
      `
        <button
        class="btn btn-primary"
        (click)="toggleChildCrying()">Is Child Crying?</button>
      `
    })

    export class ChildComponent implements OnInit {
      @Output() public childrenCrying = new EventEmitter<{name: string, cryingStatus: string}>();

      public children: {}[] = [];
      public cryingStatus: string = 'not crying';

      constructor() { }

      ngOnInit() {
      }

      toggleChildCrying() {
        this.cryingStatus = this.cryingStatus == 'not crying' ? 'crying' : 'not crying'
        this.childrenCrying.emit({
          name: 'lola',
          cryingStatus: this.cryingStatus
        });
      }
    }

    ```

  **parent.component.ts**

    ```
    @Component({
      selector: 'app-parent',
      template: 
      `
        <app-child
          (childrenCrying)="onChildrenCrying($event)">
        </app-child>

        <h3> CryBabies </h3>
        
        <li *ngFor="let child of childrenCryingList">{{ child.name }} is {{ child.cryingStatus }} !</li>

      `
    })

    export class ParentComponent implements OnInit {
      public childrenCryingList = [];
      
      constructor() { }

      ngOnInit() {
      }
      onChildrenCrying(child: {name: string, cryingStatus: string}) {
        this.childrenCryingList.push({name: child.name, cryingStatus: child.cryingStatus});
      }
    }
    ```


  **the above code displays:**

{% endraw %}

    ![sample image](../images/is-baby-crying.png)
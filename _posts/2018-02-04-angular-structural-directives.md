---
title: angular structural directives
date: 2018-02-04 10:56:06
---

### Structural Directives
- **structural directives** syntactically look like normal HTML attributes but has a leading `*` and affect/change the *whole area* in the DOM.

    - structural directive examples: `*ngFor` and `*ngIf`


- the `*` is syntactic sugar for the `<ng-template>` element.  

- for example: **ngIf**

{% raw %}
    ```
    <div *ngIf="true === true">
        {{ item }}
    </div>
    ```
- is equivalent to:

    ```
    <ng-template ngIf="true === true">
        {{ item }}
    </ng-template>
    ```
{% endraw %}

## Creating Custom Structural Directives

- let's create the opposite of `*ngIf`, `*appUnless`

- if we were to use `*ngIf`:

    ```
    <div *ngIf="!onlyOdd">
        <li
            class="list-group-item"
            [ngClass]="{even: even % 2 == 0}"
            *ngFor="let even of evenNumbers">
            {{ even }}
        </li>
      </div>
    ```
- instead we can let the `appUnless` directive handle the opposite conitional checking:

    ```
    <div *appUnless="onlyOdd">
        <li
            class="list-group-item"
            [ngClass]="{even: even % 2 == 0}"
            *ngFor="let even of evenNumbers">
            {{ even }}
        </li>
    </div>
    ```

- **unless.directive.ts** 


    ```
    import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

    @Directive({
        selector: '[appUnless]'
    })
    export class UnlessDirective {
        @Input() set appUnless(condition: boolean) {
            if (!condition) {
            this.vcRef.createEmbeddedView(this.templateRef);
            } else {
            this.vcRef.clear();
            }

        }
        constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef ) { }
    }

    ```

- **NOTE:** make sure you match the name of the `@Input` property to the directive selector name. So 

    ```
    @Input() set appUnless(condition: boolean)
    ``` 

- must match: 

    ```
    @Directive({
        selector: '[appUnless]'
    })
    ```
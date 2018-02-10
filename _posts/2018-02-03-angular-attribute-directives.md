---
title: angular attribute directives
date: 2018-02-03 10:57:06
---

## Attribute vs Structural Directives
- **attribute directives** syntactically looks like normal HTML attributes and only affect/change the element they are added to; whereas **structural directives** syntactically look like normal HTML attributes but has a leading `*` and affect/change the *whole area* in the DOM.

    - attribute directive examples: `[ngClass]` and `[ngStyle]`
    - structural directive examples: `*ngFor` and `*ngIf`

## Custom Attribute Directives

 **basic-highlight.directive.ts**
    
```
    import { Directive, Renderer2, ElementRef, HostListener, HostBinding } from '@angular/core';

    @Directive({
        selector: '[appBetterHighlight]'
        })

    export class BetterHighlightDirective {
        @HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';

        constructor(private elementRef: ElementRef, private renderer: Renderer2) { }

        @HostListener('mouseenter') mouseover(eventData: Event) {
            //below line does the same as the next, but manipulates the DOM via renderer which is not ideal. 
            this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'blue');
            this.backgroundColor = 'blue';
        }

        @HostListener('mouseleave') mouseleave(eventData: Event) {
             //below line does the same as the next, but manipulates the DOM via renderer which is not ideal.
            this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'transparent'); 
            this.backgroundColor = 'transparent';
        }
    }
```
        

**app.component.html**

    <p appBetterHighlight> Style me!</p>

- use **Angular Renderer** class to change the style of a HTML element to avoid manipulating the DOM directly. You always want to ensure that controller or component never manipulates the DOM directly because you want to ensure separation of concerns and not cause any unintended behavior.

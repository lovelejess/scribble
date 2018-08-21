---
title: angular observables and subjects
date: 2018-04-01 17:20:30
---

## What is a Subject?
* a **subject** can act as an observable and a observer at the same time! Therefore it can consume and produce data. 
* As an **observer**, it can subscribe to one or more observables, and because it's an **observable** it can pass itself as an **observer** to itself.

* **example:**
- in the below example, the `app.component.ts` subscribes to the `userActivated` subject, which gets invoked each time the "Activate" button is clicked and will display which user is activated.  

- **user.service.ts**
    ```
        import { Subject } from "rxjs";

        export class UsersService {
            public userActivated = new Subject();
        }
    ```

- **user.component.ts**
    ```
        import { Component, OnInit } from '@angular/core';
        import { ActivatedRoute, Params } from '@angular/router';
        import { UsersService } from '../users.service';

        @Component({
            selector: 'app-user',
            template: `<button class="btn btn-primary" (click)="onActivate()">Activate</button>`,
            styleUrls: ['./user.component.css']
        })
        export class UserComponent implements OnInit {
            id: number;

            constructor(private usersService: UsersService) { }

            public onActivate(): void {
                this.usersService.userActivated.next(this.id);
            }
        }

    ```
- **app.component.ts**
  {% raw %}
    ```
        import { Component } from '@angular/core';
        import { OnInit } from '@angular/core/src/metadata/lifecycle_hooks';
        import { UsersService } from './users.service';

        @Component({
            selector: 'app-root',
            template: 
            `
                <div class="row">
                    <p>User 1 {{ user1Activated ? '(activated)' : ''}}</p>
                    <p>User 2 {{ user2Activated ? '(activated)' : ''}}</p>
                </div>
            `,
            styleUrls: ['./app.component.css']
        })
        export class AppComponent implements OnInit{
            public user1Activated: boolean = false;
            public user2Activated: boolean = false;

            constructor(private userService: UsersService) {

            }
            ngOnInit() {
                this.userService.userActivated.subscribe(
                (id: number) => {
                    if (id == 1) {
                        this.user1Activated = true;
                    } else if (id == 2) {
                        this.user2Activated = true;
                    }
                }
                );
            }

        }
    ```
    {% endraw %}

## Subject vs Observer vs Observable
* an **observer** is an object that contains information on *how* to handle a data set.
    * Instructions of an assembly line. Tells you what the next steps are for a package, what to do with it when it errors, and what to do with it when it completes.
* an **observable** is a function that takes an **observer** and gets executed
    * The assembler. Takes the instructions and executes them when it receives a package.
* a **subject** is both an observer and an observable. It can consume and produce data. 
  * The supervisor of the assembly line. They tell you how it's done and they can do it themselves.

## When to Use Subject vs Observable?
* Because an observable is just a function, all executions of an observable are independent from one another. Whereas all subscribers of a subject share the same execution, therefore it will receive the same data set.

### **Examples:**

* **Observable:**

    ```
        const myObservable: Observable<string> = Observable.create((observer: Observer<string>) => {
        setInterval(() => {
            observer.next(' package recieved: ' + Math.random().toString());
        }, 2000);
        });

        myObservable.subscribe(
            (data: string) => { console.log('myObservable A ' + data);},
            (error: string) => { console.log(error);},
            () => { console.log('completed');},
        );

        myObservable.subscribe(
            (data: string) => { console.log('myObservable B ' + data);},
            (error: string) => { console.log(error);},
            () => { console.log('completed');},
        );
    ```

    * **Output:**
      * Notice that the values are different:


    ```
    myObservable A  package recieved: 0.6514089814911219
    myObservable B  package recieved: 0.22202856575373153

    ```

* **Subject:**

    ```
        var subject = new Subject();
        subject.subscribe(
            (data: string) => { console.log('myObservable A ' + data);},
            (error: string) => { console.log(error);},
            () => { console.log('completed');},
        );

        subject.subscribe(
            (data: string) => { console.log('myObservable B ' + data);},
            (error: string) => { console.log(error);},
            () => { console.log('completed');},
        );

        myObservable.subscribe(subject);
    ```

    * **Output:**
      * Notice that the values are the same:

    ```
    myObservable A  package recieved: 0.2996805531256037
    myObservable B  package recieved: 0.2996805531256037
    ```
---
title: "angular forms: template-driven"
date: 2018-04-14 14:43:02
---

## Angular Forms
* Two Approaches to handling forms:
  * **Template-Driven**
    - angular infers the Form Object from the DOM
  * **Reactive**
    - Form is created programmatically and syncrhonized with the DOM


## Template-Driven Approach

- **app.component.html**

  -  `(ngSubmit)="onSubmit(userNameForm) #userNameForm="ngForm"`
    - `onSubmit` is method that is invoked when the event for `ngSubmit` is emitted (form is submitted)
    - `#userNameForm` binds to `ngForm` and is passed into the `onSubmit` method so that the method has access to the `ngForm` 
  
    ```
      <div class="container">
        <div class="row">
          <form (ngSubmit)="onSubmit(userNameForm)" #userNameForm="ngForm">
            <div id="user-data">
              <div class="form-group">
                <label for="username">Username</label>
                <input
                      type="text"
                      id="username"
                      class="form-control"
                      ngModel
                      name="username"
                >
              </div>
            <button class="btn btn-primary" type="submit">Submit</button>
          </form>
        </div>
      </div>
    ```

- **app.component.ts**

  ```
    import { Component } from '@angular/core';
    import { NgForm } from '@angular/forms';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      public suggestUserName() {
        const suggestedName = 'Superuser';
      }

      public onSubmit(form: NgForm) {
        console.log("submitted")
        console.log(form)
      }
    }
  ```

- or can use the `@ViewChild()` way:

- **app.component.html**
    ```
      <div class="container">
        <div class="row">
          <form (ngSubmit)="onSubmit()" #userNameForm="ngForm">
            <div id="user-data">
              <div class="form-group">
                <label for="username">Username</label>
                <input
                      type="text"
                      id="username"
                      class="form-control"
                      ngModel
                      name="username"
                >
              </div>
            <button class="btn btn-primary" type="submit">Submit</button>
          </form>
        </div>
      </div>
    ```

- **app.component.ts**

  ```
    import { Component } from '@angular/core';
    import { NgForm } from '@angular/forms';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })

    export class AppComponent {
       @ViewChild('userNameForm') signupForm: NgForm;

      public suggestUserName() {
        const suggestedName = 'Superuser';
      }

      public onSubmit() {
        console.log(this.signupForm);
      }
    }
  ```
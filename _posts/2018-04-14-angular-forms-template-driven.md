---
title: "angular forms: template-driven"
date: 2018-04-14 14:43:02
---

## Angular Forms
* Two Approaches to handling forms:
  * **Template-Driven**
    - angular infers the Form Object from the DOM
  * **Reactive**
    - Form is created programmatically and synchronized with the DOM


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

- The above code generates this:

![sample image](../images/template-driven-forms.png)


## Form Validation
- **required** input
  - invalidates form if input is missing
  - use keyword `required`

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
                    required
              >
            </div>
          <button
            class="btn btn-primary"
            type="submit"
            [disabled]="!userNameForm.valid"
            [ngClass]="{ 'input.ng-invalid' : !userNameForm.valid }">
            Submit
          </button>
        </form>
      </div>
    </div>
  ```
- the `ngClass` styling above adds a red border around the invalid input boxes after they have been touched

- **app.component.css**
  ```
    input.ng-invalid.ng-touched {
      border: 1px solid red;
    }
  ```

- The above code generates this:

![sample image](../images/template-driven-form-validation.png)



## Error Messages for Form Validation: One Way Binding
- bind to the `ngModel` via template binding:

- the below code will add a 'Please enter valid email' message right below the input box if the email is invalid and the input box has been touched.

- **app.component.html**
  ```
    <div class="form-group">
      <label for="email">Mail</label>
      <input
            type="email"
            id="email"
            class="form-control"
            ngModel
            name="email"
            required
            #email="ngModel"
      >
      <span class="help-block"
            *ngIf="!email.valid && email.touched">
            Please enter valid email!
      </span>
    </div>
  ```

- The above code generates this:
  ![sample image](../images/form-validation-email-error-message.png)


## Default Values in Forms
- property bind to `ngModel` and add a `name` attribute

- **app.component.html**

  ```
    <div class="form-group">
        <label for="secret">Secret Questions</label>
        <select
              id="secret"
              class="form-control"
              [ngModel]="defaultQuestion"
              name="secret">
          <option value="pet">Your first Pet?</option>
          <option value="teacher">Your first teacher?</option>
        </select>
    </div>
  ```

- **app.component.ts**

  ```
    export class AppComponent {
      public defaultQuestion: String = "teacher";
    }
  ```

## Form Validation: Two Way Binding
- use two way binding to bind to the input answers

- **app.component.html**
  ```
    <div class="form-group">
      <label for="secret">Secret Questions</label>
      <select
            id="secret"
            class="form-control"
            [ngModel]="defaultQuestion"
            name="secret">
        <option value="pet">Your first Pet?</option>
        <option value="teacher">Your first teacher?</option>
      </select>
    </div>

    <div class="form-group">
      <textarea
        name="questionAnswer"
        rows="3"
        class="form-control"
        [(ngModel)]="questionAnswer">
      </textarea>
    </div>

    <p>Your reply: {{ questionAnswer }} </p>
  ```

- **app.component.ts**

  ```
    export class AppComponent {
      public questionAnswer: String = "";
    }

  ```
- The above code generates this:

![sample image](../images/two-way-binding-forms-template-driven.png)

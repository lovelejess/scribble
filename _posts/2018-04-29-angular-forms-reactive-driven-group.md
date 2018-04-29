---
title: "angular forms: reactive-driven "
date: 2018-04-29 12:10:30
---


## Angular Forms
* Two Approaches to handling forms:
  * **Template-Driven**
    - angular infers the Form Object from the DOM
  * **Reactive**
    - Form is created programmatically and synchronized with the DOM


## Reactive-Driven Approach
- use the Angular ReactiveFormsModule in your `app.module.ts`
- use the Angular FormGroup in your `app.component.ts`
- use the Angular FormControl for each form input

- **app.component.ts**
  - our FormGroup is the `signUpForm` which we instantiate before the DOM is rendered by `ngOnInit` in the angular lifecycle
  - create a FormGroup with a FormControl

  ```
    import { Component, OnInit } from '@angular/core';
    import { FormGroup } from '@angular/forms';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent implements OnInit {
      genders = ['male', 'female'];
      public signUpForm: FormGroup;

      public ngOnInit() {
        this.signUpForm = new FormGroup({
          'username': new FormControl(null),
          'email': new FormControl(null),
          'gender': new FormControl('female')
        });
      }
    }
  ```

- **app.component.html**
  - need to override the default angular form, so we will bind your `formGroup` to the name you defined in your `app.component.ts` 
  - example:
    - ` <form [formGroup]="signUpForm">`
  - we will also need to create and bind to each of our `FormControl`
    - example: 
      - `formControlName="username"`
```
  <form [formGroup]="signUpForm">
    <div class="form-group">
      <label for="username">Username</label>
      <input
        type="text"
        id="username"
        formControlName="username"
        class="form-control">
    </div>
    <div class="form-group">
      <label for="email">email</label>
      <input
        type="text"
        id="email"
        formControlName="email"
        class="form-control">
    </div>
    <div class="radio" *ngFor="let gender of genders">
      <label>
        <input
          type="radio"
          formControlName="gender"
          [value]="gender">{{ gender }}
      </label>
    </div>
    <button class="btn btn-primary" type="submit">Submit</button>
  </form>
```
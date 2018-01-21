---
title: angular templates
date: 2018-01-21 11:40:06
---

> Currently enrolled in <a href="https://www.udemy.com/the-complete-guide-to-angular-2" target="_blank">Udemy's- the complete guide to angular 2.</a> Notes from Lesson 5

## Local Variables in Templates
- Can use named variables in templates via `#` prefix. The variable will only exist in the scope of anywhere after the variable has been defined in the template.
- for example: 
  - instead of declaring an `ngModel` and declaring a property for that component, you can use a **locally referenced template variable** (first example)
  - in the .html file, the `#serverNameInput` variable defined in the input form is passed in to the `click` event's method `onAddServer()` (second example)
  - in the .ts file, the `onAddServer` is able to access that `HTMLInputElement` and retrieve that value of the variable that was passed in.

  **server.component.html**

  ```
    <input 
      type="text" 
      class="form-control"
      [(ngModel)]="newServerName"
    >

    <button
      class="btn btn-primary"
      (click)="onAddServer()">Add Server
    </button>
  ```

  **server.component.ts**

  ```
    export class ServerComponent implements OnInit {
      public newServerName = '';

      onAddServer() {
          this.serverCreated.emit({
            serverName: this.newServerName, 
            serverContent: this.newServerContent
          });
        }
    }
  ```

- the above code is equivalent to:

  **server.component.html**

  ```
    <input 
      type="text" 
      class="form-control"
      #serverNameInput
    >

    <button
      class="btn btn-primary"
      (click)="onAddServer(serverNameInput)">Add Server
    </button>
  ```


  **server.component.ts**

  ```
    export class ServerComponent implements OnInit {
        
      onAddServer(serverName: HTMLInputElement) {
          this.serverCreated.emit({
            serverName: serverName.value, 
            serverContent: this.newServerContent
          });
        }
    }
  ```


## Accessing Template Variables via @ViewChild
- Can store the value of the template variables in your typescript file by using the decorator `@ViewChild()`
- Make sure that you don't modify the value of this variable in your .ts file because that violates separation of concerns, and cause unwanted behavior.


  **server.component.html**

    ```
      <input 
        type="text" 
        class="form-control"
        #serverName
      >

      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server
      </button>
    ```


  **server.component.ts**

    ```
    export class ServerComponent implements OnInit {
      @ViewChild('serverName') serverName: ElementRef; 
      // @ViewChild(ServerComponent) serverName: ElementRef; <-- can use the component type itself istead of string name

      onAddServer() {
          this.serverCreated.emit({
            serverName: this.serverName.nativeElement.value, 
            serverContent: this.newServerContent
          });
        }
    }
    ```


## Content Project via ng-content
- While property binding is great for simple html properties, it's not great for complext html rendering and reusbility. 
- Content projection allows for component resuability by placing the `<ng-content>` directive as a placeholder.
- For example, if you wanted to reuse 


  **servers.component.ts**

    ```
    @Component({
    selector: 'app-servers',
    template: `
      <div>
        <p>
          <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
          <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
        </p>
      </div>
    `
    })

    export class ServersComponent implements OnInit {
      @Input('srvElement') public element: {type: string, name: string, content: string};
      constructor() { }
    }

    ```


  **app.component.ts**

    ```
    @Component({
      selector: 'app-root',
      template: `
        <div class="col-xs-12">
          <app-servers 
            *ngFor="let serverElement of serverElements"
            [srvElement]="serverElement">
          </app-servers>
        </div>
      `
    })

    export class AppComponent {
      public serverElements = [{type: 'server', name: 'TestServer', content: 'Just a test!'}];

    ```

- the above code is equivalent to the below code, since we replace the contents inside the `<div>` with `<ng-content>` in `servers.component.ts` and in the component that is rendering the `<app-servers>` directive `app.component.ts`, we move the contents that we replaced `<ng-content>` inside the `<app-servers>` directive. Now `servers.component.ts` is resusable and we can render servers however we please!

  **servers.component.ts**

    ```
    @Component({
    selector: 'app-servers',
    template: `
      <div>
        <ng-content>
      </div>
    `
    })

    export class ServersComponent implements OnInit {
      @Input('srvElement') public element: {type: string, name: string, content: string};
      constructor() { }
    }

    ```

  **app.component.ts**

    ```
    @Component({
      selector: 'app-root',
      template: `
        <div class="col-xs-12">
          <app-servers 
            *ngFor="let serverElement of serverElements"
            [srvElement]="serverElement">
            <p>
              <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
              <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
            </p>
          </app-servers>
        </div>
      `
    })

    export class AppComponent {
      public serverElements = [{type: 'server', name: 'TestServer', content: 'Just a test!'}];

    ```
---
title: angular services and dependency injection
date: 2018-02-10 02:06:25
---

### Angular Services
- services provide a way for an **application to organize its business logic** into a common place and to **maintain and share data throughout the application's lifetime**. 
- services in angular are **hierarchical**. Wherever you inject them, the same instance of the service is available to itself and all of its children. 
    - Injecting a service at **`AppModule`** level:
        - the same service instance is provided throughout the **entire application**
    - Injecting a service at **`AppComponent`** level:
        - the same service instance is provided for **all the components** in the application and all of its children, but not for any other services
    - Injecting a service at **`Component`** level:
        - the same service instance is provided throughout the **component itself and all of its children**

    


### How To Create an Angular Service
#### 1. Create your service:

- **restaurant.service.ts**

    ```
    export class RestaurantService {
        private order: Order = [];

        addOrder(order: Order){
            this.order.push(order);
        }
    }

    ```

#### 2. Inject the service in the app module, app component, or component:

#### App Module Level: 
-  the key is to inject the service via the `app.module.ts` `providers` list

- **lovelejess-restaurant.component.ts**

    ```
    import { RestaurantService }

    @Component({
        selector: 'app-lovelejess-restaurant'
    })

    export class LovelejessRestaurantComponent {
        constructor(private restaurantService: RestaurantService) {}

        onOrderReceived(order: Order) {
            this.restaurantService.addOrder(order);
        }
    }
    ```
- **app.module.ts**

    ```
    @NgModule({
    declarations: [
        AppComponent,
        LovelejessRestaurantComponent
    ],
    imports: [
        BrowserModule,
        FormsModule,
        HttpModule
    ],
    providers: [RestaurantService], 
    bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```
    
#### App Component Level
-  the key is to inject the service via the `app.component.ts` `providers` list

- **app.component.ts**

    ```
    import { RestaurantService }

    @Component({
        selector: 'app-component'
        providers: [RestaurantService]
    })

    export class AppComponent {
        constructor(private restaurantService: RestaurantService) {}

        onOrderReceived(order: Order) {
            this.restaurantService.addOrder(order);
        }
    }
    ```



#### Component Level
-  the key is to inject the service via the component's `providers` list

- **lovelejess-restaurant.component.ts**

    ```
    import { RestaurantService }

    @Component({
        selector: 'app-lovelejess-restaurant'
        providers: [RestaurantService]
    })

    export class LovelejessRestaurantComponent {
        constructor(private restaurantService: RestaurantService) {}

        onOrderReceived(order: Order) {
            this.restaurantService.addOrder(order);
        }
    }
    ```
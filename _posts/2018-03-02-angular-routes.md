---
title: angular routes query parameterization
date: 2018-03-03 14:00:43
---

### Angular Query Parameters

- To create a url with parameters, query parameters, and fragments, use the following properties: 
    `routerLink`, `queryParams`, `fragment`
- example to create a url for this: `http://localhost:4200/servers/5/edit?allowEdit=1#loading`

- in **html**:

    ```
        <a
            [routerLink]="['/servers', 5, 'edit']"
            [queryParams]="{allowEdit: '1'}"
            fragment="loading"
        </a>
    ```

- in **typescript**:

    ```
    onLoadServer() {
        this.router.navigate( ["/servers", 5, 'edit'], 
                            { queryParams: { allowEdit: '1' }, 
                                fragment: 'loading'
                            });
    }
    ```

### Angular Routes Hierarchy
- the **order of the routes** defined in the `Routes` array in `app.module.ts` is important because they are hierarchical.
- more specifcally, to add a "catch all" `/not-found` url, make sure you add that last in your `routes` list. 

    ```
    const appRoutes: Routes = [
    {path: '', component: HomeComponent},
    {path: 'not-found', component: PageNotFoundComponent},
    {path: '**', redirectTo: '/not-found' }
    ];
    ```

- the path defined as `**` is a wildcard and because it is last in the hierarchy it is a catch all path that will redirect to `/not-found`

- a common gotcha is to define the path as empty and redirect to: 

    ```
    { path: '', redirectTo: '/somewhere-else' } 
    ```
- but this will always redirect you to `/somewhere-else` you no matter what URL you navigate to because Angular's default matching strategy is to match by prefix. Since `/here` is preceeded with `/` it will always match ` ` , so be sure to use `pathMatch: 'full'` to indicate that that the URL has to match exactly.

    ```
    { path: '', redirectTo: '/somewhere-else', pathMatch: 'full' } 
    ```

### Guarding Routes via CanActivate
- adding the `canActivate` property to a defined `Route` will guard access to that route if set to `true` or `false`.
    - can also do the same with it's nested children via `canActivateChild`
- must implement the `CanActivate` interface and define the `Route` with the `canActivate` property as follows:

- **auth-guard.service.ts**
    ```
    @Injectable()
    export class AuthGuardService implements CanActivate {

        public canActivate(route: ActivatedRouteSnapshot, 
                        state: RouterStateSnapshot): boolean | Observable<boolean> | Promise<boolean> {
            return true;
        }
    }

    ``` 

- **app-routing.module.ts**
    ```
    const appRoutes: Routes = [
        {path: '', component: HomeComponent, pathMatch: 'full'},
        {path: 'servers', canActivate: [AuthGuardService], component: ServersComponent},
    ];

    @NgModule({
        imports: [  
            RouterModule.forRoot(appRoutes)
        ],
        exports: [RouterModule]
    })
    export class AppRoutingModule {
        
    }

    ```


### Passing Static Data through Routes
- use the `data` property in the `Route` object.

- example: 
    - navigating to `/this-url-doesnt-exist` will re-route you to `/not-found` and display **"Page Not Found!"**


- **app-routing.module.ts**
    ```
    const appRoutes: Routes = [
        {path: 'not-found', component: ErrorPageComponent, 
            data: {message: 'Page Not Found!'}},
        {path: '**', redirectTo: '/not-found' }
    ];

    ```

- **error-page.component.html**

    ```
    import { Component, OnInit } from '@angular/core';
    import { Data, ActivatedRoute } from '@angular/router';

    @Component({
        selector: 'app-error-page',
        template: `<h4>{{ errorMessage }}</h4>`,
        styleUrls: ['./error-page.component.css']
    })
    export class ErrorPageComponent implements OnInit {
        public errorMessage: string;
        constructor(private route: ActivatedRoute) { }

        ngOnInit() {
            // this.errorMessage = this.route.snapshot.data['message']; 
            this.route.data.subscribe((data: Data) => {
                this.errorMessage = data.message;
            });
        }
    }

    ```
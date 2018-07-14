<!-- .slide: data-background="../images/title-slide.jpg" -->
<!-- .slide: id="routing" -->
## Building Applications with Angular

# Routing

---
<!-- .slide: id="routing-configuring-routing" -->
## Configuring Routing

- Set `base` tag in the `<head>` of `src/index.html` to tell Angular where routes start

#### _src/index.html_
```html
 <base href="/">
```

- Allows applications to be hosted below the root of the domain
  - E.g., `http://angular.io/some/path` would set `base` to `/some/path`

---
<!-- .slide: id="routing-defining-routes" -->
## Defining Routes

- Create array of type `Routes` that specifies what to do with different paths

#### _src/app/app.routes.ts_
```ts
import { Routes } from '@angular/router';
import { ToDoListComponent } from './to-do-list/to-do-list.component';
import { FancyListComponent } from './fancy-list/fancy-list.component';

export const routeConfig: Routes = [
  { path: '', redirectTo: '/todo', pathMatch: 'full' },
  { path: 'todo', component: ToDoListComponent },
  { path: 'fancy', component: FancyListComponent }
];
```

---
<!-- .slide: id="routing-common-attributes-in-routes" -->
## Common Attributes in Routes

| Attribute    | Use |
|--------------|--------------------------------------------------------------|
| `path`       | Browser URL for route                                        |
| `component`  | Component displayed for route                                |
| `redirectTo` | Where to redirect                                            |
| `pathMatch`  | Match full `path` or just the beginning?                     |
| `children`   | Array of route definitions objects representing child routes |

- `component` and `redirectTo` are mutually exclusive

---
<!-- .slide: id="routing-adding-router-module" -->
## Adding Router Module

- Import `routeConfig` into `app.module.ts`
- Add `RouterModule.forRoot(routeConfig)` to `imports`

#### _src/app/app.module.ts_
```ts
import { RouterModule, Routes } from '@angular/router';
import { routeConfig } from './app.routes';

@NgModule({
  // ...
  imports: [
    // ...
    RouterModule.forRoot(routeConfig)
  ],
  // ...
})
export class AppModule { }
```

---

<!-- .slide: id="routing-adding-router-outlet" -->
## Adding RouterOutlet

- Use `<router-outlet></router-outlet>` to show where to display routed content
- Angular dynamically places content *after* the tag

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<nav>
  <a [routerLink]="['/todo']">Classic</a>
  <a [routerLink]="['/fancy']">Fancy</a>
</nav>
<router-outlet></router-outlet>
<app-generic-input (newItem)="onNewItem($event)"></app-generic-input>
```

- Use `routerLink` to create links between dynamic components
  - Parameter is an array of values

---

<!-- .slide: id="routing-using-service" -->
## Navigate using Router service

- Alternatively, you can navigate to a route by calling the `navigate` function on the router with the path array as the argument. For example to navigate to component-one, we could use:

```ts
import { Router } from '@angular/router';

@Component({ ... })
export class ProductDetails {
  constructor(private router: Router) {}

  navigateToOne() {
    router.navigate(['/component-one']);
  }
}
```

---

<!-- .slide: id="routing-passing-route-parameters" -->
## Passing Route Parameters

Adding `:id` in the path of the `product-details` route places the parameter in the route's path. For example `localhost:3000/product-details/5`

```ts
export const routes: Routes = [
  { path: 'product-details/:id', component: ProductDetails }
];
```

`routerLink` directive passes an array which specifies the path and the route parameter. This links the route to the parameter.

```html
<a *ngFor="let product of products"
  [routerLink]="['/product-details', product.id]">
  {{ product.name }}
</a>
```

---
<!-- .slide: id="routing-getting-route-parameters" -->
## Getting Route Parameters

The `ActivatedRoute` service provides a `params` Observable which we can subscribe to get the route parameters.

```ts
import { ActivatedRoute } from '@angular/router';

@Component({ ... })
export class ProductDetails {
  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.route.params.subscribe(params => {
       // params['id'] is the product's id
    });
  }
}
```

---
<!-- .slide: id="routing-handling-404" -->
## Handling 404

To detect unmatched routes you can use the `**` wildcard in the path.
This wildcard will actually match all URLs,
therefore its important that you list any other specific route paths prior to the `**` route.

This is usually your last route in the route configuration.

```ts
const routes: Routes = [
  { path: '/component-one', component: ComponentOne },
  { path: '/component-two', component: ComponentTwo },
  { path: '/component-three', component: ComponentThree },
  ...
  { path: '**', component: Four0FourComponent }
];
```

Note: This does not return a 404 status code.

---
<!-- .slide: id="routing-passing-query-parameters" -->
## Passing query Parameters

- Use `[queryParams]` along with `[routerLink]` to pass query parameters

```html
<a [routerLink]="['/product-list']" [queryParams]="{ page: 99 }">Go to Page 99</a>
```

- Alternatively, we can navigate programmatically using the `Router` service:

```ts
  goToPage(pageNum) {
    this.router.navigate(['/product-list'], { queryParams: { page: pageNum } });
  }
```

---

<!-- .slide: id="routing-advanced-passing-optional-parameters-2" -->
## Getting Query Parameters

- Similar to reading route parameters, the `ActivatedRoute` service returns an Observable we can subscribe to to read the query parameters

```ts

ngOnInit() {
  this.route
    .queryParams
    .subscribe(params => {
      this.page = params['page'] || 0;
    });
}

nextPage() {
  this.router.navigate(['/product-list'],
    { queryParams: { page: this.page + 1 } });
}
```

---

<!-- .slide: id="routing-route-authorization-1" -->
## Route Authorization (1/3)

- To control whether the user can navigate to or away from a given route, we can use route guards.
- In order to use route guards, we must register them with the specific routes we want them to run for.

```ts
const routes: Routes = [
  { path: 'home', component: HomePage },
  {
    path: 'accounts',
    component: AccountPage,
    canActivate: [LoginRouteGuard],
    canDeactivate: [SaveFormsGuard]
  }
];
```

---

<!-- .slide: id="routing-route-authorization-2" -->
## Route Authorization (2/3)

- To guard a route against unauthorized activation, we can implement the `CanActivate` interface by implementing the `canActivate` function.
- When `canActivate` returns `true`, the user can activate the route. When `canActivate` returns `false`, the user cannot access the route.

```ts
@Injectable()
export class LoginRouteGuard implements CanActivate {

  constructor(private loginService: LoginService) {}

  canActivate() {
    return this.loginService.isLoggedIn();
  }
}
```

---

<!-- .slide: id="routing-route-authorization-3" -->
## Route Authorization (3/3)

- `CanDeactivate` works in a similar way to `CanActivate` but there are some important differences
- The `canDeactivate` function passes the component being deactivated as an argument to the function.
- We can use that component to determine whether the user can deactivate.

```ts
canDeactivate(component: AccountPage) {
  return component.areFormsSaved();
}
```

---

## Child Routes (1/2)
<!-- .slide: id="routing-child-routes-1" -->

- The child routes are only reachable from within product details

```ts
export const routes: Routes = [
  { path: '', redirectTo: 'product-list', pathMatch: 'full' },
  { path: 'product-list', component: ProductList },
  { path: 'product-details/:id', component: ProductDetails,
    children: [
      { path: '', component: Overview },
      { path: 'specs', component: Specs }
    ]
  }
];
```

- The parent product-details template will contain a `<router-outlet></router-outlet>` to display the contents of the child.

---

<!-- .slide: id="routing-child-routes-2" -->
## Child Routes (2/2)

- The child route will need the product ID from the parent
- It can access the parent route's parameters using `Router` and `ActivatedRoute`

```ts
@Component({ ... })
export class Overview {
  parentRouteId: number;
  private sub: any;

  constructor(private router: Router,
    private route: ActivatedRoute) {}

  ngOnInit() {
    // Get parent ActivatedRoute of this route.
    this.router.routerState.parent(this.route)
      .params.subscribe(params => {
        this.parentRouteId = +params["id"];
      });
  }
}
```

---

<!-- .slide: id="routing-lazy-loading-1" -->
## Lazy Loading (1/2)

- To take advantage of lazy loading it's important to group the application into modules
- Lazy Loading allows us to load modules of the application on demand
- Because these modules are not loaded during our bootstrap phase, it helps us to decrease the startup time
- On demand modules can be loaded when the user navigates to a specific route
- In order to setup lazy loading we need the following
  - Remove the component from the `declarations` array of the root module
  - In the route config:
    - use `loadChildren` in the path instead of a component
    - define not only the path to the module but the name of the class as well

Here is how our routing should look like:

```ts
const routes: Routes = [
  { path: '', redirectTo: 'eager', pathMatch: 'full' },
  { path: 'eager', component: EagerComponent },
  { path: 'lazy', loadChildren: 'lazy/lazy.module#LazyModule' }
];
```

---

<!-- .slide: id="routing-lazy-loading-2" -->
## Lazy Loading (2/2)

- Routing for a feature module should always call `forChild` instead of `forRoot`
- This is specific to any feature modules and not related to lazy loading

```ts
import { ModuleWithProviders } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { LazyComponent } from './lazy.component';

const routes: Routes = [
  { path: '', component: LazyComponent }
];

export const routing: ModuleWithProviders = RouterModule.forChild(routes);
```

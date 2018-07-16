<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="components" -->
## Building Applications with Angular

# Creating a New Component

---
<!-- .slide: id="components-generating-new-components" -->
## Generating a New Component

- `ng generate component todoList`
  - `ng generate` can be run from any folder within the project
  - WebStorm has a built-in support in Angular CLI
- Creates `src/app/todo-list/*`
```
├── src
│   ├── app
│   │   └── todo-list
│   │       ├── todo-list.component.css
│   │       ├── todo-list.component.html
│   │       ├── todo-list.component.spec.ts
│   │       └── todo-list.component.ts
```

---
<!-- .slide: id="components-conventions" -->
## Conventions

- Component class name is `TodoListComponent`
- HTML selector is `app-todo-list`
  - `app` prefix to prevent namespace collisions
    - Prefix can be customized when creating the application using `--prefix`
    - Or by modifying the `prefix` property in `angular.json`

#### _src/app/todo-list/todo-list.component.ts_
```ts
@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
  styleUrls: ['./todo-list.component.css']
})
export class TodoListComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

---
<!-- .slide: id="components-including" -->

## Including a Component

- Refer to the component by putting its `selector` in another component's template

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<app-todo-list></app-todo-list>
```

![Child Component (Original)](images/todo-list-final.png)

---
<!-- .slide: id="components-passing-data-1" -->
## Passing Data to a Component

- Need to:
  1. Create a variable in the child to receive the value
  1. Use `@Input()` decorator to identify the variable

#### _src/app/to-do-list/todo-list.component.ts_
```ts
@Component({ ... })
export class TodoListComponent implements OnInit {

  @Input() heading: string;

  // ...
}
```

- Reminder: Angular decorators are functions.
Even if you don't pass arguments, you need the parentheses.

---
<!-- .slide: id="components-passing-data-2" -->

## Passing Data to a Component

- And then pass data from the parent

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<app-todo-list [heading]="Things To Do"></app-todo-list>
```

- Whoops: compiler error!

#### _src/app/app.component.html_ (corrected)
```html
<app-todo-list [heading]="'Things To Do'"></app-todo-list>
```

- Must single-quote the string or Angular will think it's a variable.

---
<!-- .slide: id="components-passing-data-3" -->

## Passing Data to a Component

- Angular also has a shorthand for passing string properties to components:

```html
<app-todo-list heading="Things To Do"></app-todo-list>
```

Passing strings to components in this way resembles setting properties in HTML elements.

---
<!-- .slide: id="components-refactoring" -->

## Refactoring

- Pass a variable instead of a constant string
- While we're at it, remove the heading from the child component
- And use a proper list instead of a bunch of paragraphs

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<app-todo-list [thingsToDo]="thingsToDo"></app-to-do-list>
```

- `[thingsToDo]` on the *left* is the input parameter in the child
- `thingsToDo` on the *right* is what we're passing in

---

## Component Lifecycle

Angular manages creation, rendering, data-bound properties etc. It also offers hooks that allow us to respond to key lifecycle events.

These are the most-used lifecycle hooks:

- `ngOnInit` - When bound inputs pass values the first time.
- `ngOnDestroy` - Before component is destroyed.
- `ngAfterContentInit` - After component's (ng-)content is initialized.
- `ngAfterViewInit` - After component's view is initialized.

**Pro Tip:** Prefer putting initialization logic in `ngOnInit` instead of `constructor`

---
<!-- .slide: id="components-get-dirty" -->

## Let's get our hands dirty!

![demo](images/todo-list-final.png)

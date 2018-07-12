<!-- .slide: data-background="../images/title-slide.jpg" -->
<!-- .slide: id="components" -->
## Building Applications with Angular

# Creating a New Component

---
<!-- .slide: id="components-roadmap" -->
## Roadmap

1. How do I create a new component?
1. How does Angular name components?
1. How do I include a component in my display?
1. How do I pass data to a component?

---
<!-- .slide: id="components-generating-new-components" -->
## Generating a New Component

- Use Angular CLI to create a new component to display the list
  - But keep the data in `app.component.ts`
- `ng generate component toDoList`
  - `ng generate` can be run from any folder within the project
- Creates `src/app/to-do-list/*`
```
├── src
│   ├── app
│   │   └── to-do-list
│   │       ├── to-do-list.component.css
│   │       ├── to-do-list.component.html
│   │       ├── to-do-list.component.spec.ts
│   │       └── to-do-list.component.ts
```

---
<!-- .slide: id="components-conventions" -->
## Conventions

- Component class name is `ToDoListComponent`
- HTML selector is `app-to-do-list`
  - `app` prefix to prevent namespace collisions
    - Prefix can be customized when creating the application using `--prefix`
    - Or by modifying the `prefix` property in `.angular-cli.json`
- What to put in constructor and `ngOnInit` will be discussed later

#### _src/app/to-do-list/to-do-list.component.ts_
```
@Component({
  selector: 'app-to-do-list',
  templateUrl: './to-do-list.component.html',
  styleUrls: ['./to-do-list.component.css']
})
export class ToDoListComponent implements OnInit {

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
<p>Summary: {{summary()}}</p>
<p *ngFor="let item of thingsToDo; let i = index" id="{{i}}">({{i}}) {{item}}</p>
<p *ngIf="thingsCompleted.length == 0">Nothing completed</p>
```

![Child Component (Original)](../images/screenshot-child-initial.png)

---
<!-- .slide: id="components-passing-data-1" -->
## Passing Data to a Component

- Need to:
  1. Create a variable in the child to receive the value
  1. Use `@Input()` decorator to identify the variable

#### _src/app/to-do-list/to-do-list.component.ts_
```ts
import { Component, OnInit, Input } from '@angular/core';

@Component({
  // ...as before...
})
export class ToDoListComponent implements OnInit {

  @Input() heading: string;

  // ...as before...
}
```

Reminder: Angular decorators are functions.
Even if you don't pass arguments, you need the parentheses.

---
<!-- .slide: id="components-passing-data-2" -->

## Passing Data to a Component

- And then pass data from the parent
  - Just like a function call

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<p *ngFor="let item of thingsToDo; let i = index" id="{{i}}">({{i}}) {{item}}</p>
<app-to-do-list [heading]="Things To Do"></app-to-do-list>
```

- Whoops: compiler error!

#### _src/app/app.component.html_ (corrected)
```html
<app-to-do-list [heading]="'Things To Do'"></app-to-do-list>
```

- Must single-quote the string or Angular will think it's a function.

---
<!-- .slide: id="components-passing-data-3" -->

## Passing Data to a Component

- Angular also has a shorthand for passing string properties to components:

```html
<app-to-do-list heading="Things To Do"></app-to-do-list>
```

Passing strings to components in this way resembles setting properties in HTML elements.

---
<!-- .slide: id="components-refactoring-1" -->

## Refactoring

- Pass a variable instead of a constant string
- While we're at it, remove the heading from the child component
- And use a proper list instead of a bunch of paragraphs

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<app-to-do-list [thingsToDo]="thingsToDo"></app-to-do-list>
```

- `[thingsToDo]` on the *left* is the input parameter in the child
- `thingsToDo` on the *right* is what we're passing in

---
<!-- .slide: id="components-refactoring-2" -->

## Refactoring

- Move the loop over the list into the child component's HTML

#### _src/app/to-do-list/to-do-list.component.html_
```html
<ul>
  <li *ngFor="let item of thingsToDo; let i = index" id="{{i}}">{{item}}</li>
</ul>
```

- Create the required variable in the child component class

#### _src/app/to-do-list/to-do-list.component.ts_
```ts
export class ToDoListComponent implements OnInit {

  @Input() thingsToDo: string[];

  // ...as before...
}
```

---
<!-- .slide: id="components-refactoring-3" -->

## Refactoring

![After Refactoring](../images/screenshot-refactored.png)

- More elaborate than necessary if we were stopping here...
- ...but absolutely necessary to control complexity in large projects

<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="two-way-data-binding" -->
## Building Applications with Angular

# Two-Way Data Binding

---

<!-- .slide: id="two-way-data-binding-in-angular" -->
## Two-Way Data Binding in Angular?

- Combination of an `@Input` with an `@Output` using the *banana in a box* syntax `[(event)]`
- The name of the event has to be equal to the name of the input plus the suffix "Change".

```html
<app-todo-list [(thingsToDo)]="thingsToDo"></app-todo-list>
```

Is equivalent to:

```html
<app-todo-list [thingsToDo]="thingsToDo" (thingsToDoChange)="thingsToDo=$event"></app-todo-list>
```

---

<!-- .slide: id="two-way-data-binding-ngmodel" -->
## Two-Way Data Binding in Angular?

- The built-in directive `NgModel` uses this trick to behave similar to Angular 1:

```html
<input [(ngModel)]="name" />
```

Equivalent:

```html
<input [ngModel]="name" (ngModelChange)="name=$event" />
```

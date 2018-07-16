<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="display" -->
## Building Applications with Angular

# Displaying Data

---

<!-- .slide: id="display-make-the-data" -->
## Make the Data

- Change the title
- Add a list of strings

#### _src/app/app.component.ts_
```ts
@Component({ ... })
export class AppComponent {
  title = 'To Do';
  thingsToDo = [
    'Learn JavaScript',
    'Learn Angular'
  ];
}
```

---
<!-- .slide: id="display-display-the-data" -->
## Display the Data

- Use `*ngFor` to repeat some HTML multiple times
- The loop variable `item` is created locally

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<p *ngFor="let item of thingsToDo">{{item}}</p>
```

---

<!-- .slide: id="display-ngfor-exports" -->
## `*ngFor` Exports Useful Values

- We can obtain the index of each iteration and assign the value to a local variable:

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<p *ngFor="let item of thingsToDo; let i = index">({{i}}) {{item}}</p>
```

- Similarly, `*ngFor` exports `first`, `last`, `even` and `odd` Booleans which can also be assigned to local variables.

---
<!-- .slide: id="display-ngif-1" -->
## Nothing to See Here, Folks

- Use `*ngIf` to control whether or not something is displayed
  - More precisely, whether or not something is added to the DOM
  - See how to show and hide content with styles later

#### _src/app/app.component.ts_
```ts
export class AppComponent {
  // ...
  thingsCompleted = [];
}
```

---

<!-- .slide: id="display-ngif-2" -->
## Nothing to See Here, Folks

#### _src/app/app.component.html_
```html
<h1>{{title}}</h1>
<p *ngFor="let item of thingsToDo; let i = index" id="{{i}}">({{i}}) {{item}}</p>
<p *ngIf="thingsCompleted.length == 0">Nothing completed</p>
```

---

<!-- .slide: id="display-demo" -->
## Let's add data to our Todo List!

![demo](images/todo-list-final.png)

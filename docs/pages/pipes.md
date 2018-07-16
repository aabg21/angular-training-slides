<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="pipes" -->
## Building Applications with Angular

# Pipes

---
<!-- .slide: id="pipes-motivation" -->
## Why do we need pipes?

- Classes could turn everything into strings for display
- Often easier to use a *pipe* in the HTML
  - Takes a value as input, produces a new value as output
  - Just like a Unix command-line pipe
- Angular comes with several pipes for common operations
- Very easy to add new ones

---
<!-- .slide: id="pipes-using-a-pipe" -->
## Using a Pipe

- Put the name of the pipe inside `{{...}}`
- Use vertical bar `|` as separator

#### _src/app/todo-list/todo-list.component.html_
```html
<ul>
  <li *ngFor="let item of thingsToDo">
    {{item | uppercase}}
  </li>
</ul>
```

---
<!-- .slide: id="pipes-passing-arguments" -->
## Passing Arguments to the Pipe

- Pipes also accept arguments
- Use colon as delimiter
- Chained pipes are evaluated from left to right

```html
Price is {{ 100.12345 | currency:"CAD":true:"1.2" | lowercase }}
```

produces:

```html
Price is ca$100.12
```

---
<!-- .slide: id="pipes-built-in-pipes" -->
## Built-in Angular Pipes

- [date](https://angular.io/docs/ts/latest/api/common/index/DatePipe-pipe.html): formats dates in various ways
- [uppercase](https://angular.io/docs/ts/latest/api/common/index/UpperCasePipe-pipe.html): converts a string to upper case
- [lowercase](https://angular.io/docs/ts/latest/api/common/index/LowerCasePipe-pipe.html): converts a string to lower case
- [currency](https://angular.io/docs/ts/latest/api/common/index/CurrencyPipe-pipe.html): formats a number as a currency
- [percent](https://angular.io/docs/ts/latest/api/common/index/PercentPipe-pipe.html): formats a number as a percentage

---
<!-- .slide: id="pipes-generating-pipes" -->
## Generating a Pipe

- Use `ng generate pipe titlecase`
- Creates `src/app/titlecase.*`
  - Note: in the `src/app` directory
  - We could (and should) create a `pipes` directory

```
├── src
│   ├── app
│   │   ├── titlecase.spec.ts
│   │   └── titlecase.ts
```

---
<!-- .slide: id="pipes-whats-in-a-pipe" -->
## What's in a Pipe?

#### _src/app/titlecase.ts_
```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'titlecase'
})
export class TitlecasePipe implements PipeTransform {

  transform(value: any, arguments?: any): any {
    return null;
  }
}
```

- Import declarations from `@angular/core`
- Decorate class with `@Pipe` using `name` property
- `transform` accepts an initial `value` and optionally some `arguments`
- Returns a transformed value

---

<!-- .slide: id="pipes-pure-and-impure" -->

## Pure vs Impure Pipes

- A pure pipe is executed every time the **reference** of the bound value is changed
  - Custom pipes are pure by default
  - All built-in pipes are pure except of `async`

```ts
@Pipe({ name: 'pure' })
export class PurePipe implements PipeTransform { /* ... */ }
```

- An impure pipe is executed every time change detection is executed
  - App performance could be severely degraded
  - To define a pipe as impure, we need to use the property/value `pure: false`

```ts
@Pipe({ name: 'impure', pure: false })
export class ImpurePipe implements PipeTransform { /* ... */ }
```

---

<!-- .slide: id="pipes-demo" -->

## Let's count some todos!

![demo](images/todo-list-final.png)

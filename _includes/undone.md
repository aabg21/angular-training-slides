<!-- .slide: data-background="../images/title-slide.jpg" -->
<!-- .slide: id="unused" -->
## Building Applications with Angular

# Unused Material

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
## Two-Way Data Binding

- Combination of an `@Input` with an `@Output` using the *banana in a box* syntax `[(event)]`
- The name of the event has to be equal to the name of the input plus the suffix "Change".

```html
<rio-counter [count]="count" (countChange)="count=$event"></rio-counter>
```

Is equivalent to:

```html
<rio-counter [(count)]="count"></rio-counter>
```

The built-in directive `NgModel` uses this trick to behave similar to Angular 1:

```html
<input [(ngModel)]="name" />
```

Which is equivalent to:

```html
<input [ngModel]="name" (ngModelChange)="name=$event" />
```

---

## Two-Way Data Binding Example

```ts
@Component({ selector: 'rio-counter',  ... })
export class CounterComponent {
  @Input() count: number;
  @Output() countChange = new EventEmitter<number>();

  increment(): void {
    this.count++;
    this.countChange.emit(this.count);
  }
}
```

The parent component can use now the *banana in a box* syntax

```ts
@Component({
  selector: 'rio-app',
  template:'<rio-counter [(count)]="myNumber"></rio-counter>'
})
class AppComponent { myNumber = 0 }
```

[View Example](http://plnkr.co/edit/nJZQYSV23sCcbb37FzLN?p=preview)

---

## Creating a Template Variable from a Directive

- We can create template variables from directives using the `#myVariable="exportedAsValue"` syntax
- `exportedAs` value is a property that every built-in directive has to create a reference
- The value of the property `exportedAs` of the directive `NgForm` is `ngForm` ([docs](https://angular.io/docs/ts/latest/api/forms/index/NgForm-directive.html))

```ts
@Component({
  selector: 'rio-app',
  template: `
    <form #myForm="ngForm" (ngSubmit)="submitForm(myForm)">
      <label>Name: <input name="name" ngModel></label>
      <button type="submit">Submit</button>
    </form>
    <pre>{{ values | json }}</pre>`
})
export class AppComponent {
  values: any;
  submitForm (form: NgForm): void {
    this.values = form.value;
  }
}
```

[View Example](https://plnkr.co/edit/ttVaCf?p=preview)

Notes:

- The value of the property `exportedAs` of the directive `NgForm` is `ngForm`
- The enhanced component instance has validation, the native `form` component doesn't

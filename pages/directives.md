<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="directives" -->
## Building Applications with Angular

# Directives

---
<!-- .slide: id="directives-what-are-directives" -->
## What are directives?

- Directives are entities that **change the behavior** of components
- Components are just directives with templates
- Directives can be applied to native elements or custom components

There are two types of directives:

- **Attribute directives:** Changes behavior without modifying the template (`NgClass`, `NgStyle`)
- **Structural directives:** Changes behavior by modifying the template (`NgIf`, `NgFor`)

---
<!-- .slide: id="directives-ngstyle-1" -->
## `NgStyle` Directive (1/2)

- Directive that modifies the `style` attribute of a component
- The `NgClass` directive can be used with objects and attributes.

```ts
@Component({
  selector: 'style-example',
  template: `
    <p style="padding: 1rem"
      [ngStyle]="{
        color: 'red',
        'font-weight': 'bold',
        borderBottom: borderStyle
    }">
    </p>
  `
})
export class StyleExampleComponent {
  borderStyle = '1px solid black';
}
```

---
<!-- .slide: id="directives-ngstyle-2" -->
## `NgStyle` Directive (2/2)

Used with attribute:

```ts
@Component({
  selector: 'style-example',
  template: `
    <p style="padding: 1rem"
      [style.color]="'red'"
      [style.font-weight]="'bold'"
      [style.borderBottom]="borderStyle">
    </p>
  `
})
export class StyleExampleComponent {
  borderStyle = '1px solid black';
}
```

---

<!-- .slide: id="directives-ngclass-1" -->
## `NgClass` Directive (1/5)

- Changes the `class` attribute of the host component
- The `NgClass` directive can be used with strings, arrays, objects and attributes.

```ts
@Component({
  selector: 'class-as-string',
  template: '<p ngClass="centered-text underlined" class="orange"></p>',
})
export class ClassAsStringComponent {}
```

Resulting class attribute:

```html
<p class="orange centered-text underlined"></p>
```

---

<!-- .slide: id="directives-ngclass-2" -->
## `NgClass` Directive (2/5) - String

Used with a string:

```ts
@Component({
  selector: 'class-as-string',
  template: '<p ngClass="centered-text underlined" class="orange"></p>',
})
export class ClassAsStringComponent {}
```

Or using a string property

```ts
@Component({
  selector: 'class-as-string',
  template: '<p [ngClass]="classes" class="orange"></p>',
})
export class ClassAsStringComponent {
  classes = 'centered-text underlined';
}
```

---
<!-- .slide: id="directives-ngclass-3" -->
## `NgClass` Directive (3/5) - Array

Used with an array:

```ts
@Component({
  selector: 'class-as-string',
  template: '<p [ngClass]="['centered-text', 'underlined']" class="orange"></p>',
})
export class ClassAsStringComponent {}
```

Or using an array property

```ts
@Component({
  selector: 'class-as-string',
  template: '<p [ngClass]="classes" class="orange"></p>',
})
export class ClassAsStringComponent {
  classes = ['centered-text', 'underlined'];
}
```

---
<!-- .slide: id="directives-ngclass-4" -->
## `NgClass` Directive (4/5) - Object

```ts
@Component({
  selector: 'class-as-string',
  template: `
    <p
      [ngClass]="{'centered-text': isCentered, underlined: isUnderlined}"
      class="orange">
    </p>`,
  styles: [ ... ]
})
export class ClassAsStringComponent {
  isCentered = true;
  isUnderlined = false;
}
```

Result:

```html
<p class="orange centered-text"></p>
```

---
<!-- .slide: id="directives-ngclass-5" -->
## `NgClass` Directive (5/5) - Attribute

```ts
@Component({
  selector: 'class-as-string',
  template: `
    <p
      [class.centered-text]="isCentered"
      [class.underlined]="isUnderlined"
      class="orange">
    </p>`,
  styles: [ ... ]
})
export class ClassAsStringComponent {
  isCentered = true;
  isUnderlined = false;
}
```

Result:

```html
<p class="orange centered-text"></p>
```

---

<!-- .slide: id="directives-structural-directives" -->
## Structural Directives

- Handles how a component or native element renders using the `<ng-template>` tag
- Have their own special syntax in the template `*myDirective`
- Built-in structural directives
  - `ngIf` and `ngFor`: seen before
  - `ngSwitch` discussed below

---
<!-- .slide: id="directives-ngswitch" -->
## `NgSwitch` Directive

- Multiple components can be matched using `ngSwitchCase`
- Since components are created or destroyed be aware of the costs in doing so.

```html
<div class="tabs-selection">
  <tab [active]="isSelected(1)" (click)="setTab(1)">Tab 1</tab>
  <tab [active]="isSelected(2)" (click)="setTab(2)">Tab 2</tab>
</div>
<div [ngSwitch]="tab">
  <tab-content *ngSwitchCase="1">Tab content 1</tab-content>
  <tab-content *ngSwitchCase="2">Tab content 2</tab-content>
  <tab-content *ngSwitchDefault>Select a tab</tab-content>
</div>
```

```ts
@Component({ ... })
export class AppComponent {
  tab = 0;
  setTab(num: number): void { this.tab = num; }
  isSelected(num: number): boolean { return this.tab === num; }
}
```

---

<!-- .slide: id="directives-multiple-structural-directives" -->
## Using Multiple Structural Directives

A component or native element can only be bound to one directive at a time

```html
<!-- Not allowed -->
<div *ngFor="[1,2,3,4,5,6]" *ngIf="item > 3">
  {{item}}
</div>
```

We need to use the alternative syntax using the `<ng-template>` tag

```html
<!-- Allowed -->
<ng-template ngFor [ngForOf]="[1,2,3,4,5,6]" let-item>
  <div *ngIf="item > 3">
    {{item}}
  </div>
</ng-template>
```

---
<!-- .slide: id="directives-demo" -->

## Let's mark those todos!

![demo](images/todo-list-marked.png)

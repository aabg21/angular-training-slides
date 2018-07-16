<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="instructor-guide" -->
## Building Applications with Angular

# QUIZ TIME!

---

<!-- .slide: id="hello-quiz" -->
<!-- .slide: data-background="images/question-slide.jpg" -->

## Quiz

The name for the root module should be `_________`:

1. `NgModule`
1. `AppModule`
1. Whatever you want.

+++

<!-- .slide: data-background="images/answer-slide.jpg" -->

## Answer

Correct answer is 2.

1: `NgModule` is the name of the Decorator used for any Angular
module.  One can easily mistake between `NgModule` and `AppModule`.

3: While it's possible that the root module can be anything,
convention is to name it `AppModule`.  This could be a good chance to
bring it up that it's convention.

---

<!-- .slide: id="quiz-active-users" -->
<!-- .slide: data-background="images/question-slide.jpg" -->

## Question

What would you rather choose?

1. `[users]="getActiveUsers()"`
1. `[users]="users | activeUsers"`

+++

<!-- .slide: data-background="images/answer-slide.jpg" -->

## Answer

Correct answer is 2.

1: `getActiveUsers()` is called every CD.

2: The pipe `activeUsers` is called only when `users` change.

---

<!-- .slide: id="quiz-time-banana" -->
<!-- .slide: data-background="images/question-slide.jpg" -->

## Quiz

What needs to be added to the following component declaration in order
for this markup to be valid `<clock [(time)]="currentTime"></clock>`:

```ts
@Component({
  selector: 'clock',
  template: `...`
})
export class ClockComponent {
  @Input() time;
}
```

1. `@Output() time = new EventEmitter();`
2. `@Output() timeChange;`
3. `@Output() timeChange = new EventEmitter();`
4. `@Output() onChange = new EventEmitter();`

+++

<!-- .slide: data-background="images/answer-slide.jpg" -->

## Answer

Correct answer is 3.

1: This may expose a misunderstanding of class member declarations in TypeScript.

2: This answer may expose a misunderstanding between the implicit
behavior of the `@Input()` decorator and having to explicitly
construct and assign an `EventEmitter` to a member decorated with
`@Output()`. It may be assumed that by decorating something as an
output you should achieve the expected behavior.

3: This may expose confusion between the relationship of the property
names and that suffixing an output member name with `Change`.

---

<!-- .slide: id="quiz-component-in-module" -->
<!-- .slide: data-background="images/question-slide.jpg" -->

## Quiz

If I want to write a custom UI Component for my project,
which field of the `@NgModule` decorator should I put the class in?

1.  `bootstrap`
1.  `declarations`
1.  `interfaces`
1.  None of the above.

+++
<!-- .slide: data-background="images/answer-slide.jpg" -->

## Answer

Correct answer is `declarations`.

---
<!-- .slide: id="quiz-events" -->
<!-- .slide: data-background="images/question-slide.jpg" -->

## Quiz

What is the difference between:

(A) `<counter [count]="count()"></counter>`

(B) `<counter (count)="count()"></counter>`

+++
<!-- .slide: data-background="images/answer-slide.jpg" -->

## Answer

`<counter [count]="count()"></counter>`
- passes the result of calling `this.count()` into the `<counter>` component via an input.

`<counter (count)="count()"></counter>`
- calls `this.count()` whenever the `<counter>` component emits a value through its own `EventEmitter` member `count`.

---
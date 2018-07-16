<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="cd" -->
## Building Applications with Angular

# Change Detection

---

<!-- .slide: id="cd-what-causes" -->
## What causes change detection?

- **Events**: `click`, `submit`, ...
- **XHR**: `fetch`, `XMLHttpRequest`, ...
- **Timers**: `setTimeout`, `requestAnimationFrame`, ...

###

- Do they have something in common?
  - Yes, they are all async.

---

<!-- .slide: id="cd-who-notifies" -->
## Who notifies Angular?

- **Zones**
  - "A Zone is an execution context that persists across async tasks"
  - Is on [stage 0 of tc39](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md) to enter ECMAScript
- Angular has its own Zone - NgZone

```ts
this.zone.onMicrotaskEmpty
  .subscribe(() => {
    this.zone.run(() => this.tick() })
  })
```
```ts
tick() {
  this.changeDetectorsRefs
    .forEach((ref) => ref.detectChanges())
}
```

---

<!-- .slide: id="cd-components-tree" -->
## Each component has its own Change Detector

![Change Detector of components](images/component-cd-tree.png)

---

<!-- .slide: id="cd-components-tree-flow" -->
## Change Detection will always begin from root

![Change Detector of components flow](images/component-cd-tree-flow.png)

---

<!-- .slide: id="cd-change-new-ref" -->
## Change = new reference

```ts
let user = {
  name: 'Amir Bashan',
  work: 'XtremIO'
};
```

- Mutating objects doesn't alert change

```ts
user.work = 'ScaleIO';
```

- But, immutables do

```ts
user = {
  name: 'Amir Bashan',
  work: 'ScaleIO'
};
```

---

<!-- .slide: id="cd-smarter-cd" -->
## Smarter Change Detection

- Angular is conservative by default and checks every component every single time
- It can skip entire Change Detection subtrees when inputs properties don't change

```ts
@Component({
  // ...
  changeDetection: ChangeDetectionStrategy.OnPush
})
class VCardCmp {
  @Input() data;
}
```

- Will perform change detection on view only if input has changed

---

<!-- .slide: id="cd-components-tree-onpush" -->
## Change Detection with OnPush strategy

![Change Detector of components with OnPush](images/component-cd-tree-onpush.png)

---

<!-- .slide: id="cd-even-smarter" -->
## Even smarter Change Detection

```ts
@Component({ ... })
class OwnCDImpl {

  @Input() stream: Observable<any>;

  constructor(private cd: ChangeDetectorRef) {}

  ngOnInit() {
    this.stream.subscribe(() => {
      // ...
      this.cd.markForCheck(); // marks path
    })
  }
}
```

---

<!-- .slide: id="cd-detach" -->
## Don't need it? You don't have to

- Components could be static
  - We should stop their Change Detection

```ts
this.cd.detach();
```

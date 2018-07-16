<!-- .slide: data-background="images/title-slide.jpg" -->
<!-- .slide: id="setup-into" -->
## Building Applications with Angular

# Setup

---
<!-- .slide: id="setup-installing-angular-cli" -->
## Installing Angular CLI

- A command-line interface (CLI) for creating and managing Angular applications
- `npm install --global @angular/cli`

---
<!-- .slide: id="setup-creating-a-new-project" -->
## Creating a New Project

`ng new todo`

This will:

- Create a new directory called `todo`
- Install the core Angular packages
- Install their dependencies
- Set up unit testing

This may take a few minutes to run the first time...

---
<!-- .slide: id="setup-top-level-project-structure" -->
## Top-Level Project Structure

```
├── angular.json
├── package.json
├── tsconfig.json
├── tslint.json
├── node_modules/
├── src/
```

- `angular.json`: Angular CLI configuration
- `package.json`: project metadata
  - Modules needed to run application
  - Custom commands
  - Author, version, and license information
- `tsconfig.json`: TypeScript configuration
- `tslint.json`: TypeScript lint sets of rules
- `node_modules`: directory containing libraries and dependencies used by the application
  - *Do not edit by hand*

---
<!-- .slide: id="whats-where-src" -->
## What's Where

```
├── src
│   ├── index.html
│   ├── main.ts
│   ├── styles.css
│   ├── tslint.json
│   ├── assets/...
│   ├── environments/...
│   ├── app/...
```

The `src` folder has:

- `index.html`: contains the entire application
  - Everything interesting right now is one level down in `app`
- `main.ts`: bootstraps the application
  - This can change from platform to platform
  - So that everything else doesn't have to
- `styles.css`: global CSS styles
- `assets/`: images, fonts, ...
- `environments/`: dev/prod environments constants

---
<!-- .slide: id="whats-where-app" -->
## What's Where

```
├── src
│   ├── app
│   │   ├── app.component.css
│   │   ├── app.component.html
│   │   ├── app.component.spec.ts
│   │   ├── app.component.ts
│   │   ├── app.module.ts
```

The *application* as a whole has:

- `app.module.ts`: what to load and how to launch
- Main application:
  - Code: `app.component.ts` (root element)
    - Modules, Components, Directives, Pipes, Services, ...
  - HTML: `app.component.html`
  - CSS: `app.component.css`
  - Test spec: `app.component.spec.ts`

---

<!-- .slide: id="angular-module-explained" -->
## Angular module explained

- `NgModule`: Packages of views and logic:
  - `Component`: Widgets, visual elements
  - `Directive`: Properties that add behaviors to elements
  - `Pipe`: Helps to show information
  - `Service`: Logical behaviors

- *Everything is in plain JavaScript*
- *Everything is a class in Angular*

---

<!-- .slide: id="whats-in-ngmodule" -->
## What's in NgModule?

#### _src/app/app.module.ts_
```ts
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})

```
- `@NgModule` is a **decorator** used to define Angular modules
- `declerations`: Set of components, directives and pipes that belong to this module
- `imports`: Set of modules, whose exported declarations are available to this module
- `exports`: Set of declarations and modules that can be used within another module
- `providers`: Set of injectables that are available in the injector of this module
- `bootstrap`: Set of components that are bootstrapped when the module is bootstrapped
- `entryComponents`: Set of components to compile when this module is defined

---

<!-- .slide: id="whats-in-the-component" -->
## What's in the Component?

#### _src/app/app.component.ts_
```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
```

- `@Component` is a **decorator** used to define Angular components
- `selector`: Tells Angular to use this class to fill in uses of `<app-root></app-root>`
- `template`: Inline HTML to use for filling in
- `templateUrl`: Tells Angular where to find the HTML to use for filling in
- `styles`: List of inline CSS stylesheets to apply to this component
- `styleUrls`: List of CSS style files to apply to this component

---
<!-- .slide: id="component-template-syntax" -->
## Component Template Syntax

```ts
@Component({
  selector: 'app-root',
  template: '<h1>{{title}}</h1>'
})
export class AppComponent {
  title = 'app works!';
}
```
- `<h1>{{title}}</h1>` represents *interpolation*
- Evaluates the expression in `{{...}}` and inserts the result as a string
- Symbols such as `title` are evaluated in an expression context
- In this case, the component instance

---

<!-- .slide: id="hello-running-the-application" -->
## Running the Application

- `ng serve` launches the application
- Should see a welcome page

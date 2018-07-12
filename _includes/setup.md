<!-- .slide: data-background="../images/title-slide.jpg" -->
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
├── node_modules/
├── src/
```

- `angular.json`: Angular CLI configuration
- `package.json`: project metadata
  - Modules needed to run application
  - Custom commands
  - Author, version, and license information
- `tsconfig.json`: TypeScript configuration
- `node_modules`: directory containing libraries and dependencies used by the application
  - *Do not edit by hand*
- `src` folder: application code

---
<!-- .slide: id="hello-whats-where-1" -->
## What's Where

```
├── src
│   ├── index.html
│   ├── main.ts
│   ├── app/...
```

The `src` folder has:

- `index.html`: contains the entire application
  - Everything interesting right now is one level down in `app`
- `main.ts`: bootstraps the application
  - This can change from platform to platform
  - So that everything else doesn't have to

---
<!-- .slide: id="hello-whats-where-2" -->
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
  - Code: `app.component.ts`
  - HTML: `app.component.html`
  - CSS: `app.component.css`
  - Test spec: `app.component.spec.ts`

---
<!-- .slide: id="hello-running-the-application" -->
## Running the Application

- `ng serve` launches the application
- Should see a welcome page
- Worth exploring the page using developer tools

---
<!-- .slide: id="hello-whats-in-the-component-1" -->
## What's in the Component?

#### _src/app/app.component.ts_
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```

- `Component` is a **decorator** used to define Angular components
- Decorators allows us to alter a class or function
  - In this case, add metadata for Angular to use

---
<!-- .slide: id="hello-whats-in-the-component-2" -->
## What's in the Component?

#### _src/app/app.component.ts_
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```

- `@Component` is a *decorator* that adds metadata to the `AppComponent` class
  - `selector` tells Angular to use this class to fill in uses of `<app-root></app-root>`
  - `templateUrl` tells it where to find the HTML to use for filling in
  - `styleUrls` is a list of CSS style files to apply to this component

---
<!-- .slide: id="hello-whats-in-the-component-3" -->
## What's in the Component?

#### _src/app/app.component.ts_
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```

- The `AppComponent` class defines how this component behaves
- So far, it just has a single instance variable and no actions
- Note that we don't need to declare the type of `title`
  - TypeScript does *type inference* to figure that out

---
<!-- .slide: id="hello-notes-on-components" -->
## Notes on Components

- Components are the core building blocks of Angular applications
  - Application logic + display
  - Everything on a page should be associated with some component
- Can put HTML and CSS inline using `template` and `style`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: '<h1>{{title}}</h1>',          // inline HTML
  styles: ['h1 { font-weight: normal; }']  // inline styles
})
export class AppComponent {
  title = 'app works!';
}
```

---
<!-- .slide: id="hello-template-syntax" -->
## Component Template Syntax

- `<h1>{{title}}</h1>` represents *interpolation*
- Evaluates the expression in `{{...}}` and inserts the result as a string
- Symbols such as `title` are evaluated in an expression context
- In this case, the component instance
- We'll see lots more syntax later...

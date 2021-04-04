## Creating Component Libraries

Now, we are going to create two separate projects with the `header` and `sidebar` components.
That should be more than enough to demonstrate publishing and consuming multiple component libraries.

In the root project folder, run the following commands to create an initial folder structure.

```sh
mkdir -p modules/header/src
mkdir -p modules/sidebar/src
```

As you can see, the `modules` folder contains all the component projects as subfolders.

Let's start with the `header` component first, and you can then perform similar steps to implement the `sidebar`.

First, create a component class and give it a unique selector to avoid clashes at runtime.
The value can be anything of your choice, for this guide we are going to use `ngfw-header`.

```ts
// modules/header/src/header.component.ts

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'ngfw-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent {}
```

The template is going to be very simple. An `h1` element wrapping the `ng-content` is the minimal implementation we need right now.
You can always extend it later with the more sophisticated layout and behavior.

```html
<!-- modules/header/src/header.component.html -->

<h1>
  <ng-content></ng-content>
</h1>
```

Add some CSS to be sure the styles are also loaded from the package when we start using it with the application.
In our case, the colour of the header is going to be red.

```css
/* modules/header/src/header.component.css */

h1 {
  color: red;
}
```

Finally, as per Angular architecture, we need to have a module
that imports all necessary dependencies and provides declarations and exports.

The most important part of our scenario is to put the `HeaderComponent` to the `exports` collection.

```ts
// modules/header/src/header.module.ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HeaderComponent } from './header.component';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: [
    HeaderComponent
  ],
  exports: [
    HeaderComponent
  ]
})
export class HeaderModule { }
```

As the `header` component it planned to be published as a separate library, we need to provide a `package.json` file.

Below is a simple set of details that needs to be in `package.json` for a library before publishing.
Note that you can also use NPM scopes to have unique library names associated with your NPM account for instance.

**modules/header/package.json**:

```json
{
  "name": "@denysvuika/ng-framework-header",
  "version": "1.0.0",
  "author": "Denys Vuika <denys.vuika@gmail.com>",
  "license": "MIT",
  "private": false,
  "peerDependencies": {
    "@angular/common": "^5.0.0",
    "@angular/core": "^5.0.0"
  }
}
```

```ts
// modules/header/public_api.ts

export * from './src/header.module';
```

Now you can repeat the steps above to implement the second `sidebar` component.
The only difference is the name of the project in the `package.json` file.

**modules/sidebar/package.json**:

```json
{
  "name": "@denysvuika/ng-framework-sidebar",
}
```

Also, let's also change the colour to `green`, just to see we are dealing with two different stylesheets at runtime.

```css
/* modules/sidebar/src/sidebar.component.css */

h1 {
  color: green;
}
```

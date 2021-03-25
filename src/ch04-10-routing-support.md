## Routing Support

If you plan working with Angular Router or want to experiment with routing capabilities,
the Angular CLI can generate an application for you with initial Router support.

Use the "--routing" switch if you want to generate a routing module scaffold with your application.

```sh
ng new my-app --routing
```

The routing scaffold should look similar to the one below:

```ts
// src/app/app-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

In addition, the main application component is going to contain the router outlet component:

```html
<!-- src/app/app.component.html -->

<router-outlet></router-outlet>
```

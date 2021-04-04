## Services

### Creating LogService

As an example, let's build a shared Logger service now.

Logging is an excellent example of how to turn a frequently used functionality into an injectable service.
That is something you are not going to re-implement in each component.

You can save your time by using Angular CLI to generate a new "log" service utilizing the following command:

```sh
ng g service services/log
```

That should give you a scaffold for a new service called `LogService`:

```ts
// src/app/services/log.service.ts

import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class LogService {

  constructor() { }

}
```

We use a special `@Injectable` decorator here to mark the class and instruct Angular
that the given class should participate in the dependency injection layer.

All classes marked with `@Injectable` can get imported into other entities like services, components, directives or pipes.
The Angular framework creates instances of those classes, usually in the form of "singletons",
and injects into other primitives on demand.

Note the warning message that Angular CLI generates for every new service scaffold:

```text
installing service
  create src/app/services/log.service.spec.ts
  create src/app/services/log.service.ts
  WARNING Service is generated but not provided, it must be provided to be used
```

We are going to walk through Modules feature later in this chapter.
For now, just edit the `app.module.ts` file and add the `LogService` file to the `providers` section as in the following example:

```ts
// src/app/app.module.ts
// ...

import { LogService } from './services/log.service';

@NgModule({
  declarations: [/* ... */],
  imports: [/* ... */],
  providers: [
    LogService
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

That allows injecting `LogService` into all application components,
including the `component1` and `component2` we have created earlier.

Next, let's extend the service implementation with an `info` method we can reuse across the components:

```ts
// src/app/services/log.service.ts

@Injectable({ providedIn: 'root' })
export class LogService {

  constructor() { }

  info(message: string) {
    console.log(`[info] ${message}`);
  }

}
```

At this point, we got a minimal logging service implementation that we can use in our components.

### Injecting and Using LogService

We are going to use constructor parameters to inject the `LogService` created earlier.

The Angular framework takes care of all the intermediate steps needed to find the expected type of the service,
instantiate it and provide as a parameter when building our component.

```ts
// src/app/component1/component1.component.ts

import { Component } from '@angular/core';
import { LogService } from './../services/log.service';

@Component({/*...*/})
export class Component1Component {

  constructor(logService: LogService) {
    logService.info('Component 1 created');
  }

}
```

You can now try to update the second component implementation yourself
and add the same LogService integration as in the example above.

Once you are finished updating the code, run the application,
and you should see the following output in the browser console:

```text
[info] Component 1 created
[info] Component 2 created
```

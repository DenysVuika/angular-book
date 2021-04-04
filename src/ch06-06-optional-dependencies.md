## Optional Dependencies

Previously, we have successfully created a `CompositeLogService` service
based on multiple plugins injected with the same custom token.

However, what happens when there are no logger plugins registered within the application?
Let's comment out the plugin registration section in the app module providers to see what happens at runtime.

```ts
@NgModule({
  providers: [
    CompositeLogService // ,
    // { provide: LOGGER_PLUGIN, useClass: ErrorLogPlugin, multi: true },
    // { provide: LOGGER_PLUGIN, useClass: WarningLogPlugin, multi: true }
  ]
})
```

Now if you rebuild and run your application it is going to crash with the following error in the browser console:

```text
Error: No provider for InjectionToken logger.plugin!
```

The error above is an expected behavior.

Essentially, when you declare a dependency within the constructor parameters,
you instruct the Angular framework to ensure the corresponding dependency indeed exists, and injection can happen.

There are scenarios when having an instance is not mandatory, and application can function without it,
like in our case with logger plugins - the log service can have default fallback behaviour in case no plugins are present.

For those kinds of scenarios, the Angular framework provides us with the "@Optional" decorator
that allows making particular injections "optional" rather than "mandatory" during dependency injection phase.

```ts
import { /*...,*/ Optional } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class CompositeLogService {

  constructor((@Optional() @Inject(LOGGER_PLUGIN) private plugins: LogPlugin[]) {
    if (plugins && plugins.length > 0) {
      // ...
    } else {
      console.log('No logger plugins found.');
    }
  }

 // ...
}
```

As you can see from the code above, we now have a possibility to check whether any plugins are available,
and perform the fallback behavior if needed.
Just for the demo purposes, we log the information message to the console:

```text
No logger plugins found.
```

The `@Optional` decorator is a handy mechanism that helps you to prevent the runtime errors
when some content is missing or is not registered correctly.

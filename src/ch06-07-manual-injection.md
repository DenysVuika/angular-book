## Manual Injection

You are not limited to automatic dependency injection mechanism.
The Angular provides you with a low-level APIs that allow you to resolve and create instances manually from code.

The DI utility class is called `ReflectiveInjector`, and you can import it from the `@angular/core` namespace.
It helps to create the `injectors` filled with resolved and created instances of the providers,
similar to those we used with the application modules.

```ts
import { ReflectiveInjector } from '@angular/core';

@Component({/*...*/})
export class AppComponent {
  
  constructor() {
    const injector = ReflectiveInjector.resolveAndCreate([ LogService ]);
    const logService: LogService = injector.get(LogService);
    logService.info('hello world');
  }

}
```

Typically you are going to use this API only for concrete scenarios like unit testing or dynamic content creation.

> You can get more detailed information including code examples in the following article: [Reflective Injector](https://angular.io/api/core/ReflectiveInjector).

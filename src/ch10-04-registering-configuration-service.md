## Registering Configuration Service

Next, we need registering our configuration service with the main application module.
However, given that many other services and components may depend on the external settings,
we should ensure the application configuration service gets the `app.config.json` file loaded and applied before others.

The Angular framework provides a special injection token called `APP_INITIALIZER`
that you should use to execute your code during the application startup.
That also helps to register providers and to use provider factories.

For the sake of simplicity the example below shows only the newly added content:

```ts
// src/app/app.module.ts

import { /*...,*/ APP_INITIALIZER } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

export function setupAppConfigServiceFactory(
  service: AppConfigService
): Function {
  return () => service.load();
}

@NgModule({
  imports: [
    // ...,
    HttpClientModule
  ],
  providers: [
    {
        provide: APP_INITIALIZER,
        useFactory: setupAppConfigServiceFactory,
        deps: [
            AppConfigService
        ],
        multi: true
    }
  ]
})
export class AppModule { }
```

So how the code above works and how do we load an external configuration at the application startup?

First of all, we declare the `AppConfigService` in the `providers` section of the module.
Then we use the `APP_INITIALIZER` injection token to declare a custom provider based on the `setupAppConfigServiceFactory` factory;
the provider also references the `AppConfigService` as a dependency to inject when the factory gets invoked.

At the runtime, the Angular resolves and creates an instance of the `AppConfigService`,
and then uses it when calling the `setupAppConfigServiceFactory`.
The factory itself calls the "load" method to fetch the "app.config.json" file.

If you run the web application right now the browser console output should be as following:

```text
using server-side configuration
```

The settings service is up and running correctly, and we are now able to use configuration settings in the components.

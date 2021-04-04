## Injection Tokens

As you might have already understood the Angular dependency injection layer keeps a map of providers that are being identified by "keys",
also known as "injection tokens", and uses this map to resolve, create and inject instances at runtime.

The injection tokens can be of different types. We have already tried Types and Strings in action in previous sections.

### Type Tokens

Type-based injection tokens are the most commonly used way to register providers.
Typically you import the service type from the corresponding file and put it into the `providers` section of the module.

```ts
import { LogService } from './services/log.service';

@NgModule({
  // ...
  
  providers: [
    LogService
  ],
  
  // ...
})
export class AppModule { }
```

The same applies to custom provider registration options we tried earlier:

```ts
providers: [
  { provide: LogService, useClass: LogService },
  { provide: LogService, useFactory: customLogServiceFactory },
  { provide: LogService, useValue: logService },
  { provide: AuthenticationService, useExisting: SafeAuthenticationService }
]
```

In all the cases above we use a real Type reference to register a new provider.

### String Tokens

Another way to register a provider involves the string-based injection tokens.
Typically you are going to use strings when there is no Type reference available,
for example when registering plain values or objects:

```ts
providers: [
  { provide: 'DATE_NOW', useFactory: dateFactory },
  { provide: 'APP_VERSION', useValue: '1.1.0' },
  {
    provide: 'logger.config',
    useValue: {
      logLevel: 'info',
      prefix: 'my-logger'
    }
  }
]
```

### Generic InjectionToken

Also, Angular provides a special generic class `InjectionToken<T>` to help you create custom injection tokens
backed by specific types: primitives, classes or interfaces.
That enables static type checks and prevents many type-related errors at early stages.

Let's create separate file `tokens.ts` to hold our custom injection tokens, and create a simple string-based one:

```ts
import { InjectionToken } from '@angular/core';

export const REST_API_URL = new InjectionToken<string>('rest.api.url');
```

Now we can use this token within the main application module to register a URL value
that all components and services can use when needed:

```ts
import { REST_API_URL } from './tokens';

@NgModule({
  // ...,

  providers: [
    // ...,
    
    { provide: REST_API_URL, useValue: 'http://localhost:4200/api' }
  ]
})
```

From this moment we can use the same token to import registered value in the service or a component like in the example below:

```ts
// ...

import { REST_API_URL } from './../tokens';

@Injectable({ providedIn: 'root' })
export class LogService {

  constructor(@Inject(REST_API_URL) restApiUrl: string) {
    console.log(restApiUrl);
  }

  // ...
}
```

At runtime, you should see the actual value of the `REST_API_URL` provider in the browser console: `http://localhost:4200/api`.

As mentioned earlier, you can also use interfaces or classes with the `InjectionToken<T>`.
That does not affect the process of dependency injection but gives you an opportunity for static compile-time checks
and auto completion if your code editor supports TypeScript.

Let's create a token for the `LoggerConfig` interface we set up in this chapter earlier:

```ts
export interface LoggerConfig {
  logLevel?: string;
  prefix?: string;
}

export const LOGGER_CONFIG = new InjectionToken<LoggerConfig>('logger.config');
```

You can now define a type-safe configuration object and register it with the dependency injection system using main application module:

```ts
const loggerConfig: LoggerConfig = {
  logLevel: 'warn',
  prefix: 'warning:'
};

@NgModule({
  // ...,
  
  providers: [
    // ...,
    
    { provide: LOGGER_CONFIG, useValue: loggerConfig }
  ]
})
```

Finally, you can use that token to inject configuration into the LogService and use it to setup the service accordingly:

```ts
@Injectable({ providedIn: 'root' })
export class LogService {

  constructor(@Inject(LOGGER_CONFIG) config: LoggerConfig) {
    console.log(config);
  }

  // ...
}
```

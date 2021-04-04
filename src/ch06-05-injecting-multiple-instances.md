## Injecting Multiple Instances

Previously we have been working with injectables backed by the singletons.
That means any components, directives or other services are typically referencing the same instance created only once on the very first request.

There are cases, however, when you may want to have multiple service instances injected at runtime utilizing a single injection token.

An excellent example is plugin systems and plugins support.
Typically you are going to require a special contract or interface that every external plugin must implement.
The service, component or an application layer need to rely on only the common and shared API,
and it makes sense injecting an entire collection of the plugin instances without knowing exact types.

Let's build a logging service that supports external plugins and injects them as a single collection.

First, create a `LogPlugin` interface for external plugin implementation,
and a basic `CompositeLogService` scaffold for our service.

We are going to get back to it shortly.

```ts
// src/app/services/composite-log.service.ts

export interface LogPlugin {
  name: string;
  level: string;
  log(message: string);
}

@Injectable({ providedIn: 'root' })
export class CompositeLogService {

  constructor() { }

}
```

The `LogPlugin` interface contains a bare minimum of APIs, at this point we need a `name` for demo and debugging purposes,
alongside the level of the messages our plugin supports and the method to write a log message.

Next, create an injection token `LOGGER_PLUGIN` backed by the interface we have just created above.

```ts
// src/app/tokens.ts

import { LogPlugin } from './services/composite-log.service';

export const LOGGER_PLUGIN = new InjectionToken<LogPlugin>('logger.plugin');
```

We are going to use that token to register various logger plugins, and also inject existing plugin instances for the `CompositeLogService`.

After that let's create a couple of Loggers that implement the `LogPlugin` interface.
There is going to be one class for error messages and one for warnings.

```ts
import { LogPlugin } from './composite-log.service';

export class ErrorLogPlugin implements LogPlugin {

    name = 'Error Log Plugin';
    level = 'error';

    log(message: string) {
      console.error(message);
    }
}

export class WarningLogPlugin implements LogPlugin {

  name = 'Warning Log Plugin';
  level = 'warn';

  log(message: string) {
    console.warn(message);
  }
}
```

Now you are ready to register the service and its plugins with the main application module like in the following example:

```ts
// src/app/app.module.ts

import { LOGGER_PLUGIN } from './tokens';
import { ErrorLogPlugin, WarningLogPlugin } from './services/loggers';


@NgModule({
  providers: [
    CompositeLogService,
    { provide: LOGGER_PLUGIN, useClass: ErrorLogPlugin, multi: true },
    { provide: LOGGER_PLUGIN, useClass: WarningLogPlugin, multi: true }
  ]
})
```

Please note that the most important part that enables multiple injections is the `multi` attribute we set to `true` when registering a provider.

Now let's get back to our `CompositeLogService` and inject instances of all previously registered plugins using the following format:

```ts
constructor(@Inject(LOGGER_PLUGIN) plugins: LogPlugin[])
```

To demonstrate the instances, we are going to enumerate the injected collection and log all plugin names to the browser console:

```ts
// src/app/services/composite-log.service.ts

import { Injectable, Inject } from '@angular/core';
import { LOGGER_PLUGIN } from './../tokens';

export interface LogPlugin {
  name: string;
  level: string;
  log(message: string);
}

@Injectable({ providedIn: 'root' })
export class CompositeLogService {

  constructor(@Inject(LOGGER_PLUGIN) plugins: LogPlugin[]) {
    if (plugins && plugins.length > 0) {
      for (const plugin of plugins) {
        console.log(`Loading plugin: ${plugin.name} (level: ${plugin.level})`);
      }
    }
  }

}
```

The service is ready for testing. The only thing we have left is to inject it somewhere.
The main application component is the best place to test all the newly introduced code quickly.

```ts
// src/app/app.component.ts

import { CompositeLogService } from './services/composite-log.service';

@Component({/*...*/})
export class AppComponent {
  
  constructor(private logService: CompositeLogService) {
  }
  
}
```

Now if you run the application the console log is going to contain the following output:

```text
Loading plugin: Error Log Plugin (level: error)
Loading plugin: Warning Log Plugin (level: warn)
```

In the real-life scenario, you would most probably want the log service to use different types of plugins for certain purposes.
Let's now extend our service and introduce a `log` method redirects logging calls to the plugins that support the corresponding levels.

```ts
// src/app/services/composite-log.service.ts

@Injectable({ providedIn: 'root' })
export class CompositeLogService {
  // ...

  log(level: string, message: string) {
    const logger = this.plugins.find(p => p.level === level);
    if (logger) {
      logger.log(message);
    }
  }
  
}
```

To test how it works you can even use the `log` method within the service itself.
Update the constructor to send a message once all the external plugins are enumerated:

```ts
@Injectable({ providedIn: 'root' })
export class CompositeLogService {
  
  constructor(@Inject(LOGGER_PLUGIN) private plugins: LogPlugin[]) {
    if (plugins && plugins.length > 0) {
      for (const plugin of plugins) {
        console.log(`Loading plugin: ${plugin.name} (level: ${plugin.level})`);
      }
      this.log('warn', 'All plugins loaded');
    }
  }
  
}
```

For the sake of simplicity, we are going to use the `warn` level because we got only `warn` and `error` loggers registered.
Feel free to extend the collection of the loggers with the `info` or `debug` one as an exercise.

Once you run the web application, the main component should provide the following output to the browser console:

```text
Loading plugin: Error Log Plugin (level: error)
Loading plugin: Warning Log Plugin (level: warn)
All plugins loaded
```

Now you are ready to deal with multiple instances injected as collections and got a basic scenario working in practice.

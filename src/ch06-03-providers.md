## Providers

In the previous examples, you should have noticed that to import and use a class decorated with `@Injectable`
one needs to declare its Type in the `providers` array of the main application Module.

That makes Angular "know" about the services when instantiating components for your web application.

You can have numerous services that perform various sets of functionality, all registered within the root module:

```ts
@NgModule({
  // ...,

  providers: [
    ...
    LogService,
    AuthenticationService,
    AvatarService,
    UserService,
    ChatService
  ],
  
  // ...
})
export class AppModule { }
```

The concept of `providers` in Angular goes beyond the collection of classes, in fact,
it supports several powerful ways to control how dependency injection behaves at runtime.

Besides strings, the framework supports an object-based notation for defining providers.

### Using a Class

Earlier in this chapter, we have been using a string value to define a LogService dependency in the `providers` section.
We can express the same value with the help of the following notation:

```ts
{ provide: <key>, useClass: <class> }
```

Let's take a look at the next example:

```ts
@NgModule({
  // ...
  
  providers: [
    { provide: LogService, useClass: LogService }
  ],

  //...
})
export class AppModule { }
```

We are using `LogService` both as a "key" for injection and as a "value" to build a new instance for injection.

The main feature of this approach is that "key" and "value" can be different classes.
That allows swapping the value of the service with a custom implementation if needed, or with a Mock object for an improved unit testing experience.

Now, let's create a custom logger implementation `CustomLogService`:

```sh
ng g service services/custom-log
```

Next, implement the `info` method, but this time it should contain some different output for us to distinguish both implementations:

```ts
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class CustomLogService {

  constructor() { }

  info(message: string) {
    console.log(`[custom]: [info] ${message}`);
  }

}
```

Finally, you need to import this class into the main application module and declare as a new provider with the "LogService" key.
Don't forget to comment out or remove the former logger declaration as in the example below:

```ts
import { LogService } from './services/log.service';
import { CustomLogService } from './services/custom-log.service';

@NgModule({
  // ...,

  providers: [
    // LogService
    { provide: LogService, useClass: CustomLogService }
  ],
  
  // ...
})
export class AppModule { }
```

The code above means that all the components that inject the `LogService` as part of the constructor parameters
are going to receive the `CustomLogService` implementation at runtime.

Essentially we are swapping the value of the logger, and no component is going to notice that.
That is the behavior developers often use for unit testing purposes.

If you now run the web application and navigate to browser console, you should see the following output:

```text
[custom]: [info] Component 1 created
[custom]: [info] Component 2 created
```

Strings now contain "[custom]: " as a prefix, which proves the `Component1` and `Component2`
are now dealing with the `CustomLogService` code that has been successfully injected using the `LogService` key.

### Using a Class Factory

Previously we have been relying on the Angular framework to create instances of the injectable entities.

There is also a possibility to control how the class gets instantiated if you need more than just a default constructor calls.
Angular provides support for "class factories" for that very purpose.

You are going to use the following notation for class factories:

```ts
{ provide: <key>, useFactory: <function> }
```

Before we jump into configuration details, let's extend our newly introduced `CustomLogService` service with the custom "prefix" support.
We are going to implement a special `setPrefix` method:

```ts
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class CustomLogService {

  private prefix = '[custom]';

  setPrefix(value: string) {
    this.prefix = value;
  }

  info(message: string) {
    console.log(`${this.prefix}: [info] ${message}`);
  }

}
```

As you can see from the code above the `info` method is going to use a custom prefix for all the messages.

Next, create an exported function `customLogServiceFactory` that is going to control how the CustomLogService instance gets created.
In our case we are going to provide a custom prefix like in the example below:

```ts
export function customLogServiceFactory() {
  const service = new CustomLogService();
  service.setPrefix('(factory demo)');
  return service;
}
```

As you can imagine, there could be more sophisticated configuration scenarios for all application building blocks,
including services, components, directives and pipes.

Finally, you can use the factory function for the `LogService`.
In this case, we both replace the real instance with the `CustomLogService`, and pre-configure the latter with a custom prefix for info messages:

```ts
@NgModule({
  // ...
  
  providers: [
    { provide: LogService, useFactory: customLogServiceFactory }
  ],
  
  // ...
})
export class AppModule { }
```

This time, when the application runs, you should see the following output:

```text
(factory demo): [info] Component 1 created
(factory demo): [info] Component 2 created
```

#### Class Factories With Dependencies

When you use the default provider registration for a service, directive or component,
the Angular framework automatically manages and injects dependencies if needed.
In the case of factories, you can use additional `deps` property to define dependencies
and allow your factory function to access corresponding instances during execution.

Let's imagine we need to manually bootstrap the `AuthenticationService`
that depends on the `RoleService` and `LogService` instances.

```ts
@Injectable({ providedIn: 'root' })
export class AuthenticationService {

  constructor(private roles: RoleService,
              private log: LogService) {
  }
  
  // ...
}
```

We should declare our factory-based provider the following way now:

```ts
@NgModule({
  // ...,
  
  providers: [
      {
        provide: AuthenticationService,
        useFactory: authServiceFactory,
        deps: [ RoleService, LogService ]
      }
  ],
  
  // ...
})
export class AppModule { }
```

With the code above we instruct Angular to resolve the `RoleService` and `LogService`
and use with our custom factory function when the `AuthenticationService` singleton instance gets created for the first time.

Finally, your factory implementation should look similar to the following one:

```ts
export function authServiceFactory(roles: RoleService, log: LogService) {
  const service = new AuthenticationService(roles, log);
  // do some additional service setup
  return service;
}
```

### Using @Inject Decorator

Another important scenario you might be interested in is the `@Inject` decorator.

The `@Inject` decorator instructs Angular that a given parameter must get injected at runtime.
You can also use it to get references to "injectables" using string-based keys.

To demonstrate `@Inject` decorator in practice let's create a `dateFactory` function to generate current date:

```ts
// src/app/app.module.ts

export function dateFactory() {
  return new Date();
}
```

Now we define a custom provider with the key `DATE_NOW` that is going to use our new factory.

```ts
@NgModule({
  // ...,

  providers: [
    { provide: 'DATE_NOW', useFactory: dateFactory }
  ],

  // ...
})
export class AppModule { }
```

For the next step, you can import the `@Inject` decorator from the `angular/core` namespace
and use it with the `Component1` we created earlier in this chapter:

```ts
import { /*...,*/ Inject } from '@angular/core';

@Component({...})
export class Component1Component {

  constructor(logService: LogService, @Inject('DATE_NOW') now: Date) {
    logService.info('Component 1 created');
    logService.info(now.toString());
  }

}
```

There are two points of interest in the code above. First, we inject `LogService` instance
as a `logService` parameter using its type definition: `logService: LogService`.
Angular is smart enough to resolve the expected value based on the `providers` section in the Module, using `LogService` as the key.

Second, we inject a date value into the `now` parameter.
This time Angular may experience difficulties resolving the value based on the `Date` type,
so we have to use the `@Inject` decorator to explicitly bind `now` parameter to the `DATE_NOW` value.

The browser console output, in this case, should be as the following one:

```text
(factory demo): [info] Component 1 created
(factory demo): [info] Sun Aug 06 2017 08:45:36 GMT+0100 (BST)
(factory demo): [info] Component 2 created
```

Another important use case for the @Inject decorator is using custom types in TypeScript
when the service has different implementation class associated with the provider key,
like in our early examples with LogService and CustomLogService.

Below is an alternative way you can use to import `CustomLogService` into the component and use all the API exposed:

```ts
@Component({/*...*/})
export class Component1Component {

  constructor(@Inject(LogService) logService: CustomLogService) {
    logService.info('Component 1 created');
  }

}
```

In this case, you are getting access to real `CustomLogService` class that is injected by Angular for all the `LogService` keys.
If your custom implementation has extra methods and properties, not provided by the `LogService` type, you can use them from within the component now.

This mechanism is often used in unit testing when the Mock classes expose additional features to control the execution and behavior flow.

### Using a Value

Another scenario for registering providers in the Angular framework is providing instances directly, without custom or default factories.

The format, in this case, should be as following:

```ts
{ provide: <key>, useValue: <value> }
```

There are two main scenarios of providing the values.

The first scenario is pretty much similar to the factory functions you can create and initialize the service instance
before other components and services use it.

Below is the basic example of how you can instantiate and register custom logging service by value:

```ts
const logService = new CustomLogService();
logService.setPrefix('(factory demo)');

@NgModule({
  // ...
  
  providers: [
    { provide: LogService, useValue: logService }
  ],
  
  // ...
})
export class AppModule { }
```

The second scenario is related to configuration objects you can pass to initialize or setup other components and services.

Let's now register a `logger.config` provider with a JSON object value:

```ts
@NgModule({
  // ...,

  providers: [
    {
      LogService,
      {
        provide: 'logger.config',
        useValue: {
          logLevel: 'info',
          prefix: 'my-logger'
        }
      }
    }
  ],
  
  // ...
})
export class AppModule { }
```

Now any component, service or directive can receive the configuration values by injecting it as `logger.config`.
To enable static type checking you can create a TypeScript interface describing the settings object:

```ts
export interface LoggerConfig {
  logLevel?: string;
  prefix?: string;
}
```

Finally, proceed to the `LogService` code and inject the JSON object
using the `logger.config` token and `LoggerConfig` interface like in the following example:

```ts
@Injectable({ providedIn: 'root' })
export class LogService {

  constructor(@Inject('logger.config') config: LoggerConfig) {
    console.log(config);
  }

  info(message: string) {
    console.log(`[info] ${message}`);
  }

}
```

For the sake of simplicity we just log the settings content to the browser console.
Feel free to extend the code with configuring the log service behavior based on the incoming setting values.

If you run the web application right now and open the browser console you should see the next output:

```text
{
  logLevel: 'info',
  prefix: 'my-logger'
}
```

Registering providers with exact values is a compelling feature when it comes to global configuration and setup.
Especially if you are building redistributable components, directives or services that developers can configure from the application level.

### Using an Alias

The next feature we are going to see in action is "provider alias".

You are probably not going to use this feature frequently in applications,
but it is worth taking a look at what it does if you plan to create and maintain redistributable component libraries.

Let's imagine a scenario when you have created a shared component library with an `AuthenticationService` service
that performs various login and logout operations that you and other developers
can reuse across multiple applications and other component libraries.

After some time you may find another service implementation with the same APIs,
or let's assume you want to replace the service with a newer `SafeAuthenticationService` implementation.

The main issue you are going to come across when replacing Types is related to breaking changes.
The old service might be in use in a variety of modules and applications, many files import the Type,
use in constructor parameters to inject it, and so on.

For the scenario above is where "alias" support comes to the rescue.
It helps you to smooth the transition period for old content and provide backwards compatibility with existing integrations.

Let's now take a look at the next example:

```ts
@NgModule({
  // ...
  
  providers: [
    SafeAuthenticationService,
    { provide: AuthenticationService, useExisting: SafeAuthenticationService }
  ],
  
  // ...
})
export class AppModule { }
```

As you can see from the example above, we register a new `SafeAuthenticationService` service
and then declare an `AuthenticationService` that points to the same `SafeAuthenticationService`.

Now all the components that use `AuthenticationService` are going to receive the instance of the `SafeAuthenticationService` service automatically.
All the newly introduced components can now reference new service without aliases.

> **Difference with the "useClass"**
>
> You may wonder what's the difference with the `useClass` provider registration compared to the `useExisting` one.
>
> When using `useClass`, you are going to end up with two different instances registered at the same time.
> That is usually not a desirable behavior as services may contain events for example, and various components may have issues finding the "correct" instance.
> 
> The `useExisting` approach allows you to have only one singleton instance referenced by two or more injection tokens.

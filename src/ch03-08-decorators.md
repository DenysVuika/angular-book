## Decorators

TypeScript introduces `decorators` feature, metadata expressions similar to Java annotation tags or C# and Swift attributes.
ECMAScript does not yet have native support for annotating classes and class members (the feature is in the `proposal` state),
so `decorators` is an experimental TypeScript feature.

Decorators have a traditional notation of `@expression` where `expression` is the name of the function that should be invoked at runtime.

This function receives `decorated` target as a parameter and can be attached to:

- class declaration
- method
- accessor
- property
- parameter

### Class Decorators

Class decorators are attached to class declarations.
At runtime, the function that backs the decorator gets applied to the class constructor.
That allows decorators inspecting, modifying or even replacing class instances if needed.

Here's a simple example of the `LogClass` decorator that outputs some log information every time being invoked:

```ts
function LogClass(constructor: Function) {
    console.log('LogClass decorator executed for the constructor:');
    console.log(constructor);
}
```

Now you can use newly created decorator with different classes:

```ts
@LogClass
class TextWidget {
    text: string;

    constructor(text: string = 'default text') {
        this.text = text;
    }

    render() {
        console.log(`Rendering text: ${this.text}`);
    }
}
```

When a new instance of `TextWidget` class is created, the `@LogClass` attribute will be automatically invoked:

```ts
let widget = new TextWidget();
widget.render();
```

The class decorator should produce the following output:

```text
LogClass decorator executed for the constructor:
[Function: TextWidget]
Rendering text: default text
```

#### Decorators with parameters

It is also possible passing values to decorators. You can achieve this with a feature known as `decorator factories`.
A _decorator factory_ is a function returning an expression that is called at runtime:

Let's create another simple decorator with log output that accepts additional `prefix` and `suffix` settings:

```ts
function LogClassWithParams(prefix: string, suffix: string) {
    return (constructor: Function) => {
        console.log(`
            ${prefix} 
            LogClassWithParams decorator called for: 
            ${constructor} 
            ${suffix}
        `);
    };
}
```

It can now be tested with the `TextWidget` class created earlier:

```ts
@LogClassWithParams('BEGIN:', ':END')
class TextWidget {
    text: string;

    constructor(text: string = 'default text') {
        this.text = text;
    }

    render() {
        console.log(`Rendering text: ${this.text}`);
    }
}

let widget = new TextWidget();
widget.render();
```

You have marked `TextWidget` class with the `LogClassWithParams` decorator having a `prefix` and `suffix` properties
set to `BEGIN:` and `:END` values. The console output, in this case, should be:

```text
BEGIN:
LogClassWithParams decorator called for: 
function TextWidget(text) {
    if (text === void 0) { text = 'default text'; }
        this.text = text;
    }
}
:END
```

#### Multiple decorators

You are not limited to a single decorator per class.
TypeScript allows declaring as much class and member decorators as needed:

```ts
@LogClass
@LogClassWithParams('BEGIN:', ':END')
@LogClassWithParams('[', ']')
class TextWidget {
    // ...
}
```

Note that decorators are called from right to left, or in this case from bottom to top.
It means that first decorator that gets executed is:

```ts
@LogClassWithParams('[', ']')
```

and the last decorator is going to be

```ts
@LogClass
```

### Method Decorators

Method decorators are attached to class methods and can be used to inspect, modify or completely replace method definition of the class.
At runtime, these decorators receive following values as parameters: target instance, member name and member descriptor.

Let's create a decorator to inspect those parameters:

```ts
function LogMethod(target: any, 
                   propertyKey: string, 
                   descriptor: PropertyDescriptor) {
    console.log(target);
    console.log(propertyKey);
    console.log(descriptor);
}
```

Below is an example of this decorator applied to a `render` method of `TextWidget` class:

```ts
class TextWidget {
    text: string;

    constructor(text: string = 'default text') {
        this.text = text;
    }

    @LogMethod
    render() {
        console.log(`Rendering text: ${this.text}`);
    }
}

let widget = new TextWidget();
widget.render();
```

The console output in this case will be as following:

```text
TextWidget { render: [Function] }
render
{ value: [Function],
  writable: true,
  enumerable: true,
  configurable: true }
Rendering text: default text
```

You can use `decorator factories` also with method decorators to support additional parameters.

```ts
function LogMethodWithParams(message: string) {
    return (target: any, 
            propertyKey: string, 
            descriptor: PropertyDescriptor) => {
        console.log(`${propertyKey}: ${message}`);
    };
}
```

This decorator can now be applied to methods. You can attach multiple decorators to a single method:

```ts
class TextWidget {
    text: string;

    constructor(text: string = 'default text') {
        this.text = text;
    }

    @LogMethodWithParams('hello')
    @LogMethodWithParams('world')
    render() {
        console.log(`Rendering text: ${this.text}`);
    }
}

let widget = new TextWidget();
widget.render();
```

Note that decorators are called from right to left, or in this case from bottom to top.
If you run the code the output should be as follows:

```text
render: world
render: hello
Rendering text: default text
```

### Accessor Decorators

Accessor decorators are attached to property `getters` or `setters` and can be used to inspect, modify or completely replace accessor definition of the property.
At runtime, these decorators receive following values as parameters: target instance, member name and member descriptor.

Note that you can attach accessor decorator to either `getter` or `setter` but not both.
This restriction exists because on the low level decorators deal with
[Property Descriptors](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
that contain both `get` and `set` accessors.

Let's create a decorator to inspect parameters:

```ts
function LogAccessor(target: any, 
                     propertyKey: string, 
                     descriptor: PropertyDescriptor) {
    console.log('LogAccessor decorator called');
    console.log(target);
    console.log(propertyKey);
    console.log(descriptor);
}
```

Now the decorator can be applied to the following `TextWidget` class:

```ts
class TextWidget {
    private _text: string;

    @LogAccessor
    get text(): string {
        return this._text;
    }

    set text(value: string) {
        this._text = value;
    }

    constructor(text: string = 'default text') {
        this._text = text;
    }
}

let widget = new TextWidget();
```

Once invoked the decorator should produce the following output:

```text
LogAccessor decorator called
TextWidget { text: [Getter/Setter] }
text
{ get: [Function: get],
  set: [Function: set],
  enumerable: true,
  configurable: true }
```

Same as with class and method decorators you can use decorator factories feature to pass parameters to your accessor decorator.

```ts
function LogAccessorWithParams(message: string) {
    return (target: any, 
            propertyKey: string, 
            descriptor: PropertyDescriptor) => {
        console.log(`Message from decorator: ${message}`);
    }
}
```

TypeScript allows using more than one decorator given you attach it to the same property accessor:

```ts
class TextWidget {
    private _text: string;

    @LogAccessorWithParams('hello')
    @LogAccessorWithParams('world')
    get text(): string {
        return this._text;
    }

    set text(value: string) {
        this._text = value;
    }

    constructor(text: string = 'default text') {
        this._text = text;
    }
}

let widget = new TextWidget();
```

The console output should be as shown below, note the right-to-left execution order:

```text
Message from decorator: world
Message from decorator: hello
```

In case you declare decorator for both accessors TypeScript generates an error at compile time:

```ts
class TextWidget {
    private _text: string;

    @LogAccessorWithParams('hello')
    get text(): string {
        return this._text;
    }
    
    @LogAccessorWithParams('world')
    set text(value: string) {
        this._text = value;
    }
}
```

```text
error TS1207: Decorators cannot be applied to multiple get/set accessors of the same name.
```

### Property Decorators

Property decorators are attached to class properties.
At runtime, property decorator receives the following arguments:

- target object
- property name

Due to technical limitations, it is not currently possible observing or modifying property initializers.
That is why property decorators do not get Property Descriptor value at runtime
and can be used mainly to observe a property with a particular name has been defined for a class.

Here's a simple property decorator to display parameters it gets at runtime:

```ts
function LogProperty(target: any, propertyKey: string) {
    console.log('LogProperty decorator called');
    console.log(target);
    console.log(propertyKey);
}
```

```ts
class TextWidget {

    @LogProperty
    id: string;

    constructor(id: string) {
        this.id = id;
    }

    render() {
        // ...
    }
}

let widget = new TextWidget('text1');
```

The output in this case should be as following:

```text
LogProperty decorator called
TextWidget { render: [Function] }
id
```

### Parameter Decorators

Parameter decorators are attached to function parameters.
At runtime, every parameter decorator function is called with the following arguments:

- target
- parameter name
- parameter position index

Due to technical limitations, it is possible only detecting that a particular parameter has been declared on a function.

Let's inspect runtime arguments with this simple parameter decorator:

```ts
function LogParameter(target: any, 
                      parameterName: string, 
                      parameterIndex: number) {
    console.log('LogParameter decorator called');
    console.log(target);
    console.log(parameterName);
    console.log(parameterIndex);
}
```

You can now use this decorator with a class constructor and method parameters:

```ts
class TextWidget {

    render(@LogParameter positionX: number, 
           @LogParameter positionY: number) {
        // ...
    }

}
```

Parameter decorators are also executed in right-to-left order.
So you should see console outputs for `positionY` and then `positionX`:

```text
LogParameter decorator called
TextWidget { render: [Function] }
render
1
LogParameter decorator called
TextWidget { render: [Function] }
render
0
```

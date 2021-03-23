## TypeScript Features

*[todo: Needs introduction][issue]*

[issue]: https://github.com/DenysVuika/angular-book/issues/4

### Types

TypeScript supports all the types used in JavaScript:

- **boolean**
- **number**
- **string**
- **arrays**

TypeScript also adds the following types:

- **enum**
- **any**
- **void**

#### Basic Types

##### Boolean

> The most basic datatype is the simple true/false value, which JavaScript and TypeScript call a boolean value.

```ts
let isEnabled: boolean = true;
```

Assigning non-Boolean value to the variable will produce an error.

```ts
isEnabled = 'YES';
// logger.ts(2,1): error TS2322: Type '"YES"' is not assignable to type 'boolean'.
```

It is also possible annotating function or method return types.

```ts
function isEmpty(str: string): boolean {
    return !str;
}
```

##### Number

TypeScript maps all JavaScript numbers to the `number` type:

- floating point numbers (default JavaScript type for all numbers)
- decimal numbers
- hexadecimal numbers
- binary literals (ES6)
- octal literals (ES6)

Here's an example:

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

##### String

Typescript supports ES6 **template literals** (formerly known as **template strings**).

As in ECMAScript 6, you use backticks (`) to enclose a string literal and **${}** to interpolate JavaScript variables or arbitrary expressions.

Either double quotes (") or single quotes (') can be used to surround string data.

```ts
let firstName: string = "Joan";
let lastName: string = 'Doe';
let fullName: string = `${firstName} ${lastName}`;
let template: string = `
    <h1>Title<h1>
    <p>Hello, ${fullName}</p>
`;
```

#### Arrays

There are two main ways you can provide type definition for arrays of values in TypeScript:

```ts
let arr1: string[] = [];
let arr2: Array<string> = new Array();
```

You can also initialize arrays upon declaring them:

```ts
let arr1: string[] = ['hello', 'world'];
let arr2: Array<string> = ['hello', 'world'];

let flags1: boolean[] = [true, false, true, false];
let flags2: boolean[] = new Array(false, true);
```

As in JavaScript arrays, you can **push** elements and access them by **index**

```ts
let users: string[] = [];

users.push('user1');

console.log(`First user: ${users[0]}`);
```

The sample above demonstrates array element access together with string interpolation.
When executed it should produce:

```text
First user: user1
```

#### Enum

TypeScript provides support for an **enumerated type** known in many languages (Swift, C#, Java, C, and others).
This data type consists of a set of named values mapped to numbers.

```ts
enum Suit { Club, Diamond, Heart, Spade };

let s: Suit = Suit.Spade;
```

By default numbering of enum members starts with 0 and increments by one.
You have full control of the values if needed.

```ts
enum Suit { Club = 1, Diamond, Heart, Spade };
enum Suit { Club = 1, Diamond = 2, Heart = 4, Spade = 8 }
```

Another valuable feature is accessing by a numeric value.

```ts
enum Suit { Club, Diamond, Heart, Spade };

console.log(Suit[0]); // Club
```

It must be noted however that you access names by the numeric values, not by an array index as it may seem.

```ts
enum Suit { Club = 1, Diamond, Heart, Spade };

console.log(Suit[0]); // undefined
console.log(Suit[1]); // Club
```

#### Any

A special **any** type is used to opt-out of the TypeScript type-checking process and addresses the following cases:

- dynamic content (objects created on the fly)
- 3rd party libraries (having no TypeScript support via definition files)

```ts
let obj: any = {
    log(message) {
        console.log(message);
    }
};
obj.log('hello world');
```

Please note that by opting-out of the type-checking process you take full responsibility for safety checks,
as now TypeScript compiler is not able to verify the code at compile time.

The following example shows valid TypeScript code:

```ts
obj.log('hello world'); 
obj.helloWorld('log');
```

However, at runtime the second line causes a TypeError exception:

```text
hello world
TypeError: obj.helloWorld is not a function
```

So it is recommended using **any** type only where necessary.

#### Void

The **void** type is used to declare a function does not return any value explicitly.

```ts
class Logger {

    log(message: string): void {
        console.log(message);
        return true;
    }

}
```

If you try compiling the code above you should get an error:

```text
error TS2322: Type 'true' is not assignable to type 'void'.
```

You can fix the type-check error by removing **return** statement from the **log** method:

```ts
class Logger {

    log(message: string): void {
        console.log(message);
    }

}
```

You might also be using **void** types as function parameters or with **Interfaces**:

```ts
function fn(x: () => void) {
  x();
}

interface Logger {

  log(message: string): void;
  warn(message: string): void;
  error(message: string): void;

}
```

_You will get more information on **Interfaces** later in this book._

### Classes

TypeScript provides support for classes introduced with ES6 (ECMAScript 2015) and adds a set of features to improve object-oriented development.

```ts
class Widget {

    id: string;

    constructor(id: string) {
        this.id = id;
    }

    render() {
        console.log(`Rendering widget "${this.id}"`);
    }

}

let widget = new Widget('text1');
widget.render();
```

You should get the following output when executed:

```text
Rendering widget "text1"
```

#### Properties

With ES6 you define class properties from with the class constructor:

```js
// ES6
class Widget {

    constructor(id) {
        this.id = id;
        this.x = 0;
        this.y = 0;
    }

}
```

If you try compiling example above with `tsc` utility (TypeScript compiler) you should get the following errors:

```text
error TS2339: Property 'id' does not exist on type 'Widget'.
error TS2339: Property 'x' does not exist on type 'Widget'.
error TS2339: Property 'y' does not exist on type 'Widget'.
```

The errors are raised because TypeScript requires you to define properties separately.
It is needed to enable many other features TypeScript provides.

```ts
class Widget {

    id: string;
    x: number;
    x: number;

    constructor(id: string) {
        this.id = id;
        this.x = 0;
        this.y = 0;
    }
}
```

Properties in TypeScript can have default values:

```ts
class Widget {

    id: string;
    x: number = 0;
    x: number = 0;

    constructor(id: string) {
        this.id = id;
    }
}
```

#### Setters and Getters

TypeScript supports _computed properties_, which do not store a value.
Instead, they provide _getters_ and _setters_ to retrieve and assign values in a controlled way.

**TBD**: describe get/set format

One of the common cases for a _getter_ is computing a return value based on other property values:

```ts
class User {

    firstName: string;
    lastName: string;

    get fullName(): string {
        return `${this.firstName} ${this.lastName}`.trim();
    }

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

}

let user = new User('Joan', 'Doe');
console.log(`User full name is: ${user.fullName}`);
```

If you save this example to file `script.ts`, compile it and run like shown below:

```sh
tsc --target ES6 script.ts
node script.js
```

You should see the output with the full username as expected:

```text
User full name is: Joan Doe
```

Now let's introduce a simple _setter_ for the `firstName` property.

Every time a new property value is set we are going to remove leading and trailing white space.
Such values as " Joan" and "Joan  " are automatically converted to "Joan".

```ts
class User {

    private _firstName: string;

    get firstName(): string {
        return this._firstName;
    }

    set firstName(value: string) {
        if (value) {
            this._firstName = value.trim();
        }
    }
}

let user = new User();
user.firstName = '  Joan   ';
console.log(`The first name is "${user.firstName}".`);
```

The console output, in this case, should be:

```text
The first name is "Joan".
```

#### Methods

Methods are functions that operate on a class object and are bound to an instance of that object.
You can use `this` keyword to access properties and call other methods like in the example below:

```ts
class Sprite {

    x: number;
    y: number;

    render() {
        console.log(`rendering widget at ${this.x}:${this.y}`);
    }
    
    moveTo(x: number, y: number) {
        this.x = x;
        this.y = y;
        this.render();
    }

}

let sprite = new Sprite();
sprite.moveTo(5, 10);
// rendering widget at 5:10
```

##### Return values

```ts
class NumberWidget {

    getId(): string {
        return 'number1';
    }

    getValue(): number {
        return 10;
    }

}
```

You can use a `void` type if the method does not return any value.

```ts
class TextWidget {

    text: string;

    reset(): void {
        this.text = '';
    }

}
```

##### Method parameters

You can add types to each parameter of the method.

```ts
class Logger {
    
    log(message: string, level: number) {
        console.log(`(${level}): ${message}`);
    }

}
```

TypeScript will automatically perform type checking at compile time.
Let's try providing a string value for the `level` parameter:

```ts
let logger = new Logger();
logger.log('test', 'not a number');
```

You should get a compile error with the following message:

```text
error TS2345: Argument of type '"string"' is not assignable to parameter of type 'number'.
```

Now let's change `level` parameter to a number to fix compilation

```ts
let logger = new Logger();
logger.log('test', 2);
```

Now we should get the expected output:

```text
(2): test
```

##### Optional parameters

By default, all method/function parameters in TypeScript are `required`.
However, it is possible making parameters optional by appending **?** (question mark) symbol to the parameter name.

Let's update our `Logger` class and make `level` parameter optional.

```ts
class Logger {
    
    log(message: string, level?: number) {
        if (level === undefined) {
            level = 1;
        }
        console.log(`(${level}): ${message}`);
    }

}

let logger = new Logger();
logger.log('Application error');
```

The `log` method provides default value automatically if `level` is omitted.

```text
(1): Application error
```

Please note that optional parameters must always follow required ones.

##### Default parameters

TypeScript also supports default values for parameters.
Instead of checking every parameter for `undefined` value you can provide defaults directly within the method declaration:

```ts
class Logger {
    
    log(message: string = 'Unknown error', level: number = 1) {
        console.log(`(${level}): ${message}`);
    }

}
```

Let's try calling `log` without any parameters:

```ts
let logger = new Logger();
logger.log('Application error');
```

The output, in this case, should be:

```text
(1): Application error
```

##### Rest Parameters and Spread Operator

In TypeScript, you can gather multiple arguments into a single variable known as _rest parameter_.
Rest parameters were introduced as part of ES6, and TypesScripts extends them with type checking support.

```ts
class Logger {

    showErrors(...errors: string[]) {
        for (let err of errors) {
            console.error(err);
        }
    }

}
```

Now you can provide an arbitrary number of arguments for `showErrors` method:

```ts
let logger = new Logger();
logger.showErrors('Something', 'went', 'wrong');
```

That should produce three errors as an output:

```text
Something
went
wrong
```

_Rest parameters_ in TypeScript work great with _Spread Operator_ allowing you to expand a collection into multiple arguments.
It is also possible mixing regular parameters with _spread_ ones:

```ts
let logger = new Logger();
let messages = ['something', 'went', 'wrong'];

logger.showErrors('Error', ...messages, '!');
```

In the example above we compose a collection of arguments from arbitrary parameters and content of the `messages` array in the middle.

The `showErrors` method should handle all entries correctly and produce the following output:

```text
Error
something
went
wrong
!
```

#### Constructors

Constructors in TypeScript got same features as methods.
You can have default and optional parameters, use rest parameters and spread operators with class constructor functions.

Besides, TypeScript provides support for automatic property creation based on constructor parameters.
Let's create a typical `User` class implementation:

```ts
class User {

    firstName: string;
    lastName: string;

    get fullName(): string {
        return `${this.firstName} ${this.lastName}`.trim();
    }

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

}
```

Instead of assigning parameter values to the corresponding properties we can instruct TypeScript to perform an automatic assignment instead.
You can do that by putting one of the access modifiers **public**, **private** or **protected** before the parameter name.

You are going to get more details on _access modifiers_ later in this book.
For now, let's see the updated `User` class using automatic property assignment:

```ts
class User {

    get fullName(): string {
        return `${this.firstName} ${this.lastName}`.trim();
    }

    constructor(public firstName: string, public lastName: string) {
    }

}

let user = new User('Joan', 'Doe');
console.log(`Full name is: ${user.fullName}`);
```

TypeScript creates `firstName` and `lastName` properties when generating JavaScript output.
You need targeting at least ES5 to use this feature.

Save example above to file `script.ts` then compile and run with `node`:

```sh
tsc script.ts --target ES5
node script.js
```

The output should be as following:

```text
Full name is: Joan Doe
```

You have not defined properties explicitly, but `fullName` getter was still able accessing them via `this`.
If you take a look at the emitted JavaScript you should see the properties are defined there as expected:

```js
// ES5
var User = (function () {
    function User(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    Object.defineProperty(User.prototype, "fullName", {
        get: function () {
            return (this.firstName + " " + this.lastName).trim();
        },
        enumerable: true,
        configurable: true
    });
    return User;
}());
var user = new User('Joan', 'Doe');
console.log("Full name is: " + user.fullName);
```

Now you can also switch to ES6 target to see how TypeScript assigns properties:

```sh
tsc script.ts --target ES6
```

The generated JavaScript, in this case, is even smaller and cleaner:

```js
// ES6
class User {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    get fullName() {
        return `${this.firstName} ${this.lastName}`.trim();
    }
}
let user = new User('Joan', 'Doe');
console.log(`Full name is: ${user.fullName}`);
```

#### Inheritance

One of the important TypeScript features is the class inheritance that enables OOP patterns for developers.
Under the hood TypeScript is using the same `extends` syntactic sugar when targeting ES6 JavaScript,
and prototypical inheritance wrappers when generating output in ES5.

We can refer to animals as a classic example of class-based programming and inheritance.

```ts
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    makeSound() {
        console.log('Unknown sound');
    }
}
```

You have created a basic `Animal` class that contains a `name` property and `makeSound` method.
That translates to ES5 as following:

```js
// ES5
var Animal = (function () {
    function Animal(name) {
        this.name = name;
    }
    Animal.prototype.makeSound = function () {
        console.log('Unknown sound');
    };
    return Animal;
}());
```

Now you can create a `Dog` implementation that provides a right sound:

```ts
class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }
    makeSound() {
        console.log('Woof-woof');
    }
}
```

Please note that if you have a constructor in the base class, then you must call it from all derived classes.
Otherwise, TypeScript should raise a compile-time error:

```text
error TS2377: Constructors for derived classes must contain a 'super' call.
```

Here's how a `Dog` gets converted to ES5:

```ts
var Dog = (function (_super) {
    __extends(Dog, _super);
    function Dog(name) {
        return _super.call(this, name) || this;
    }
    Dog.prototype.makeSound = function () {
        console.log('Woof-woof');
    };
    return Dog;
}(Animal));
```

Now let's add a `Cat` implementation with its sound and test both classes:

```ts
class Cat extends Animal {
    constructor(name: string) {
        super(name);
    }
    makeSound() {
        console.log('Meow');
    }
}

let dog = new Dog('Spot');
let cat = new Cat('Tom');

dog.makeSound();
cat.makeSound();
```

Once the code compiles and executes you should get the following output:

```text
Woof-woof
Meow
```

#### Access Modifiers

TypeScript supports `public`, `private` and `protected` modifiers for defining accessibility of the class members.

##### Public

By default, each member of the class is `public` so that you can omit it.
However, nothing stops you from declaring `public` modifier explicitly if needed:

```ts
class User {
    public firstName: string;
    public lastName: string;

    public speak() {
        console.log('Hello');
    }

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

Now if you compile example above to JavaScript you should see the following:

```js
var User = (function () {
    function User(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    User.prototype.speak = function () {
        console.log('Hello');
    };
    return User;
}());
```

##### Private

You mark a member as `private` when it should never be accessed from outside of its containing class.
One of the most common scenarios is creating private fields to hold values for properties.
For example:

```ts
class User {
    private _firstName: string;
    private _lastName: string;

    get firstName() {
        return this._firstName;
    }

    get lastName() {
        return this._lastName;
    }

    constructor(firstName: string, lastName: string) {
        this._firstName = firstName;
        this._lastName = lastName;
    }
}
```

The class we have created above allows setting user's first and last name only from within the constructor.

If you try changing name properties from outside the class, TypeScript will raise an error at compile time:

```ts
let user = new User('John', 'Doe');
user.firstName = 'Rob';
// error TS2540: Cannot assign to 'firstName' because it is a constant or a read-only property.
```

##### Protected

The `protected` modifier restricts member visibility from outside of the containing class but provides access from the derived classes.

Let's start with base `Page` class implementation:

```ts
class Page {

    protected renderHeader()    { /* ... */ }
    protected renderContent()   { /* ... */ }
    protected renderFooter()    { /* ... */ }

    render() {
        this.renderHeader();
        this.renderContent();        
        this.renderFooter();
    }
}
```

We created a `Page` class that has public method `render`.
Internally `render` calls three separate methods to render header, content and footer of the page.
These methods are not available from the outside the the class.

Now we are going to create a simple derived `AboutPage` class:

```ts
class AboutPage extends Page {

    private renderAboutContent() { /* ... */ }

    render() {
        this.renderHeader();
        this.renderAboutContent();
        this.renderFooter();
    }

}
```

As you can see the `AboutPage` defines its `render` method that calls
`renderHeader` and `renderFooter` in parent class but puts custom content in the middle.

You can also use `protected` modifier with class constructors.
In this case, the class can be instantiated only by the derived classes that extend it.
That becomes handy when you want to have properties and methods available for multiple classes as a base implementation,
but don't want a base class to be instantiated outside its containing class.

For example

```ts
class Page {
    protected constructor(id: string) {
        // ...
    }

    render() { /* base render */ }
}

class MainPage extends Page {
    constructor(id: string) {
        super(id);
    }

    render() { /* render main page */ }
}

class AboutPage extends Page {
    constructor(id: string) {
        super(id);
    }

    render() { /* render about page */ }
}

let main = new MainPage('main');
let about = new AboutPage('about');
```

You can create instances of `MainPage` and `AboutPage` both having access to protected members of the `Page` class.
However, you are not able creating an instance of the `Page` class directly.

```ts
let page = new Page(); 
// error TS2674: Constructor of class 'Page' is protected and only accessible within the class declaration.
```

#### Readonly modifier

One of the common ways to create a read-only property in many object-oriented programming languages
is by having a private local variable with a `getter` only.

```ts
class Widget {

    private _id: string;

    get id(): string {
        return this._id;
    }

    constructor(id: string) {
        this._id = id;
    }
}

let widget = new Widget('textBox');
console.log(`Widget id: ${widget.id}`);
// Widget id: textBox
```

You can also make properties read-only by using the `readonly` keyword.
That reduces repetitive typing when dealing with many read-only properties, and greatly improves overall code readability.

Let's update the previous example to use `readonly`:

```ts
class Widget {
    readonly id: string;

    constructor(id: string) {
        this.id = id;
    }
}
```

If you try changing the value of the property outside of the constructor TypeScript will raise an error:

```ts
let widget = new Widget('text');
widget.id = 'newId';
// error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

You can provide default values for read-only properties only in two places: property declaration and constructor.

```ts
class Widget {
    readonly id: string;
    readonly minWidth: number = 200;
    readonly minHeight: number = 100;

    constructor(id: string) {
        this.id = id;
    }
}

let widget = new Widget('text');
widget.minWidth = 1000;
// error TS2540: Cannot assign to 'minWidth' because it is a constant or a read-only property.
```

### Interfaces

An _interface_ is a description of the actions that an object can do.

You might already be familiar with _interfaces_ in other programming languages like C# and Java,
or _contracts_ in Swift.

Interfaces are not part of the ECMAScript.
It is a level of abstraction supported by TypeScript to improve the type-checking process, and not converted to JavaScript code.

Here's an example of an interface describing generic **Text** component:

```ts
interface TextComponent {

    text: string;
    render(): void;

}
```

Now you can use the interface above to describe the requirement of having the **text** property that is a string and a **render** method:

```ts
class PlainTextComponent implements TextComponent {

    text: string;

    render() {
        console.log('rendering plain text component');
    }

}
```

We are using `implements` keyword to wire class with a particular interface.
It is not important in what order class members are defined as long as all properties and methods the interface requires
are present and have required types.

Let's create another class that implements `TextComponent` interface partially:

```ts
class RichTextComponent implements TextComponent {
    text: string;
}
```

Upon compilation TypeScript will produce the following error:

```text
error TS2420: Class 'RichTextComponent' incorrectly implements interface 'TextComponent'.
Property 'render' is missing in type 'RichTextComponent'.
```

You can use multiple interfaces delimited by a comma:

```ts
class RichTextComponent implements TextComponent, OnInit, OnDestroy {
    // ...
}
```

The example above shows a class that must implement three different interfaces to compile.

### Abstract Classes

Interfaces describe only requirements for classes; you cannot create an instance of the interface.
You need `abstract` classes un order to provide implementation details.

```ts
abstract class PageComponent {

    abstract renderContent(): void;

    renderHeader() {
        // ...
    }

    renderFooter() {
        // ...
    }
}
```

Same as with interfaces you cannot create instances of abstract classes directly, only other classes derived from an abstract one.
Also, it is possible marking class methods as `abstract`.
Abstract methods do not contain implementation, and similar to `interface` methods provide requirements for derived classes.

```ts
class HomePageComponent extends PageComponent {

    renderContent() {
        this.renderHeader();
        console.log('rendering home page');
        this.renderFooter();
    }

}
```

Note how `HomePageComponent` implements abstract `renderContent` that has access to `renderHeader` and `renderFooter` methods carried out in the parent class.

You can also use access modifiers with abstract methods.
The most frequent scenario is when methods need to be accessible only from within the child classes, and invisible from the outside:

For example:

```ts
abstract class PageComponent {

    protected abstract renderContent(): void;

    renderHeader() {
        // ...
    }

    renderFooter() {
        // ...
    }
}
```

Now `HomePageComponent` can make `renderContent` protected like shown below:

```ts
class HomePageComponent extends PageComponent {

    constructor() {
        super();
        this.renderContent();
    }

    protected renderContent() {
        this.renderHeader();
        console.log('rendering home page');
        this.renderFooter();
    }

}
```

Any additional class that inherits (extends) `HomePageComponent` will still be able calling or redefining `renderContent` method.
But if you try accessing `renderContent` from outside the TypeScript should raise the following error:

```ts
let homePage = new HomePageComponent();
homePage.renderContent();
// error TS2445: Property 'renderContent' is protected and only 
// accessible within class 'HomePageComponent' and its subclasses.
```

Abstract classes is a great way consolidating common functionality in a single place.

### Modules

TypeScript supports the concept of modules introduced in ES6.
Modules allow isolating code and data and help splitting functionality into logical groups.

One of the major features of ES6 (and TypeScript) modules is their file scope.
The code inside the module (classes, variables, functions, and other) does not pollute global scope
and is not accessible from the outside unless `exported` explicitly.

To share the code of the module with the outside world, you use `export` keyword:

```ts
// module1.ts
export class TextBoxComponent {
    constructor(public text: string) {}
    render() {
        console.log(`Rendering '${this.text}' value.`);
    }
}
```

To use this code in your main application file or another module, you must import it first.
You import the `TextBoxComponent` class using `import` keyword:

```ts
// app.ts
import { TextBoxComponent } from './module1'

let textBox = new TextBoxComponent('hello world');
textBox.render();
```

#### Module Loaders

ES6 and TypeScript rely on `module loaders` to locate files, resolve external dependencies and execute module files.

The most popular module loaders are:

- Server side
  - CommonJs (used by node.js)
- Client side
  - SystemJS
  - RequireJS
  - Webpack

TypeScript supports different formats of generated JavaScript output.
You can instruct compiler to generate code adopted for multiple module loading systems using formats such as

- CommonJS (used in node.js)
- RequireJS
- UMD (Universal Module Definition)
- SystemJS
- ES6 (or ECMAScript 2015)

#### Running at server side

You can test `TextBoxComponent` we have created earlier with node.js using `commonjs` module target:

```sh
tsc app.ts --module commonjs
node app.js
```

When executed it produces the following output:

```text
Rendering 'hello world' value.
```

TypeScript automatically compiles referenced modules.
It starts with `app.ts`, resolves and compiles `module1` as `module1.ts` file,
and produces two JavaScript files `app.js` and `module.js` that can be executed by node.js.

Here's an example of `app.js` file content:

```js
"use strict";
// app.ts
var module1_1 = require("./module1");
var textBox = new module1_1.TextBoxComponent('hello world');
textBox.render();
```

#### Running in browser

In order to run module-based application in browser you can take `SystemJS` loader:

```html
<script src="systemjs/dist/system.js"></script>
<script>
  SystemJS.import('/app/app.js');
</script>
```

Let's take a look at a simple TypeScript application that references an external module.

```ts
// logger.ts
export class Logger {

    output: any;

    constructor(outputId: string) {
        this.output = document.getElementById(outputId);
    }

    info(message: string) {
       this.output.innerText = `INFO: ${message}`;
    }

}
```

Our simple `logger` is going to put a message as a content of the document element provided from the outside.

```ts
// app.ts
import { Logger } from './logger';

let logger = new Logger('content');
logger.info('hello world');
```

The application needs to be compiled with SystemJS support to load correctly.
You can configure TypeScript to generate compatible JavaScript code by setting module code generation setting to `system`:

```sh
tsc app.ts --module system
```

> **Source code**
>
> You can find source code for the examples above in the "[typescript/systemjs-example](https://github.com/DenysVuika/developing-with-angular/tree/master/typescript/systemjs-example)" folder.

To install dependencies, compile and run the demo use the following commands:

```sh
npm install
npm start
```

Your default browser should run example page automatically.
Once the page gets loaded you should see an expected message:

```text
INFO: hello world
```

### Decorators

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

#### Class Decorators

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

##### Decorators with parameters

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

##### Multiple decorators

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

#### Method Decorators

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

#### Accessor Decorators

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

#### Property Decorators

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

#### Parameter Decorators

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

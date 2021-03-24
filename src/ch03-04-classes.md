## Classes

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

### Properties

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

### Setters and Getters

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

### Methods

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

#### Return values

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

#### Method parameters

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

#### Optional parameters

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

#### Default parameters

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

#### Rest Parameters and Spread Operator

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

### Constructors

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

### Inheritance

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

### Access Modifiers

TypeScript supports `public`, `private` and `protected` modifiers for defining accessibility of the class members.

#### Public

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

#### Private

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

#### Protected

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

### Readonly modifier

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

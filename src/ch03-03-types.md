## Types

TypeScript supports all the types used in JavaScript:

- **boolean**
- **number**
- **string**
- **arrays**

TypeScript also adds the following types:

- **enum**
- **any**
- **void**

### Basic Types

#### Boolean

The most basic datatype is the simple true/false value, which JavaScript and TypeScript call a boolean value.

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

#### Number

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

#### String

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

### Arrays

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

### Enum

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

### Any

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

### Void

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

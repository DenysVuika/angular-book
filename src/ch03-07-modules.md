## Modules

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

### Module Loaders

ES6 and TypeScript rely on `module loaders` to locate files, resolve external dependencies and execute module files.

The most popular module loaders are:

- Server side
  - [CommonJs](http://www.commonjs.org/) (used by Node.js)
- Client side
  - [SystemJS](https://github.com/systemjs/systemjs)
  - [RequireJS](https://requirejs.org/)
  - [Webpack](https://webpack.js.org/)

TypeScript supports different formats of generated JavaScript output.
You can instruct compiler to generate code adopted for multiple module loading systems using formats such as

- [CommonJs](http://www.commonjs.org/) (used in Node.js)
- [RequireJS](https://requirejs.org/)
- UMD (Universal Module Definition)
- [SystemJS](https://github.com/systemjs/systemjs)
- ES6 (or ECMAScript 2015)

### Running at server side

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

### Running in browser

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

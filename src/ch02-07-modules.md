# Modules

Before ES6 developers traditionally were using `Revealing Module` pattern to emulate modules in JavaScript.

The basic concept of a Revealing Module is that you use `closures` (self-invoking functions)
with an `Object` which encapsulates its data and behavior.

```js
// ES5
var Module = (function() {
    var privateMethod = function() {
        // do something 
        console.log('private method called');
    };

    return {
        x: 10,
        name: 'some name',
        publicMethod: function() {
            // do something
            console.log('public method called');
            privateMethod();
        }
    };
})();

Module.publicMethod()
```

You should get the following output to browser console:

```text
public method called
private method called
```

I recommend also reading an excellent article "[Mastering the Module Pattern](https://toddmotto.com/mastering-the-module-pattern/)" by Todd Motto to get deep coverage of **Revealing Module** pattern in JavaScript.

The rise of module systems based on either AMD or CommonJS syntax has mostly replaced revealing modules and other hand-written solutions in ES5.

### Exporting and Importing Values

ECMAScript 6 provides a long-needed support for exporting and importing values from/to modules without global namespace pollution.

```js
// ES6

// module lib/logger.js
export function log (message) { console.log(message); };
export var defaultErrorMessage = 'Aw, Snap!';

//  myApp.js
import * as logger from "lib/logger";
logger.log(logger.defaultErrorMessage);

//  anotherApp.js
import { log, defaultErrorMessage } from "lib/logger";
log(defaultErrorMessage);
```

Here's how the same approach would look like if written with ECMAScript 5:

```js
// ES5

// lib/logger.js
LoggerLib = {};
LoggerLib.log = function(message) { console.log(message); };
LoggerLib.defaultErrorMessage = 'Aw, Snap!';

// myApp.js
var logger = LoggerLib;
logger.log(logger.defaultErrorMessage);

// anotherApp.js
var log = LoggerLib.log;
var defaultErrorMessage = LoggerLib.defaultErrorMessage;
log(defaultErrorMessage);
```

### Default Values

You can make your ES6 module exporting some value as `default` one.

```js
// ES6

// lib/logger.js
export default (message) => console.log(message);

// app.js
import output from 'lib/logger';
output('hello world');
```

### Wildcard Export

Another great feature of ES6 modules is support for wildcard-based export of values.
That becomes handy if you are creating a composite module that re-exports values from other modules.

```js
// ES6

// lib/complex-module.js
export * from 'lib/logger';
export * from 'lib/http';
export * from 'lib/utils';

// app.js
import { logger, httpClient, stringUtils } from 'lib/complex-module';
logger.log('hello from logger');
```

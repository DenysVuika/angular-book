## Extended Parameter Handling

ES6 brings improvements to parameter handling by introducing `default values`, `rest parameter` and `spread operator`.

### Default Parameter Values

Simple and intuitive default values for function parameters.

```js
// ES6
function playSound(file, volume = 50) {
  console.log(`Playing '${file}' with volume ${volume}.`);
}
playSound('test.mp3'); 
// Playing 'test.mp3' with volume 50.

playSound('test.mp3', 70);
// Playing 'test.mp3' with volume 70.
```

With ES5 you have to check every parameter to be `undefined` and setting defaults manually if needed.

```js
// ES5
function playSound(file, volume) {
    if (volume === undefined) {
        volume = 50;
    }
    console.log("Playing '" + file + "' with volume " + volume);
}
playSound('test.mp3'); 
// Playing 'test.mp3' with volume 50.

playSound('test.mp3', 70);
// Playing 'test.mp3' with volume 70.
```

So support for `default parameter values` is a huge step forward and real time saver.

### Rest Parameter

In ES5, if you want your function to handle an indefinite or an arbitrary number of arguments,
you must use special `arguments` variable:

```js
// ES5
function logMessages() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}

logMessages('Hello,', 'world!');
```

Which produces:

```text
Hello,
world!
```

In ES6, you can aggregate all remaining arguments into a single function parameter

```js
// ES6
function logMessages(...messages) {
  for (const message of messages) {
    console.log(message);
  }
}

logMessages('Hello,', 'world!');
```

Also, that gives the same console output as before:

```text
Hello, 
world!
```

Rest parameters become even more valuable when you need collecting arguments starting from a different position.

In the next example, the rest parameter is used to collect arguments from the second one to the end of the array.

```js
// ES6
function greet(message, ...friends) {
  for (const friend of friends) {
    console.log(`${message}, ${friend}!`);
  }
}

greet('Hello', 'John', 'Joan', 'Bob')
```

The function above allows you to set the greeting message as the first parameter and array of friend names to generate messages.
The console output, in this case, should be:

```text
Hello, John!
Hello, Joan!
Hello, Bob!
```

### Spread Operator

Spread operator is used to expand an iterable collection into multiple arguments.

```js
// ES6
let positive = [ 1, 2, 3 ];
let negative = [ -1, -2, -3 ]

let numbers = [...negative, 0, ...positive];

console.log(numbers);
// [-1, -2, -3, 0, 1, 2, 3]
```

You can use spread operator even with strings:

```js
// ES6
let message = 'Hello, world';
let chars = [...message];

console.log(chars);
// ["H", "e", "l", "l", "o", ",", " ", "w", "o", "r", "l", "d"]
```

Spread operator easily becomes an alternative to the `Array.prototype.concat()` method.
With ES5 the example above will look like the following:

```js
// ES5
var positive = [ 1, 2, 3 ];
var negative = [ -1, -2, -3 ];
var zero = [0];

var numbers = negative.concat(zero, positive);

console.log(numbers);
// [-1, -2, -3, 0, 1, 2, 3]
```

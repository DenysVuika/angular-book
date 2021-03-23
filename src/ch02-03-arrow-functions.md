## Arrow Functions

ES6 offers a shorter syntax for a **function expression** called **arrow function**, also known as **fat arrow function**.

Arrow functions provide more expressive closure syntax, simplify function scoping and change the way `this` is handled.

### Expression Bodies

When used as expressions bodies arrow functions work much like anonymous one-line **lambdas** that you can meet in many programming languages.

Let's filter a book collection to find something to read using both ES5 and ES6 to see the difference:

```js
var books = [
    { name: 'Book 1', read: true },
    { name: 'Book 2' , read: false },
    { name: 'Book 3', read: true }
];

// ES5
var booksToRead = books.filter(function (b) { return !b.read });

// ES6
var booksToRead = books.filter(b => !b.read);
```

Curly brackets and `return` statement are not required if only one expression is present.

You could write the same example like following:

```js
// ES6
let booksToRead = books.filter(b => { return !b.read; });
```

### Statement Bodies

Arrow functions provide more expressive closure syntax.

```js
// ES6
// list the books I've read
books.forEach(b => {
    if (book.read) {
        console.log(b.name);
    }
});
```

And another example using DOM:

```js
// ES6
let button = document.getElementById('submit-button');

button.addEventListener('click' () => {
    this.onButtonClicked();
});
```

Parameterless arrow functions are much easier to read

```js
// ES6
setTimeout(_ => {
    console.log('First callback');
    setTimeout(_ => {
        console.log('Second callback');
    }, 1);
}, 1);
```

### Lexical _this_

One of the best features of arrow functions in ES6 is the more intuitive handling of current object context.
These function expressions do not bind their variables:

- arguments
- super
- this
- new.target

```js
// ES6
this.books.forEach(b => {
    if (!b.read) {
        this.booksToRead.push(b);
    }
});
```

There are multiple ways of doing the same with ECMAScript 5, and all of them involve manual context management

```js
// ES5: using 'bind()'
this.books.forEach(function(b) {
  if (!b.read) {
    this.booksToRead.push(b);
  }
}).bind(this);

// ES5: referencing 'this' via variables
var self = this;

this.books.forEach(function(b) {
  if (!b.read) {
    self.booksToRead.push(b);
  }
});

// ES5: passing context if supported
this.books.forEach(function(b) {
  if (!b.read) {
    this.booksToRead.push(b);
  }
}, this);
```

As arrow functions do not create and bind their own `this` context the following code is concise and works as expected:

```js
// ES6
function ProgressBar() {
  this.progress = 0;

  setInterval(() => {
    this.progress++;
  }, 1000);
}

const p = new ProgressBar();
```

In the example above `this` properly refers to the `ProgressBar` object.
Before ES6 you would most probably additional variables like `self`, `that`, and other.

```js
// ES5
function ProgressBar() {
  var self = this;
  self.progress = 0;

  setInterval(function () {
    self.progress++;
  }, 1000);
}
```

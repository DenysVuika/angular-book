## Template Literals

Template Literals (formerly called "template strings" in prior drafts of the ECMAScript 6 language specification) are string literals providing intuitive expression interpolation for single-line and multiline strings. 

You use backticks to enclose a string literal and ${} to interpolate JavaScript variables or arbitrary expressions

```js
// ES6
let point = { x: 10, y: 20 };

console.log(`Position is ${point.x}:${point.y}`);
// output: Position is 10:10
```

With ES5 you have to concatenate strings when dealing with multiple lines:

```js
// ES5
var title = 'Title'
var component = {
  template: '' + 
    '<h1>' + title + '<h1>\n' +
    '<div class="grid">\n' +
    '   <div class="col-6"></div>\n' +
    '   <div class="col-6"></div>\n' +
    '</div>'
}
```

Multi-line string creation with template literals becomes very clean and readable:

```js
// ES6
let title = 'Title';
let component = {
  template: `
    <h1>${title}</h1>
    <div class="grid">
      <div class="col-6"></div>
      <div class="col-6></div>
    </div>
  `
}
```

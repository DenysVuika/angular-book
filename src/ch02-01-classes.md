## Classes

The `class` syntax in JavaScript is not a new object-oriented inheritance model
but simply a syntactical sugar on top of the existing prototype-based inheritance.

Traditionally we have been using standard Objects and Prototypes like shown below:

```js
var Widget = function(id, x, y) {
    this.id = id;
    this.setPosition(x, y);
}
Widget.prototype.setPosition = function(x, y) {
    this.x = x;
    this.y = y;
}
```

With class syntax developers get more natural and boilerplate-free result:

```js
class Widget {
    constructor(id, x, y) {
        this.id = id;
        this.setPosition(x, y);
    }

    setPosition(x, y) {
        this.x = x;
        this.y = y;
    }
}
```

The `constructor` function is automatically called when you create a new instance of `Widget`:

```js
const myWidget = new Widget(1, 10, 20);
```

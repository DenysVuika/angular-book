## Inheritance

The `extends` keyword is used to define a class as a child of another class.
The following example demonstrates inheritance in practice:

```js
class TextBox extends Widget {
    constructor (id, x, y, text) {
        super(id, x, y);
        this.text = text;
    }
}
```

We created a new `TextBox` class that is based on the `Widget` and adds additional `text` property.

Note that a base Widget constructor must also be called when a child class instantiated.
It must be the very first line of the child constructor implementation.

Here's another example:

```js
class ImageBox extends Widget {
    constructor (id, x, y, width, height) {
        super(id, x, y);
        this.setSize(width, height);
    }

    setSize(width, height) {
        this.width = width;
        this.height = height;
    }

    reset() {
        this.setPosition(0, 0);
        this.setSize(0, 0);
    }
}
```

`ImageBox` also inherits `Widget` class and adds size-related information alongside position.

Access to both classes is demonstrated with the `reset` function that calls `Widget.setPosition` and `ImageBox.setSize` functions.

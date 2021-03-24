## Abstract Classes

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

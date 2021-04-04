## Component Events

It is now time to improve our `Header` component.

Let's provide a way to set the content text for the component to display when rendered,
and a simple click event `contentClick` that gets emitted every time user clicks the content.

```ts
// src/app/panel-header/panel-header.component.ts

import { /*...,*/ Input, Output, EventEmitter } from '@angular/core';

@Component({...})
export class PanelHeaderComponent {

  @Input()
  content = 'Panel header';

  @Output()
  contentClick = new EventEmitter();

  onContentClicked() {
    console.log('panel header clicked');
    this.contentClick.next();
  }

}
```

From the earlier chapters, you already know that we use `@Input` decorator
for the class properties we want to set or bind from the outside.
We also need to use the `@Output` decorator to mark our events.

> **Output Events**
>
> You can get more details on the component events in the [Output Events](#output-events) section of the Components chapter.

Also, note that in the example above we also add the `onContentClicked` method that is going to raise our `contentClick` event.

Below is an example of a minimal `Header` component template we can use to display a `content` value:

```html
<!-- src/app/panel-header/panel-header.component.html -->

<p (click)="onContentClicked()">
    {{ content }}
</p>
```

As you can see, we link the `click` event of the enclosed `p` element with the `onContentClicked` handler
that temporarily sends a message to the console log for testing/debugging purposes
and also invokes the `contentClicked` event that other components can use.

Also, we set a default value for the `content` property to the "Panel header" string.
So at the runtime, the content or application main page is going to look similar to the following:

```text
Panel header

panel works!

panel-footer works!
```

However, we are not going to use the `Header` component directly.
It is the `Panel` component that needs it.
So we should allow our `Panel` component to control the header content, and also react on header click events.
Let's toggle the panel content as an example.

Edit the panel component class and add the `header` input property to hold the text for the `Header`,
and `displayBody` property to serve as a flag for showing and hiding the main panel content,
like in the example below:

```ts
// src/app/panel/panel.component.ts

import { /*...,*/ Input } from '@angular/core';

@Component({...})
export class PanelComponent {

  @Input()
  header = 'My panel header';

  displayBody = true;
}
```

For the next step, let's update the panel component template
to link the `Header` properties with the newly introduced class members:

```html
<!-- src/app/panel/panel.component.html -->

<app-panel-header
  [content]="header"
  (contentClick)="displayBody = !displayBody">
</app-panel-header>

<ng-container *ngIf="displayBody">
  <p>
    panel works!
  </p>
</ng-container>

<app-panel-footer></app-panel-footer>
```

You can now run the application and test your components by clicking the panel header text multiple times.
The panel should toggle its body content every time a header gets clicked.

Below is how the panel should look like by default, in the expanded state:

```text
My panel header

panel works!

panel-footer works!
```

Also, the next example shows how the panel looks like in the `collapsed` state:

```text
My panel header

panel-footer works!
```

Congratulations, you just got the component events working, and tested them in practice.
Now feel free to extend the `PanelFooterComponent` and add similar `content` and `contentClick` implementations.

### Bubbling Up Child Events

We have created a `PanelHeader` component earlier in this chapter.
Also, we introduced a `click` event for the component and made the `Panel` component host it within its template,
and toggle panel body content every time the `Panel Header` is clicked.

Imagine that the `<app-panel>` is a redistributable component, and you would like developers to have access to header clicks as well.
The `<app-panel-header>` however, is a child element, and developers do not have direct access to its instance when working with the `Panel`.
In this case, you would probably want your main `Panel` component re-throwing its child events.

We already got the `header` and the `footer` input properties that hold the values for the `<app-panel-header` and `<app-panel-footer>` elements.
Let's now introduce two new output events and call them `headerClick` and `footerClick`.

```ts
// src/app/panel/panel.component.ts

export class PanelComponent {

  displayBody = true;

  @Input()
  header = 'My panel header';

  @Input()
  footer = 'My panel footer';

  @Output()
  headerClick = new EventEmitter();

  @Output()
  footerClick = new EventEmitter();

  onHeaderClicked() {
    this.displayBody = !this.displayBody;
    this.headerClick.next();
  }

  onFooterClicked() {
    this.footerClick.next();
  }

}
```

As you can see from the code above, we also get two methods to raise our events.
The `onHeaderClicked` method is still toggling the panel body before raising the `headerClick` event.

Next, our `<app-panel>` component is going to watch the `contentClick` events of the child elements, and emit events for developers.
Update the HTML template and subscribe to the header and footer events like in the example below:

```html
<!-- src/app/panel/panel.component.html -->

<app-panel-header
  [content]="header"
  (contentClick)="onHeaderClicked()">
</app-panel-header>

<ng-container *ngIf="displayBody">
    <p>
        panel works!
    </p>
</ng-container>

<app-panel-footer
  [content]="footer"
  (contentClick)="onFooterClicked()">
</app-panel-footer>
```

Finally, let's test our panel events in action.
Update your main application template and subscribe to our newly introduced events for header and footer clicks:

```html
<!-- src/app/app.component.html -->

<app-panel
  (headerClick)="onHeaderClicked()"
  (footerClick)="onFooterClicked()">
</app-panel>
```

For the sake of simplicity we are going to log messages to browser console similar to the following:

```ts
// src/app/app.component.ts

@Component({/*...*/})
export class AppComponent {

  onHeaderClicked() {
    console.log('App component: Panel header clicked');
  }

  onFooterClicked() {
    console.log('App component: Panel footer clicked');
  }

}
```

If you compile and run your web application with the `ng serve --open` command,
you should be able to see messages in the console every time a header or footer elements of the panel get clicked.

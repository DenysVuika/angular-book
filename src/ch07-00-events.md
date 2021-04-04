# Events

There are three main event cases we are going to review in this chapter.
With Angular, you can raise Component events, DOM events and Service events.

> **Source Code**
>
> You can find the source code in the **[angular/events](https://github.com/DenysVuika/developing-with-angular/tree/master/angular/events)** folder.

To address all three scenarios let's create a simple `Panel` component that consists of the `Body`, `Header` and `Footer`.
The Header and Footer are going to be separate components.

First, generate a new Angular application using the Angular CLI tool.
Then execute the following commands to generate the prerequisites:

```sh
ng g component panel
ng g component panel-header
ng g component panel-footer
```

Next, update the `Panel` component template like in the example below:

```html
<!-- src/app/panel/panel.component.html -->

<app-panel-header></app-panel-header>

<p>
  panel works!
</p>

<app-panel-footer></app-panel-footer>
```

Finally, we can replace the default auto-generated content of the main application template
with the `Panel` we have just created above:

```html
<!-- src/app/app.component.html -->

<app-panel>
</app-panel>
```

If you make a pause at this point and run the `ng serve --open` command you should see the following output on the main page:

```text
panel-header works!

panel works!

panel-footer works!
```

At this point, we got all the basic prerequisites for testing the events.

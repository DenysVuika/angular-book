## Preparing a Project

First, let's use Angular CLI tool and generate a new project called "dependency-injection".

```sh
ng new dependency-injection
cd dependency-injection
```

Next, generate two components `component1` and `component2`, as in the example below:

```sh
ng g component component1
ng g component component2
```

Finally, update the main application component template to use both components we have just created:

```html
<!-- src/app/app.component.html -->

<app-component1></app-component1>
<app-component2></app-component2>
```

If you now run the application with the `ng serve --open` command you should see two default component templates
that get generated automatically by Angular CLI:

```text
component1 works!
component2 works!
```

You now have a working project ready for DI experiments.

## Creating a Simple Component

Let's build a simple Angular component in the new "basic-components" project.

The easiest and quickest way to prepare a project structure is using Angular CLI to setup scaffold.

> **Angular CLI**
>
> You can find detailed information on setting up project scaffolds in the **[Angular CLI](ch04-00-angular-cli.md)** chapter.

You start creating a component with importing the `@Component` decorator from the `@angular/core`:

```ts
import { Component } from '@angular/core';
```

The `@Component` decorator supports multiple properties,
and we are going review them in the **[Component Metadata](ch05-04-component-metadata.md)** section later in this chapter.
For the bare minimum, you need to set the "selector" and "template" values to create a basic reusable component.

Our minimal "Header" component implementation can look in practice like in the following example.

```ts
// src/app/components/header.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  template: '<div>{{ title }}</div>'
})
export class HeaderComponent {

  title: string = 'Header';

}
```

You set the selector value to 'app-header'. That means you are registering a new HTML element called `<app-header>`.

You also set a "template" property that holds the inline HTML template string.
At run time the Header element is to be rendered as a `<div>` element with its inner text bound to the "title" property of the "HeaderComponent" class.

Note that before using Header component within an application, we need to register it within the main application module.

> **Application Module**
>
> You are going to get more detailed information on Angular modules in a separate **[Modules](#modules)** chapter.
>
> For now, it is good for you to have just a basic understanding of how components get registered within modules.

Below you can see an example of how typically register a component.
For the sake of simplicity, we are going to see only newly added content.

```ts
// src/app/app.component.ts
// ...
import { HeaderComponent } from './components/header.component';

@NgModule({
  declarations: [
    // ...
    HeaderComponent
  ],
  // ...
})
export class AppModule { }
```

Finally, to test your component just put the following content into the main application template HTML file.

```html
<!-- src/app/app.component.template -->
<app-header></app-header>
```

Once you compile and run your web application, you should see the following content on the main page.

```text
Header
```

Congratulations, you have just created a basic Angular component that you can now reuse across all your web application.

> **Source code**
>
> You can find the source code as an Angular CLI project in the **[angular/components/basic-components](https://github.com/DenysVuika/developing-with-angular/tree/master/angular/components/basic-components)** folder.

## Generating Components with Angular CLI

Now let's try creating another component utilizing the Angular CLI.

This time we are going to create a Footer element.
That should give a good comparison on manual versus automatically generated approaches.

Using the command line prompt execute the following command in the project root directory:

```sh
ng g component components/footer
```

You should instantly notice how Angular CLI saves your time.
It creates you a full set of files for your Footer component and event modifies the main application module file for you.

You can check the console output below:

```text
installing component
  create src/app/components/footer/footer.component.css
  create src/app/components/footer/footer.component.html
  create src/app/components/footer/footer.component.spec.ts
  create src/app/components/footer/footer.component.ts
  update src/app/app.module.ts
```

> **Angular CLI**
>
> You can find detailed information on blueprints and content generation in the [4. Angular CLI](ch04-00-angular-cli.md) chapter.

As a result, you get an initial component implementation with an external HTML and CSS templates and even a unit test scaffold.

```ts
//src/app/components/footer/footer.component.ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-footer',
  templateUrl: './footer.component.html',
  styleUrls: ['./footer.component.css']
})
export class FooterComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

Finally, you can update your main application template to see both header and footer elements in action:

```html
<!-- src/app/app.component.html -->
<app-header></app-header>
<app-footer></app-footer>
```

Upon compiling the application and reloading the page, you should see the following output.

```text
Header
footer works!
```

Note how Angular CLI provides a content of the automatically generated Footer element.

So as you can from the examples above, you save an enormous amount of time when using Angular CLI when it comes to scaffold generation.

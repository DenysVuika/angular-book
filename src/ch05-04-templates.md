## Templates

There are two ways to assign a component view template: inline-defined and external file.

### Inline Templates

You specify inline template by setting the "template" field value of the `@Component` decorator.
To get better formatting and multi-line support, you can use template literals feature introduced in ES6 and backed by TypeScript out of the box.

> **Template Literals**
>
> Template literals are string literals allowing embedded expressions.
> You can use multi-line strings and string interpolation features with them.
>
> For more detailed information on this ES6 feature, please refer to the following **[Template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)** article.

Most of the modern IDEs already have support for mixed content, including TypeScript files.
If you are using [Visual Studio Code](https://code.visualstudio.com/) for development,
then you already have syntax highlighting for HTML and CSS.

Let's edit the Header component template to take multiple lines like in the example below:

```ts
// src/app/components/header.component.ts
@Component({
  selector: 'app-header',
  template: `
    <div>
        <div>{{ title }}</div>
    </div>
  `
})
export class HeaderComponent {
  title: string = 'Header';
}
```

Using backtick characters also allows you to have single and double quotes in HTML without any additional escaping or string concatenation.
You are using the same HTML content inlined as you would have in separate files.

Typically you may want to use inline templates only when your component view is small.

### External Templates

The HTML code in the templates usually grows over time and becomes less maintainable.
That is why storing HTML in the separate files may be more practical and productive.

The `@Component` decorator provides support for external templates through the "templateUrl" option.
Please note that you should set only "template" or "templateUrl", you cannot define both of them at the same time.

Let's now update our Header component we created earlier and move its HTML template to a separate file.

```html
<!-- src/app/components/header.component.html -->
<div>
    <div>{{ title }}</div>
</div>
```

The `templateUrl` property should always point to a path relative to the component class file.
In our case, we put both files together in the same directory and update decorator declaration accordingly.

```ts
// src/app/components/header.component.ts
@Component({
  selector: 'app-header',
  templateUrl: './header.component.html'
})
export class HeaderComponent {
  title: string = 'Header';
}
```

Typically developers put them next to the component class implementation and give the same name for the file as the component:

- header.component.html
- header.component.ts

> **External Files**
>
> External files are the most convenient and recommended way of storing your components' HTML templates.

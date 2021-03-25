## Creating Modules

The Angular CLI tool also provides support for multiple modules
and generating entities that belong to the particular module.

Let's start by generating a new module using the next command:

```sh
ng g module my-components
```

The output in the console should look similar to the following:

```text
create src/app/my-components/my-components.module.ts (196 bytes)
```

And the content of the module contains a basic implementation like in the example below:

```ts
// src/app/my-components/my-components.module.ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: []
})
export class MyComponentsModule { }
```

Note that by default Angular creates a folder for your module, similar to what it does for components.
This is handy once you create components, services, directives and pipes that need to belong to this module.
But if you want to put the resulting module in a single file next to the "app.module.ts" use the "--flat" switch.

```sh
ng g module my-components --flat
```

In that case, the output will be:

```text
create src/app/my-components.module.ts (196 bytes)
```

You can check more details on the available switches by running the "ng g module --help" command.

### Assigning components to modules

By default, Angular CLI appends all generated content to the main application module inside "app.module.ts".
Once you have two or more modules in the application, the CLI will require the module name for every new content.

Try running the following command to see what happens:

```sh
ng g component my-button-1
```

The output should be similar to the following one:

```text
Error: More than one module matches.
Use skip-import option to skip importing the component into the closest module.
```

To include your new component into a particular module use the "--module" switch.
If you are building a shared module, you might also use the "--export" switch,
so that module exports your component besides declaration.

```sh
ng g component my-button-1 --module=my-components --export
```

This time, you will get the following result:

```text
  create src/app/my-button-1/my-button-1.component.css (0 bytes)
  create src/app/my-button-1/my-button-1.component.html (30 bytes)
  create src/app/my-button-1/my-button-1.component.spec.ts (651 bytes)
  create src/app/my-button-1/my-button-1.component.ts (287 bytes)
  update src/app/my-components.module.ts (321 bytes)
```

And the module content now looks like in the code example below:

```ts
// src/app/my-components.module.ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { MyButton1Component } from './my-button-1/my-button-1.component';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: [MyButton1Component],
  exports: [MyButton1Component]
})
export class MyComponentsModule { }
```

Do not forget to check the "ng g component --help" to see all available options.

You can also include your new module into some other existing module from the command line.

```sh
ng g module my-feature --module=my-components --flat
```

As a result, the "MyComponentsModule" module will include "MyFeatureModule":

```ts
// src/app/my-components.module.ts

@NgModule({
  imports: [
    CommonModule,
    MyFeatureModule
  ],
  declarations: [MyButton1Component],
  exports: [MyButton1Component]
})
export class MyComponentsModule { }
```

## Creating Translate Pipe

The pipe we have generated at the beginning of this chapter needs to inject
the translation service instance in the constructor like in the code below:

```ts
// src/app/translate.pipe.ts

import { Pipe, PipeTransform } from '@angular/core';
import { TranslateService } from './translate.service';

@Pipe({
  name: 'translate'
})
export class TranslatePipe implements PipeTransform {

  constructor(private translate: TranslateService) {}

  transform(value: any, args?: any): any {
    return null;
  }

}
```

Now the pipe needs to map the key to the underlying resource string.

Let's get also provide a fallback behavior and return the resource key if the corresponding translation is missing.
That should help you to identify the problems with translation earlier.

```ts
// src/app/translate.pipe.ts

@Pipe({/*...*/})
export class TranslatePipe implements PipeTransform {
  // ...

  transform(key: any): any {
    return this.translate.data[key] || key;
  }

}
```

Don't forget that your newly created pipe needs to be present in the main application module.
The Angular CLI tool automatically registers it, if you have added the pipe manually then see the following code for example:

```ts
// src/app/app.module.ts

import { TranslatePipe } from './translate.pipe';

@NgModule({
  declarations: [
    // ...,
    
    TranslatePipe
  ],
  
  // ...
})
export class AppModule { }
```

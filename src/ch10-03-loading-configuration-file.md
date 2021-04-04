## Loading Server-Side Configuration File

Let's introduce the `load` method that fetches the `app.config.json` file and exposes its content as a `data` property.

For the convenience purposes, the method is going to return a `Promise` instance
that resolves to the loaded configuration file or falls back to the default values upon loading errors.
We also provide support for the optional default values that you can pass as part of the `load` call.

Below you can see the full code for the service implementation:

```ts
// src/app/app-config.service.ts

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AppConfig } from './app-config';

@Injectable({ providedIn: 'root' })
export class AppConfigService {

  data: AppConfig = {};

  constructor(private http: HttpClient) {}

  load(defaults?: AppConfig): Promise<AppConfig> {
    return new Promise<AppConfig>(resolve => {
      this.http.get('app.config.json').subscribe(
        response => {
          console.log('using server-side configuration');
          this.data = Object.assign({}, defaults || {}, response || {});
          resolve(this.data);
        },
        () => {
          console.log('using default configuration');
          this.data = Object.assign({}, defaults || {});
          resolve(this.data);
        }
      );
    });
  }

}
```

As you can see from the code above, we also add a basic debugging output to the browser console log.
It should help us to see whether the application is using an external configuration file or fallback values.

Logging to console is a fully optional step, and you may want to remove that code at the later stages.

## Creating the Configuration Service

We now need two things to be created: an `AppConfig` interface to provide a type-safe access to our configuration properties,
and `AppConfigService` service that is going to load the remote file, store the resulting settings
and expose them to external components and services.

Use the following commands to generate both an interface and a service scaffolds:

```sh
ng g interface app-config
ng g service app-config
```

> Don't forget to register the newly generated service with the main application module,
> as Angular CLI does not do that by default.

Now, edit the `AppConfig` interface and add the "title" property we have declared
in the server-side config earlier in this chapter:

```ts
// src/app/app-config.ts

export interface AppConfig {

  title?: string;

}
```

Next, let's wire our `AppConfigService` with the `Http` service to be able making HTTP calls and fetching remote files:

```ts
// src/app/app-config.service.ts

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({ providedIn: 'root' })
export class AppConfigService {

  constructor(private http: HttpClient) {}

}
```

We are now ready to load the configuration from the server.

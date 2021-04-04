## Service Events

When working with events in Angular, you can achieve a significant level of flexibility
by utilizing the application services and service events.

Service-based events allow multiple components to communicate with each other
regardless of the component and DOM structure using the publish/subscribe approach.

Before we dive into details let's use Angular CLI and generate a new service using the following command:

```sh
ng g service panel
```

You need to manually register the newly generated service within one of your modules.
For now, let's add the `PanelService` to the main application module in the `app.module.ts` file:

```ts
// src/app/app.module.ts

import { PanelService } from './panel.service';

@NgModule({
  providers: [
    PanelService
  ]
})
export class AppModule { }
```

Next, extend the service with a couple of events for header and footer clicks:

```ts
// src/app/panel.service.ts

import { Injectable } from '@angular/core';
import { Subject } from 'rxjs/Rx';

import { PanelHeaderComponent } from './panel-header/panel-header.component';
import { PanelFooterComponent } from './panel-footer/panel-footer.component';

@Injectable({ providedIn: 'root' })
export class PanelService {

  headerClicked = new Subject<PanelHeaderComponent>();
  footerClicked = new Subject<PanelFooterComponent>();

}
```

In the example above, we are using generic `Subject<T>` to allow both emitting and subscribing to the same event.
We are going to pass either `PanelHeaderComponent` or `PanelFooterComponent` instance as the event argument.

Let's update the `PanelHeaderComponent` class and emit the `headerClicked` event like in the following example:

```ts
// src/app/panel-header/panel-header.component.ts

import { PanelService } from '../panel.service';

@Component({/*...*/})
export class PanelHeaderComponent {

  constructor(
    private panelService: PanelService,
    private elementRef: ElementRef) {
  }

  onContentClicked() {
    // ...
    
    // raise service event
    this.panelService.headerClicked.next(this);
  }
}
```

As you can see, the component now injects the `PanelService` instance
and saves a reference to the private `panelService` property so that click handler can use to emit the corresponding event.

Subscribing to the event is also simple.
The component, in our case main application one, injects the `PanelService`
and uses `headerClicked.subscribe` to wire the event handler code:

```ts
// src/app/app.component.ts

import { PanelHeaderComponent } from './panel-header/panel-header.component';
import { PanelService } from './panel.service';

@Component({/*...*/})
export class AppComponent {

  constructor(panelService: PanelService) {
    panelService.headerClicked.subscribe(
      (header: PanelHeaderComponent) => {
        console.log(`Header clicked: ${header.content}`);
      }
    );
  }
}
```

Now if you run your web application and click the header you should see the following output in the browser console:

```text
Header clicked: My panel header
```

Congratulations, you have just established a basic communication channel between header
and footer components with the rest of the application content.

Many other components and services now can subscribe and react to click events.
Of course, we got over-simplified examples; you can imagine more complex scenarios involving different events in your application.

> **Source Code**
>
> You can find the source code in the **[angular/events](https://github.com/DenysVuika/developing-with-angular/tree/master/angular/events)** folder.

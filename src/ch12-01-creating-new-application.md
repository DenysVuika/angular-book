## Creating New Application

Let's use Angular CLI to create a new application and call it `ng-framework`.
You can use that application to test components before building and publishing them to NPM, or redistributing via other resources.

```sh
ng new ng-framework
```

For convenience purposes, you can change the "start" script to also launch the application in the browser.
To do that, open the `package.json` file and append `--open` argument to the `start` command:

```json
{
    "scripts": {
        "start": "ng serve --open"
    }
}
```

Next, you need to install the `ng-packagr` library as a development dependency.

```sh
npm install ng-packagr --save-dev
```

At this point, the project setup is complete, and you are ready to start building reusable component libraries.

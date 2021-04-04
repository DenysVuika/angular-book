## Preparing the Configuration File

We are going to use the JSON format for configuration files. The name of the file can be anything as we need only the content.

As a first step, create an `app.config.json` file in the `src` folder with the following content:

**src/app.config.json**:

```json
{
  "title": "My application"
}
```

> **Configuration Content**
>
> Theoretically, you could even dynamically generate the file if necessary,
> for example building the output based on some database values.

In our case, we just store the custom application title in the config file to see the basic flow in action.
You can extend the file content later on with more properties and values.

We also need configuring Angular CLI to copy the newly created file to the application output directory upon every build.
Edit the `apps` section of the `angular.json` file and append the name of the settings file
to the "assets" collection like in the example below:

**angular.json**:

```json
{
    "apps": {
        "assets": [
            "app.config.json"
        ]
    }
}
```

> **Updating File Content**
>
> One of the greatest features of Angular CLI is automatic rebuilding and live-reloading upon file changes.
>
> If you start your application in the development mode, the runner should also be watching for the `app.config.json` changes,
> and automatically reloading your application every time you update your settings.

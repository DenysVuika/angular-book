## Code Linting

Checking code is one of the essential steps.
Angular CLI ships with the TSLint support and predefined set of rules in the `tsconfig.json` file.

```sh
ng lint
```

Default auto-generated project should contain no errors. You should see the following result in the console:

```text
All files pass linting.
```

Let's try to ensure TSLint works as expected by modifying the `/src/app/app.component.ts` file.
Just change single quotes with double quotes like below:

```ts
export class AppComponent {
  title = "app works!";
}
```

Now running `ng lint` should produce next output:

```text
src/app/app.component.ts[9, 11]: " should be '
Lint errors found in the listed files.
```

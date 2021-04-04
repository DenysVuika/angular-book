# Reusable Component Libraries

We have already used the Angular CLI to generate applications.
For the time being the CLI does not support creating the reusable libraries,
so we are going to use "[ng-packagr](https://github.com/dherges/ng-packagr)" tool for this purpose.

The "ng-packagr" transpiles your component libraries
using the "[Angular Package Format](https://docs.google.com/document/d/1CZC2rcpxffTDfRDs6p1cfbmKNLA6x5O-NtkJglDaBVs/preview)".
Resulting bundles are ready to be consumed by Angular CLI, Webpack, or SystemJS.
For more details on the features, please refer to the project page.

In this chapter, we are going to use Angular CLI to create a new Angular application and two reusable component libraries:
a header and a sidebar components. Both libraries can be built and published to NPM separately.

Also, the application is going to be linked with the component libraries locally.
That allows developing and testing components with the main application.

## Generating Standalone Scripts

The Angular CLI provides a special feature that allows detaching command line tools from the project,
and generating a set of scripts needed for standalone project compilation and testing:

```sh
ng eject
```

Which is an equivalent for `ng eject -dev` or `ng eject --target=development`,
and instructs `ng` tool to use `development` configuration.
Alternatively, you can use `-prod` or `--target=production` switches to enable `production` mode.

Upon running `eject` command, the CLI will:

- update `package.json` with all dependencies needed to compile project without extra tools
- generate and output the proper webpack configuration (`webpack.config.js`) and scripts for testing

The tool might provide additional notes in the console output like below:

```text
==============================================================
Ejection was successful.

To run your builds, you now need to do the following commands:
   - "npm run build" to build.
   - "npm run test" to run unit tests.
   - "npm start" to serve the app using webpack-dev-server.
   - "npm run e2e" to run protractor.

Running the equivalent CLI commands results in error.

==============================================================
Some packages were added. Please run "npm install".
```

Now `scripts` section in your `package.json` file should link to local content for a `start`, `build` and various `test` scripts:

```json
{
    "scripts": {
        "ng": "ng",
        "start": "webpack-dev-server --port=4200",
        "build": "webpack",
        "test": "karma start ./karma.conf.js",
        "lint": "ng lint",
        "e2e": "protractor ./protractor.conf.js",
        "prepree2e": "npm start",
        "pree2e": "webdriver-manager update --standalone false --gecko false --quiet"
    }
}
```

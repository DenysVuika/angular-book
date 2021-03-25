## Development and Production builds

The Angular CLI supports producing both `development` and `production` using the `build` command:

```sh
ng build
```

The format of the command is:

```sh
ng build <options...>
```

By default it is running in `development` mode (an equivalent of `ng build -dev`) and produces output to the `dist/` folder.
You will get bundles together with source maps for better debugging:

| File | Size |
| --- | --- |
| favicon.ico | 5430 |
| index.html | 613 |
| inline.bundle.js | 5764 |
| inline.bundle.js.map | 5824 |
| main.bundle.js | 6539 |
| main.bundle.js.map | 3817 |
| polyfills.bundle.js | 169209 |
| polyfills.bundle.js.map | 204535 |
| styles.bundle.js | 10039 |
| styles.bundle.js.map | 13372 |
| vendor.bundle.js | 2884505 |
| vendor.bundle.js.map | 3081499 |

For production purposes you will want using the following command:

```sh
ng build -prod
```

Which is an equivalent of the:

```sh
ng build --target=production
```

This will give you much smaller output:

| File | Size |
| --- | --- |
| favicon.ico | 5430 |
| inline.d72284a6a83444350a39.bundle.js | 1460 |
| main.e088c8ce83e51568eb21.bundle.js | 12163 |
| polyfills.f52c146b4f7d1751829e.bundle.js | 58138 |
| styles.d41d8cd98f00b204e980.bundle.css | 0 |
| vendor.a2da17b9c49cdce7678a.bundle.js | 362975 |

Please note that `styles` bundle will be empty because by default newly generated app has `src/styles.css` file empty.

The `ng` tool removes `dist` folder between the builds so you should not worry about files left from previous builds and modes.

The content of the `dist` folder is everything you need to deploy your application to the remote server.
You can also use any web server of your choice to run the application in production.

For example:

- Nginx server
- Tomcat server
- IIS server
- and many others

In addition, you can deploy your application to any static pages host, like:

- [GitHub pages](https://pages.github.com/)
- [Netlify](https://www.netlify.com/)
- and many others

It is still possible to use Angular CLI and embedded development server to test production builds.
You can use the following command to build the app in production mode and then run and open default browser to check it:

```sh
ng serve --prod --open
```

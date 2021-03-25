## Running Application

Now switch to the newly generated `my-first-app` folder and launch the app:

```sh
cd my-first-app/
ng serve
```

The `ng serve` command compiles and serves entire project using `webpack` bundler with an output similar to following:

```text
** NG Live Development Server is running on http://localhost:4200 **
Hash: 2c5e702e0dbbc24e055c
Time: 10564ms
chunk    {0} polyfills.bundle.js, polyfills.bundle.js.map (polyfills) 158 kB {4} [initial] [rendered]
chunk    {1} main.bundle.js, main.bundle.js.map (main) 3.62 kB {3} [initial] [rendered]
chunk    {2} styles.bundle.js, styles.bundle.js.map (styles) 9.77 kB {4} [initial] [rendered]
chunk    {3} vendor.bundle.js, vendor.bundle.js.map (vendor) 2.37 MB [initial] [rendered]
chunk    {4} inline.bundle.js, inline.bundle.js.map (inline) 0 bytes [entry] [rendered]
webpack: Compiled successfully.
```

It is important to note that with `ng serve` you are going to run your project with `live development server`.
The server is going to watch for code changes, rebuild all affected bundles and reload the browser.

Now if you navigate to `http://localhost:4200` you should see the following default text:

```text
app works!
```

Alternatively, you can run `serve` command with the `--open` switch to automatically open system browser with the application once compilation is complete:

```sh
ng serve --open
```

It is also possible configuring default `host` and `port` settings:

```sh
ng serve --host 0.0.0.0 --port 3000
```

The command above allows accessing your application from the local machine and local network via port 3000.

There are plenty of options and switches that can be used with `ng serve` command; you can refer to full details by calling `ng help`.

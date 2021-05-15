## Testing in a Container

It is now an excellent time to test our image in a container.
Use the following command to create a temporary container out of our image, and run the application at port 3000:

```sh
docker container run -p 3000:80 --rm ng-docker
```

Once you stop the process with `Ctrl+C`, the Docker is going to perform a cleanup.

Running the container should not take much time. If you now visit the `http://localhost:3000/` in your browser,
you should see the Angular CLI application up and running.

Note that the log output gets redirected to your console.
You should see the `nginx` output if you switch to the console right now:

```text
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET / HTTP/1.1" 200 611 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET /inline.bundle.js HTTP/1.1" 200 1863 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET /polyfills.bundle.js HTTP/1.1" 200 50339 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET /styles.bundle.js HTTP/1.1" 200 3930 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET /vendor.bundle.js HTTP/1.1" 200 492190 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
172.17.0.1 - - [16/Dec/2017:11:41:33 +0000] "GET /main.bundle.js HTTP/1.1" 200 2526 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:57.0) Gecko/20100101 Firefox/57.0"
```

You can now stop the process and let Docker cleanup the data, or continue experimenting with the application.

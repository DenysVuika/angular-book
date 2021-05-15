## Publishing to Docker Hub

In this section, we are going to publish our application image to the public [Docker Hub](https://hub.docker.com/).
You can create a new account if you do not yet have one, this takes a couple of minutes.

If you clone a new copy of the project to publish it directly to the Docker Hub, don't forget to install dependencies.
In all the cases you should also create a fresh build of the application
to be sure the resulting image contains all the latest source code changes.

```sh
npm install
npm run build
```

Let's now build the image and tag it for publishing:

```sh
docker image build -t account/ng-docker:1.0 .
```

Note that typically you are going to replace the `account` prefix with your account name.

To publish the image run the next command with your account name instead of the "account" prefix:

```sh
docker push account/ng-docker:1.0
```

In less than a minute your image should be published and available online.

## Docker Compose

> For more details on the Docker Compose,
> please refer to the [Overview of Docker Compose](https://docs.docker.com/compose/) article.

For the next step, let's create a simple Compose file with our Angular application.

```yml
version: '3.1'

services:
    app:
        image: 'ng-docker'
        build: '.'
        ports:
            - 3000:80
```

Note that we have put the `build` parameter so that Docker builds our local image instead of pulling it from the repository.
The Application runs on port 3000 and maps to port 80 inside the container.

You can now run the following command to test the container and docker compose file:

```sh
docker-compose up
```

The console output should be similar to the following:

```text
Creating network "ngdocker_default" with the default driver
Creating ngdocker_app_1 ...
Creating ngdocker_app_1 ... done
Attaching to ngdocker_app_1
```

Once again, visit the `http://localhost:3000` address and ensure the application is up and running.

As soon as you are done testing, press `Ctrl+C` to stop the process,
and run the next command if you want to perform a cleanup operation:

```sh
docker-compose down
```

The Docker cleans only the containers created by our docker-compose file.
Add the `--rmi all` parameter if you want to remove the images as well.

```sh
docker-compose down --rmi all
```

The console output, in this case, should be similar to the example below:

```text
Removing ngdocker_app_1 ... done
Removing network ngdocker_default
Removing image ng-docker
```

You now need to publish your image to the docker hub to allow other people use your docker-compose file
or build their custom containers with your Angular application image.

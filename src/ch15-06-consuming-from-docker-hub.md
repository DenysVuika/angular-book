## Consuming from Docker Hub

You have successfully published your Angular application image to the Docker Hub,
and before testing it out locally, you should remove the one created earlier before publishing.

Please use the following command to remove the existing image:

```sh
docker image rm account/ng-docker:1.0
```

Now let's create a temporary container and pull the image from the public repository.
Replace the `account` prefix with your Docker Hub account name.

```sh
docker container run -p 3000:80 --rm account/ng-docker:1.0
```

This time you should see Docker downloading and unpacking your image from the internet.
Once the setup is over, visit the `http://localhost:3000` and ensure the application is available and running fine.

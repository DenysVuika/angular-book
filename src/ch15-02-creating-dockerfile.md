## Creating Dockerfile

First of all, you need to build an application to get the "dist" folder with the content ready to redistribute.

```sh
npm run build
```

In the project root, create a file named "Dockerfile" with the following content:

**Dockerfile**:

```text
FROM nginx

COPY nginx.conf /etc/nginx/nginx.conf

WORKDIR /usr/share/nginx/html
COPY dist/ .
```

The image extends the public [nginx](https://hub.docker.com/_/nginx/) one.
Besides, we provide an external configuration to serve our application and copy the contents of the `dist` folder into the image.

The minimal `nginx` configuration can be as following:

**nginx.conf**:

```text
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    server {
        listen 80;
        server_name  localhost;

        root   /usr/share/nginx/html;
        index  index.html index.htm;
        include /etc/nginx/mime.types;

        gzip on;
        gzip_min_length 1000;
        gzip_proxied expired no-cache no-store private auth;
        gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;

        location / {
            try_files $uri $uri/ /index.html;
        }
    }
}
```

> **Deployment**
>
> There are multiple deployment scenarios documented in the official documentation: "[Deployment](https://angular.io/guide/deployment)".
> Please refer to that article if you want to get more information on available options and best practices.

Now, let's build the image using the next command:

```sh
docker image build -t ng-docker .
```

Note the dot character at the end of the command as it is essential.

You can also list your local images to ensure the `ng-docker` got created successfully.

```sh
docker image ls
```

Excluding the images you may already have created or pulled, you should see the at least the following output:

| REPOSITORY | TAG | IMAGE ID | CREATED | SIZE |
| --- | --- | --- | --- | --- |
| ng-docker | latest | 98b129bff2fc | 24 seconds ago | 114MB |

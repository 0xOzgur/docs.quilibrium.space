# Running With Docker

### Build

The only requirements are `git` (to checkout the repository) and docker (to build the image and run the container). Golang does not have to be installed, the docker image build process uses a build stage that provides the correct Go environment and compiles the node down to one command.

In the repository root folder, where the [Dockerfile](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/Dockerfile) file is, build the docker image:

```
docker build --build-arg GIT_COMMIT=$(git log -1 --format=%h) -t quilibrium -t quilibrium:1.4.16 .
```

Use latest version instead of `1.4.16`.

The image that is built is light and safe. It is based on Alpine Linux with the Quilibrium node binary, not the source code, nor the Go development environment. The image also has the `grpcurl` tool that can be used to query the gRPC interface.

### Run

You can run Quilibrium on the same machine where you built the image, from the same repository root folder where [docker-compose.yml](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/docker-compose.yml) is.

You can also copy `docker-compose.yml` to a new folder on a server and run it there. In this case you have to have a way to push your image to a Docker image repo and then pull that image on the server. Github offers such an image repo and a way to push and pull images using special authentication tokens. See [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

Run Quilibrium in a container:

```
docker compose up -d
```

A `.config/` subfolder will be created under the current folder, this is mapped inside the container. Make sure you backup `config.yml` and `keys.yml`.

#### Resource management



To ensure that your client performs optimally within a specific resource configuration, you can specify resource limits and reservations in the node configuration as illustrated below.

This configuration helps in deploying the client with controlled resource usage, such as CPU and memory, to avoid overconsumption of resources in your environment.

The [docker-compose.yml](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/docker-compose.yml) file already specifies resources following the currently recommended hardware requirements.

```
services:
  node:
    # Some other configuration sections here
    deploy:
      resources:
        limits:
          cpus: '4'  # Maximum CPU count that the container can use
          memory: '16G'  # Maximum memory that the container can use
        reservations:
          cpus: '2'  # CPU count that the container initially requests
          memory: '8G'  # Memory that the container initially request
```

#### Customizing docker-compose.yml

If you want to change certain parameters in [docker-compose.yml](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/docker-compose.yml) it is better not to edit the file directly as new versions pushed through git would overwrite your changes. A more flexible solution is to create another file called `docker-compose.override.yml` right next to it and specifying the necessary overriding changes there.

For example:

```
services:
  node:
    image: ghcr.io/mscurtescu/ceremonyclient
    restart: on-failure:7
```

The above will override the image name and also the restart policy.

To check if your overrides are being picked up run the following command:

```
docker compose config
```

This will output the merged and canonical compose file that will be used to run the container(s).

### Interact with a running container

Drop into a shell inside a running container:

```
docker compose exec -it node sh
```

Watch the logs:

```
docker compose logs -f
```

Get the node related info (peer id, version, max frame and balance):

```
docker compose exec node node -node-info
```

Run the DB console:

```
docker compose exec node node -db-console
```

Run the Quilibrium client:

```
docker compose exec node qclient help
docker compose exec node qclient token help
docker compose exec node qclient token balance
```

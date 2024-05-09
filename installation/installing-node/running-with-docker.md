# Running With Docker

## Prepare Requirements

Before you can install Docker Engine, you need to uninstall any conflicting packages.

Distro maintainers provide unofficial distributions of Docker packages in APT. You must uninstall these packages before you can install the official version of Docker Engine.

The unofficial packages to uninstall are:

* `docker.io`
* `docker-compose`
* `docker-compose-v2`
* `docker-doc`
* `podman-docker`

Run the following command to uninstall all conflicting packages:

{% code overflow="wrap" lineNumbers="true" %}
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
{% endcode %}

`apt-get` might report that you have none of these packages installed.

Install Docker With

## Install Using the Apt Repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

1.  Set up Docker's `apt` repository.

    {% code overflow="wrap" %}
    ```bash
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    # Add the repository to Apt sources:
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    ```
    {% endcode %}

**Note**

If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use `UBUNTU_CODENAME` instead of `VERSION_CODENAME`.

## **İnstall the Docker packages.**

***

To install the latest version, run:

{% code overflow="wrap" lineNumbers="true" %}
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin doc
```
{% endcode %}

## Build

The only requirements are `git` (to checkout the repository) and docker (to build the image and run the container). Golang does not have to be installed, the docker image build process uses a build stage that provides the correct Go environment and compiles the node down to one command.

In the repository root folder, where the [Dockerfile](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/Dockerfile) file is, build the docker image:

{% code overflow="wrap" lineNumbers="true" %}
```bash
docker build --build-arg GIT_COMMIT=$(git log -1 --format=%h) -t quilibrium -t quilibrium:1.4.16 .
```
{% endcode %}

Use latest version instead of `1.4.16`.

The image that is built is light and safe. It is based on Alpine Linux with the Quilibrium node binary, not the source code, nor the Go development environment. The image also has the `grpcurl` tool that can be used to query the gRPC interface.

## Run

You can run Quilibrium on the same machine where you built the image, from the same repository root folder where [docker-compose.yml](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/docker-compose.yml) is.

You can also copy `docker-compose.yml` to a new folder on a server and run it there. In this case you have to have a way to push your image to a Docker image repo and then pull that image on the server. Github offers such an image repo and a way to push and pull images using special authentication tokens. See [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

Run Quilibrium in a container:

```bash
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

```bash
docker compose config
```

This will output the merged and canonical compose file that will be used to run the container(s).

## Interact with a running container

Drop into a shell inside a running container:

```bash
docker compose exec -it node sh
```

Watch the logs:

```bash
docker compose logs -f
```

Get the node related info (peer id, version, max frame and balance):

```bash
docker compose exec node node -node-info
```

Run the DB console:

```bash
docker compose exec node node -db-console
```

Run the Quilibrium client:

```bash
docker compose exec node qclient help
docker compose exec node qclient token help
docker compose exec node qclient token balance
```

### How to use gRPCurl with a Quilibrium node

The Docker image has `grpcurl` installed, you can use that instance through `docker exec`, for example:

{% code overflow="wrap" lineNumbers="true" %}
```bash
docker compose exec node grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetNodeInfo | jq
```
{% endcode %}

### List Services



List available services:

```bash
grpcurl -plaintext localhost:8337 list
```

Output:

```bash
grpc.reflection.v1.ServerReflection
grpc.reflection.v1alpha.ServerReflection
quilibrium.node.node.pb.NodeService
```

The first two are reflection related services provided by gRPC, the service provided by the Quilibrium Node is `quilibrium.node.node.pb.NodeService`.

### List Methods

List supported method by `NodeService`:

{% code overflow="wrap" %}
```bash
grpcurl -plaintext localhost:8337 list quilibrium.node.node.pb.NodeService
```
{% endcode %}

Output:

```bash
quilibrium.node.node.pb.NodeService.GetFrameInfo
quilibrium.node.node.pb.NodeService.GetFrames
quilibrium.node.node.pb.NodeService.GetNetworkInfo
quilibrium.node.node.pb.NodeService.GetNodeInfo
quilibrium.node.node.pb.NodeService.GetPeerInfo
quilibrium.node.node.pb.NodeService.GetTokenInfo
```

### Describe Method

To get a description of the `GetNodeInfo` method:

{% code overflow="wrap" lineNumbers="true" %}
```bash
grpcurl -plaintext localhost:8337 describe quilibrium.node.node.pb.NodeService.GetNodeInfo
```
{% endcode %}

Output:

```bash
quilibrium.node.node.pb.NodeService.GetNodeInfo is a method:
rpc GetNodeInfo ( .quilibrium.node.node.pb.GetNodeInfoRequest ) returns ( .quilibrium.node.node.pb.NodeInfoResponse );
```

The request and response messages are all defined in the [node.proto](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/node/protobufs/node.proto) source file. Most (not all) request messages are empty.

### GetNodeInfo

How to call `GetNodeInfo`:

{% code overflow="wrap" lineNumbers="true" %}
```bash
grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetNodeInfo
```
{% endcode %}

Output:

```bash
{
  "peerId": "FiBgOwjQo6aam6iQAq1fvc6wJ3xnTXNTzOLDLwFHyBuujw==",
  "maxFrame": "7658"
}
```

As you can see the output has both the peer id and the max frame that the node is at.

### GetTokenInfo

You can call `GetTokenInfo` to see balances:

{% code overflow="wrap" %}
```bash
grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetTokenInfo
```
{% endcode %}

Output:

```bash
{
  "confirmedTokenSupply": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACsuy4PqzEAA=",
  "unconfirmedTokenSupply": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACsuxtRfqwAA=",
  "ownedTokens": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
  "unconfirmedOwnedTokens": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
}
```

Important

The values are encoded. The strings of only As are for sure zero. TODO: determine how to decode.

### GetPeerInfo

Call `GetPeerInfo` to get info about all the nodes:

{% code overflow="wrap" lineNumbers="true" %}
```bash
grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | less
```
{% endcode %}

Output (truncated):

{% code overflow="wrap" %}
```bash
{
  "peerInfo": [
    {
      "peerId": "EiCIlJTLaQ+GE0ElmoCTb1wK/OWfWTzDA9hC1AyAmNNCew==",
      "multiaddrs": [
        ""
      ],
      "maxFrame": "1439",
      "timestamp": "1708984499614",
      "version": "AQIP",
      "signature": "uOXC/WFc3tCsaqJKplle2q/oeUDGlwZf/ZrDdjWqn/Z3ZBUZZy2jwbB78vhFrNsArVuEw+x9eSWAVTq7ugE4ME5bUNUB4+7Gl6BtV8vPNwqh5NWbfNooKbHDNyees6MEi+VsM04wS4wf147SLiGPxicA",
      "publicKey": "gMTEXXTwVMd9JGBTdJ3+eh/qIGuYapU+PGe7kg3HUgdlj4Ff8NBMVhHxoN/dYLnmpaquGOcqgKcA",
      "totalDistance": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAET2HAgKu7BSR42UPY8a2MEk/qOTrCjR9shges9bZvO/Ng=="
    },
    {
      "peerId": "EiA3Xnx1esdF2BJXwEuVFXw+gj5g5ss4ajJBQ3TNXWiAxA==",
      "multiaddrs": [
        ""
      ],
      "maxFrame": "6783",
      "timestamp": "1704564573411",
      "version": "AQIB",
      "signature": "JOJoCRUmzizAlFjf3dXB8wAeEvFXsXCIl66A/sLu4hjIJktYzvZPAc83abLeG3Zm8WHhuhtnHH4AXHitiWGlqwtFbWCRE6TuDFioemJBKhRgwS3bLF3KIC0yWEcBSTM5hgJCDWe7oCSI2NV8n7mufRUA",
      "publicKey": "prlfQeX4gLPLOz87HChd1zOWihxXeoVdUBMdBTjNmSYsYAGnJuMmGDdgZoipZWsrgsEbhnjKfa2A",
      "totalDistance": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHW0BTyd89JROjwc23I0r7qOJ7ROny+L+EWD1n6OHytKKw=="
    },
```
{% endcode %}

The output is extremely long, this is why it is piped to `less`. Also, note that there is a `-max-msg-sz 5000000` flag on the command. By default `grpcurl` limits responses to 4MB and this flag allows larger one.

### Count Nodes

Run this command to count how many nodes are there:

{% code overflow="wrap" lineNumbers="true" %}
```bash
grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | grep peerId | wc -l
```
{% endcode %}

\

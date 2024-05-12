# Running With Docker

## Prepare Requirements

The Docker installation package available in the official Ubuntu repository may not be the latest version. To ensure we get the latest version, we’ll install Docker from the official Docker repository. To do that, we’ll add a new package source, add the GPG key from Docker to ensure the downloads are valid, and then install the package.

First, update your existing list of packages:

```bash
sudo apt update
```

Next, install a few prerequisite packages which let `apt` use packages over HTTPS:

{% code overflow="wrap" lineNumbers="true" %}
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
{% endcode %}

Then add the GPG key for the official Docker repository to your system:

{% code overflow="wrap" lineNumbers="true" %}
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
{% endcode %}

Add the Docker repository to APT sources:

{% code overflow="wrap" lineNumbers="true" fullWidth="false" %}
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
{% endcode %}

Update your existing list of packages again for the addition to be recognized:

```bash
sudo apt update
```

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:

```bash
apt-cache policy docker-ce
```

You’ll see output like this, although the version number for Docker may be different:

Output of apt-cache policy docker-ce

```bash
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.14~3-0~ubuntu-jammy
  Version table:
     5:20.10.14~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
     5:20.10.13~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
```

Notice that `docker-ce` is not installed, but the candidate for installation is from the Docker repository for Ubuntu 22.04 (`jammy`).

Finally, install Docker:

```bash
sudo apt install docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot.

## Build

The only requirements are `git` (to checkout the repository) and docker (to build the image and run the container). Golang does not have to be installed, the docker image build process uses a build stage that provides the correct Go environment and compiles the node down to one command.

```bash
git https://github.com/QuilibriumNetwork/ceremonyclient.git
cd ceremonyclient
```

In the repository root folder, where the [Dockerfile](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/Dockerfile) file is, build the docker image:

{% code overflow="wrap" lineNumbers="true" %}
```bash
docker build --build-arg GIT_COMMIT=$(git log -1 --format=%h) -t quilibrium -t quilibrium:1.4.17 .
```
{% endcode %}

Use latest version instead of `1.4.17`.

The image that is built is light and safe. It is based on Alpine Linux with the Quilibrium node binary, not the source code, nor the Go development environment. The image also has the `grpcurl` tool that can be used to query the gRPC interface.

## Run

You can run Quilibrium on the same machine where you built the image, from the same repository root folder where [docker-compose.yml](https://github.com/QuilibriumNetwork/ceremonyclient/blob/main/docker-compose.yml) is.

You can also copy `docker-compose.yml` to a new folder on a server and run it there. In this case you have to have a way to push your image to a Docker image repo and then pull that image on the server. Github offers such an image repo and a way to push and pull images using special authentication tokens. See [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

Run Quilibrium in a container:

```bash
docker compose up -d
```

A `.config/` subfolder will be created under the current folder, this is mapped inside the container. Make sure you backup `config.yml` and `keys.yml`.

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

## Run the Quilibrium client

```bash
docker compose exec node qclient help
docker compose exec node qclient token help
docker compose exec node qclient token balance
```

## How to use gRPCurl with a Quilibrium node

The Docker image has `grpcurl` installed, you can use that instance through `docker exec`, for example:

{% code overflow="wrap" lineNumbers="true" %}
```bash
docker compose exec node grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetNodeInfo
```
{% endcode %}

### List Services

List available services:

<pre class="language-bash"><code class="lang-bash"><strong>docker compose exec node grpcurl -plaintext localhost:8337 list
</strong></code></pre>

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
docker compose exec node grpcurl -plaintext localhost:8337 list quilibrium.node.node.pb.NodeService
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
docker compose exec node grpcurl -plaintext localhost:8337 describe quilibrium.node.node.pb.NodeService.GetNodeInfo
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
docker compose exec node grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetNodeInfo
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

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>docker compose exec node grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetTokenInfo
</strong></code></pre>

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
docker compose exec node grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | less
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
docker compose exec node grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | grep peerId | wc -l
```
{% endcode %}

\

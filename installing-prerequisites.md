# 💾 Installing Prerequisites

We strongly recommend running your Node via binary after v2.0.1 This documentation does not support running or building nodes using source.

When run with Binary, you do not need to install any additional requirements. But we still share these for those who want to use grpcurl.



The first step is to install the required software.

## **Installing Git**

```bash
sudo  apt -q update
```

```bash
sudo  apt  install  git  -y
```

Check the version with

```bash
git --version
```

It will show your git version:

> git version 2.34.1

## **Installing GO**

You must install Go version 1.24.4.&#x20;

If you are using arm64 system, please apply the commands at Arm 64 System tab below.

{% tabs %}
{% tab title="Amd64 Systems" %}
```bash
wget https://go.dev/dl/go1.22.4.linux-amd64.tar.gz
sudo tar -xvf go1.22.4.linux-amd64.tar.gz
sudo mv go /usr/local
sudo rm go1.22.4.linux-amd64.tar.gz
sudo nano ~/.bashrc
```
{% endtab %}

{% tab title="Arm64 Systems" %}
```bash
wget https://go.dev/dl/go1.22.4.linux-arm64.tar.gz
sudo tar -xvf go1.22.4.linux-arm64.tar.gz
sudo mv go /usr/local
sudo rm go1.22.4.linux-arm64.tar.gz
sudo nano ~/.bashrc
```
{% endtab %}
{% endtabs %}

At the end of the file, add these lines and save the file.

```bash
GOROOT=/usr/local/go
GOPATH=$HOME/go
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

After you save the file, run:

```bash
source ~/.bashrc
```

Check GO Version&#x20;

```bash
go version
```

It must show "go version go.1.22.4 linux/amd64" or  "go version go.1.22.4 linux/arm64" depending on your system.

## Installing gRPCurl

If you already have the GO installed, you can use the `go` tool to install `grpcurl`:

```bash
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

If you are sure that all the requirements are installed, you can now proceed to the [installing node](installation/installing-node.md) section.

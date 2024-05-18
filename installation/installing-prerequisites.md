# 💾 Installing Prerequisites

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

You must strictly install Go version 1.20.14.&#x20;

If you are using arm64 system, please apply the commands at Arm 64 System tab below.

{% tabs %}
{% tab title="Amd64 Systems" %}
```bash
wget https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
sudo tar -xvf go1.20.14.linux-amd64.tar.gz
sudo mv go /usr/local
sudo rm go1.20.14.linux-amd64.tar.gz
sudo nano ~/.bashrc
```
{% endtab %}

{% tab title="Arm64 Systems" %}
```bash
wget https://go.dev/dl/go1.20.14.linux-arm64.tar.gz
sudo tar -xvf go1.20.14.linux-arm64.tar.gz
sudo mv go /usr/local
sudo rm go1.20.14.linux-arm64.tar.gz
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

It must show "go version go.1.20.14 linux/amd64" or  "go version go.1.20.14 linux/arm64" depending on your system.

## Installing gRPCurl

If you already have the GO installed, you can use the `go` tool to install `grpcurl`:

```bash
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

If you are sure that all the requirements are installed, you can now proceed to the [installing node](installing-node/) section.

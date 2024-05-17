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

### **Installing to amd64 systems**

You must strictly install Go version 1.20.14.&#x20;

If you are using arm64 system, please follow [installing to arm64 system](installing-prerequisites.md#installing-to-arm64-systems) link

```bash
wget https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
sudo tar -xvf go1.20.14.linux-amd64.tar.gz
sudo mv go /usr/local
sudo rm go1.20.14.linux-amd64.tar.gz
sudo nano ~/.bashrc
```

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

It must show "go version go.1.20.14 linux/amd64" depending on your system.

### **Installing to arm64 systems**

The installation process is similar to amd64 systems, but you'll need to use the appropriate download links and commands for arm64 architecture.

```
wget https://go.dev/dl/go1.20.14.linux-arm64.tar.gz
sudo tar -xvf go1.20.14.linux-arm64.tar.gz
sudo mv go /usr/local
sudo rm go1.20.14.linux-arm64.tar.gz
sudo nano ~/.bashrc
```

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

It must show "go version go.1.20.14 linux/arm64" depending on your system.

## Installing gRPCurl

If you already have the GO installed, you can use the `go` tool to install `grpcurl`:

```bash
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

If you are sure that all the requirements are installed, you can now proceed to the [installing node](installing-node/) section.

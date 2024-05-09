# Installing Prerequisites

At firs you need to install prerequisites.&#x20;

**Installing Git**

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

It must show:

> git version 2.34.1





**Installing GO**

You must install strictly Go version 1.20.14 for amd64 systems. If you are using arm64 system, you need the change releases links and commands with arm64 instead of amd64

```bash
wget https://:go.dev/dl/go1.20.14.linux-amd64.tar.gz
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


---
description: >-
  Please note that, these instructions would work with V1.5 but later it will be
  updated.
---

# 📀 Upgrading Node

## Manually Upgrade your Q Node to latest release

First, run:

```bash
service ceremonyclient status
```

Press `ctrl` + `c` on your keyboard twice, to exit the status section in the terminal\
If the response says service is Inactive, proceed to git fetch command below.\
If response says service is Active, run:

```bash
service ceremonyclient stop
```

Go to ceremonyclient folder.

```bash
cd ~/ceremonyclient
```

Check whether some files are updated in the ceremonyclient git repo, run:

```bash
git fetch origin
```

If there are files shown that are changed or different from your local copy, run:

```bash
git merge origin
```

Go to ceremonyclient/node folder.

```bash
cd ~/ceremonyclient/node
```

Next, do a `go clean` command to clear all previous build files. Run:

```bash
GOEXPERIMENT=arenas go clean -v -n -a ./...
```

Next, remove the compiled go binary file `node`, run:

```bash
rm /root/go/bin/node
ls /root/go/bin
```

The `ls` command should respond empty\
\
Next, make a new build compiled binary file `node`, run:

```bash
GOEXPERIMENT=arenas  go  install  ./...
```

Verify that the `node` binary is built again, run:

```bash
ls /root/go/bin
```

Response should show

> node

Lastly, start your Q Node via the service command, run:

```bash
service ceremonyclient start
```

## Upgrade via Update Script

Create a file named update.sh in your server and put the code below.

```bash
sudo nano update.sh
```

Copy the code below into update.sh

```bash
#!/bin/bash

# Stop the ceremonyclient service
service ceremonyclient stop

# Switch to the ~/ceremonyclient directory
cd ~/ceremonyclient

# Fetch updates from the remote repository
git fetch origin
git merge origin

# Switch to the ~/ceremonyclient/node directory
cd ~/ceremonyclient/node

# Clean and reinstall node
GOEXPERIMENT=arenas go clean -v -n -a ./...
rm /root/go/bin/node
GOEXPERIMENT=arenas go install ./...

# Start the ceremonyclient service
service ceremonyclient start
```

Save the file and then run;

```bash
chmod u+x update.sh
```

When you need to update your node, you can run update.sh

```bash
./update.sh
```

## Upgrading Docker Node

Go to ceremonyclient folder.

```bash
cd ~/ceremonyclient
```

Stop the node

```
docker compose down
```

Check whether some files are updated in the ceremonyclient git repo, run:

```bash
git fetch origin
```

If there are files shown that are changed or different from your local copy, run:

```bash
git merge origin
```

Build node;

{% code overflow="wrap" %}
```bash
docker build --build-arg GIT_COMMIT=$(git log -1 --format=%h) -t quilibrium -t quilibrium:1.4.17 .
```
{% endcode %}

Use latest version incase version is different than `1.4.17`.

Start the node again

```
docker compose up -d
```

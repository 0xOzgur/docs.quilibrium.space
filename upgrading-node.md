---
description: >-
  Please note that, these instructions would work with V1.5 but later it will be
  updated.
---

# Upgrading Node

### Manual Upgrade your Q Node to latest release

First, run:

```
service ceremonyclient status
```

Press `ctrl` + `c` on your keyboard twice, to exit the status section in the terminal\
If the response says service is Inactive, proceed to git fetch command below.\
If response says service is Active, run:

```
service ceremonyclient stop
```

Go to ceremonyclient folder.

```
cd ~/ceremonyclient
```

Check whether some files are updated in the ceremonyclient git repo, run:

```
git fetch origin
```

If there are files shown that are changed or different from your local copy, run:

```
git merge origin
```

Go to ceremonyclient/node folder.

```
cd ~/ceremonyclient/node
```

Next, do a `go clean` command to clear all previous build files. Run:

```
GOEXPERIMENT=arenas go clean -v -n -a ./...
```

Next, remove the compiled go binary file `node`, run:

```
rm /root/go/bin/node
ls /root/go/bin
```

The `ls` command should respond empty\
\
Next, make a new build compiled binary file `node`, run:

```
GOEXPERIMENT=arenas  go  install  ./...
```

Verify that the `node` binary is built again, run:

```
ls /root/go/bin
```

Response should show

> node

Lastly, start your Q Node via the service command, run:

```
service ceremonyclient start
```

### Upgrading via Update Script

Create a file named update.sh in your server and put the code below.

```
sudo nano update.sh
```

Copy the code below into update.sh

```
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

```
chmod u+x update.sh
```

When you need to update your node, you can run update.sh

```
./update.sh
```


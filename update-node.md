---
description: Please note that, these instructions would upgrade your node to v1.4.21
---

# 📀 Update Node

## Upgrading Service

Note that this update is intended for those who use their nodes as services.

Please follow the commands below.&#x20;

This script will cherck your system architecture and upgrade your node based on that info.

{% code overflow="wrap" %}
```bash
wget -O - https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/update/update.sh | bash
```
{% endcode %}

## Upgrade Tmux or Screen Using Nodes

Make sure to stop your autoscript!&#x20;

Go to node folder:

```bash
cd ~ceremonyclient/node
```

Checkout to the correct branch and run autorun.sh script

```bash
git remote set-url origin https://github.com/QuilibriumNetwork/ceremonyclient.git
git pull
git checkout release 
./release_autorun.sh
```

## Docker Upgrade

Coming soon

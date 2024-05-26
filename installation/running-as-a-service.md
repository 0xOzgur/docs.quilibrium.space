---
description: Installlin Manually v.1.4.18
---

# 💿 Running as a Service

If you've already installed and started your Quilibrium Ceremony Client node, you can convert it to a service for easier management. This simplifies tasks like starting, stopping, and monitoring your node.

Get the node binary files and checkout release branch;

{% code overflow="wrap" %}
```bash
cd ~
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
git pull
git checkout release
```
{% endcode %}

Then we will create service;

```bash
nano /lib/systemd/system/ceremonyclient.service
```

Write the code below;

```bash
[Unit]
Description=Ceremony Client Go App Service

[Service]
Type=simple
Restart=always
RestartSec=5s
WorkingDirectory=/root/ceremonyclient/node
Environment=GOEXPERIMENT=arenas
ExecStart=/root/ceremonyclient/node/node-1.4.18-linux-amd64

[Install]
WantedBy=multi-user.target
```

Save and exit

**Then start the Node**

```bash
service ceremonyclient start
```

This will start your Q Node. While at this stage script will create the `.config` folder inside `~/ceremonyclient/node` - with your  `config.yml` and `keys.yml` .

Please do your backup `config.yml` and `keys.yml`files within `~/ceremonyclient/node/.config` folder.

## Node Commands

To start service, run

```bash
service ceremonyclient start
```

To stop service, run

```bash
service ceremonyclient stop
```

To restar service, run

```bash
service ceremonyclient restart
```

To view service logs run

```bash
sudo journalctl -u ceremonyclient.service -f --no-hostname -o cat
```

## Get the node related info&#x20;

(peer id, version, max frame and balance):

<mark style="color:orange;">Please note that, you must properly configure your node to run all these commands. For details, visit</mark> [<mark style="color:purple;">**Configuration**</mark>](../../configurations.md) <mark style="color:orange;">section.</mark>

See Peer ID:

```bash
cd ~/ceremonyclient/node && ./node-1.4.18-linux-amd64 -peer-id
```

See Node Info:

```bash
cd ~/ceremonyclient/node && ./node-1.4.18-linux-amd64 -node-info
```

Run the DB console:

```bash
cd ~/ceremonyclient/node && ./node-1.4.18-linux-amd64 -db-console
```

Check Balances:

```bash
cd ~/ceremonyclient/node && ./node-1.4.18-linux-amd64 -balance
```

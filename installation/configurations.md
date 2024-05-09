---
description: >-
  Your node and machine needed to be configured to make ceremonycliend node run
  correctly.
---

# Configurations

### Configure your Node Network Firewall  Run:

```bash
sudo ufw enable
```

Type `y` and press `enter` or `return` on keyboard Run:

```bash
sudo ufw allow 22
sudo ufw allow 8336
sudo ufw allow 443
sudo ufw status
```

Response for the status command should be:

```bash
> To            Action            From
> --            ------            -----
> 22            ALLOW             Anywhere
> 8336          ALLOW             Anywhere
> 443           ALLOW             Anywhere
> 22 (v6)       ALLOW             Anywhere (v6)
> 8336 (v6)     ALLOW             Anywhere (v6)
> 443 (v6)      ALLOW             Anywhere (v6)
```

### Configure your config.yml

**1. Enable gRPC to enable gRPC Function Calls for your Node**\
Note: This interface, while read-only, is unauthenticated and not rate-limited. It is recommended that you only enable them if you are properly controlling access via firewall or only query via localhost (i.e. if port 8337 is used for gRPC calls, best not to allow it on your firewall configuration later and only trigger gRPC calls on localhost).\
\
Go to ceremonyclient/node folder.

```bash
cd ~/ceremonyclient/node
```

Run:

```bash
sudo nano .config/config.yml
```

At the end of file, there is a field `listenGrpcMultiaddr: “”`, replace it with

```bash
listenGrpcMultiaddr: /ip4/127.0.0.1/tcp/8337
listenRESTMultiaddr: "/ip4/127.0.0.1/tcp/8338"
```

#### 2. Enable Stats Collection by Opt-In

Go to ceremonyclient/node folder.

```bash
cd ~/ceremonyclient/node
```

Run:

```bash
sudo nano .config/config.yml
```

On the line, right about the middle part of file, there is a field `engine`, append a sub-field called `statsMultiaddr`

```
engine:
  statsMultiaddr: "/dns/stats.quilibrium.com/tcp/443"
```

\

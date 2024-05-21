---
description: Thanks to snapshot provider CherryServers.
---

# 📸 Snapshot

If you are having problems with synchronization, you can install the current snapshot store folder on your system with the commands below.

Thanks to snapshot provider [CherryServers](https://www.cherryservers.com/?affiliate=676XHODW).

```bash
service ceremonyclient stop
apt install unzip
cd ~/ceremonyclient/node/.config
mv store storeold
wget https://snapshots.cherryservers.com/quilibrium/store.zip
unzip store.zip
rm store.zip
rm -rf storeold
service ceremonyclient start
```

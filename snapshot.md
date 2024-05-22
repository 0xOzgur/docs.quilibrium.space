# 📸 Snapshot

If you are having difficulties with synchronization, you can install the current snapshot store folder on your system with the commands below.

Thanks to snapshot provider [CherryServers](https://www.cherryservers.com/?affiliate=676XHODW).

```bash
apt install unzip
cd ~/ceremonyclient/node/.config
wget https://snapshots.cherryservers.com/quilibrium/store.zip
service ceremonyclient stop
mv store storeold
unzip store.zip
rm store.zip
rm -rf storeold
service ceremonyclient start
```

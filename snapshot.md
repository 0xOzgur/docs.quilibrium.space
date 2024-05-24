# 📸 Snapshot

If you are having difficulties with synchronization, you can install the current snapshot store folder on your system with the commands below.

Thanks to snapshot provider [CherryServers](https://www.cherryservers.com/?affiliate=676XHODW).

{% code overflow="wrap" %}
```bash
service ceremonyclient stop
apt install unzip -y
rm -r $HOME/ceremonyclient/node/.config/store && wget -qO- https://snapshots.cherryservers.com/quilibrium/store.zip > /tmp/store.zip && unzip -j -o /tmp/store.zip -d $HOME/ceremonyclient/node/.config/store && rm /tmp/store.zip
service ceremonyclient start
```
{% endcode %}

Check if your  ceremonyclient/node/.config folder has REPAIR file. If you don't have REPAIR file in your .config file, do this please;

```bash
cd ~/ceremonyclient/node/.config
wget https://snapshots.cherryservers.com/quilibrium/repair
mv repair REPAIR
```

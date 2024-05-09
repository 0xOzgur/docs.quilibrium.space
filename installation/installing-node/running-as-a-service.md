# Running as a Service

If you already installed and started node, you can build it as a service to manage easily.

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
ExecStart=/root/go/bin/node ./...

[Install]
WantedBy=multi-user.target
```

Save and exit

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

Get the node related info (peer id, version, max frame and balance):

```bash
cd ~/ceremonyclient/node && GOEXPERIMENT=arenas go run ./... -node-info
```

Run the DB console:

```bash
cd ~/ceremonyclient/node && - GOEXPERIMENT=arenas go run ./... --db-console
```

Check Balances:

```bash
cd ~/ceremonyclient/node && GOEXPERIMENT=arenas go run ./... -balance
```

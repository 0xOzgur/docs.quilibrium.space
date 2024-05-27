# 💽 Installing Node

You must clone latest version of ceremony client.

```bash
cd ~
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
```

**At this stage, we need to decide whether to run our **<mark style="color:red;">**Node s a Service**</mark>** or as a **<mark style="color:red;">**Docker container**</mark>**.**

If you will run it as a service, please follow [**Running as a Service**](running-as-a-service.md) instructions.

If you want to run with Docker, please follow [**Running With Docker**](running-with-docker.md) instructions.

**If you want to run manualy or with tmux session, you can run:**

```bash
cd ~/ceremonyclient/node
git checkout release 
./release_autorun.sh
```

If you have a voucher from the offline ceremony, first run:

Go to the ceremonyclient/node folder

```bash
cd ~/ceremonyclient/node
```

All commands are to be run in the `ceremonyclient/node/` folder.

```bash
GOEXPERIMENT=arenas go run ./... -import-priv-key `cat /path/to/voucher.hex`
```

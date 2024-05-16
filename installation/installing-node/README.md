# 💽 Installing Node

You must clone latest version of ceremony client.

```bash
cd ~
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
```

_If you want to run your node via docker, please follow Installing_ [_Docker instructions_](running-with-docker.md)_._

Go to the ceremonyclient/node folder

```bash
cd ~/ceremonyclient/node
```

All commands are to be run in the `node/` folder.

If you have a voucher from the offline ceremony, first run:

```bash
GOEXPERIMENT=arenas go run ./... -import-priv-key `cat /path/to/voucher.hex`
```

If you do not, or have already run the above, run:

```bash
GOEXPERIMENT=arenas go run ./...
```

This will start your Q Node. While at this stage script will create the `.config` folder inside `~/ceremonyclient/node` - with your  `config.yml` and `keys.yml` .

Please do your backup `config.yml` and `keys.yml`files within `~/ceremonyclient/node/.config` folder.

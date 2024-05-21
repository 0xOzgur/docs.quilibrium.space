---
description: >-
  The development has been carried out by 0xOzgur.eth and you should use it at
  your own risk.
---

# 🤖 AutoInstaller

This page explains how to install ceremonyclient with automatic installation scripts.&#x20;

{% hint style="info" %}
This automatic installation script is especially designed for fresh installations on newly turned on devices.
{% endhint %}

{% tabs %}
{% tab title="Install as a Service" %}
Please copy the command below and sit back while node is being built.

{% code overflow="wrap" %}
```bash
wget https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/install/install_quilibrium_service.sh https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/configuration/config.sh
chmod u+x install_quilibrium_service.sh config.sh
./install_quilibrium_service.sh
```
{% endcode %}

Please wait until setup finish. When you see ceremonyclient logs let it run for 5 minutes.&#x20;

After that press CTRL+C then apply this command:

```
source ~/.bashrc
```

Lastly run your config script to make your grpcurl enabled

```
./config.sh
```

{% hint style="info" %}
Please visit [Running as a Service](installation/installing-node/running-as-a-service.md) section for required commands to manage your node.
{% endhint %}
{% endtab %}

{% tab title="Install as a Docker" %}
Please copy the command below and sit back while node is being built.

{% code overflow="wrap" %}
```bash
wget https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/install/install_docker.sh https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/configuration/config_docker.sh
chmod u+x install_docker.sh config_docker.sh
./install_docker.sh
```
{% endcode %}

{% hint style="info" %}
Please visit [Running With Docker](installation/installing-node/running-with-docker.md) section for required commands to manage your node.
{% endhint %}
{% endtab %}
{% endtabs %}

Do not forget to reboot your machine after your installation finish.

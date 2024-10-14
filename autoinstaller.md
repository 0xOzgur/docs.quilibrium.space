---
description: >-
  The development has been carried out by 0xOzgur and you should use it at your
  own risk.
---

# 🤖 AutoInstaller

This page explains how to install ceremonyclient with automatic installation scripts.&#x20;

This script will install latest version.

{% hint style="info" %}
This automatic installation script is especially designed for fresh installations on newly turned on devices.
{% endhint %}

## Install as Service

Please copy the command below and sit back while node is being built.

{% code overflow="wrap" %}
```bash
wget -O - https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/install/install_quilibrium_service.sh | bash
```
{% endcode %}

## Install Docker

The command below will install docker container for your node.

{% code overflow="wrap" %}
```bash
wget -O - https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/main/install/install_docker.sh | bash
```
{% endcode %}

Do not forget to reboot your machine after your installation finish.

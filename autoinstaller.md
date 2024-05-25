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

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>wget -O - https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/v1.4.18/install/install_quilibrium_service.sh | bash
</strong></code></pre>



{% hint style="info" %}
Please visit [Running as a Service](installation/installing-node/running-as-a-service.md) section for required commands to manage your node.
{% endhint %}
{% endtab %}

{% tab title="Install as a Docker" %}
Please copy the command below and sit back while node is being built.

{% code overflow="wrap" %}
```bash
wget -O - https://raw.githubusercontent.com/0xOzgur/QuilibriumTools/v1.4.18/install/install_docker.sh | bash
```
{% endcode %}

{% hint style="info" %}
Please visit [Running With Docker](installation/installing-node/running-with-docker.md) section for required commands to manage your node.
{% endhint %}
{% endtab %}
{% endtabs %}

Do not forget to reboot your machine after your installation finish.

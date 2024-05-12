# Claiming wQuil Token

<mark style="background-color:red;">These updates are not currently accessible on the website. Information regarding the completion of the updates and the release of v1.4.18 will be announced soon.</mark>

## About Wrapped Quil Token

wQuil token is the wrapped version of Quil tokens on the Equilibrium Network and represents a 1:1 ratio. It aims to free the circulation of Quil tokens until the Quilibrium crosschain-bridge is active. Currently, it will only be available on ETH Mainnet, and the Quilibrium crosschain bridge is planned to be opened with V2.

### Token Details

Name:

Ticker:

Network:

Contract Code:

<mark style="color:purple;">Plase add wQuil as custome token to your EVM wallet!</mark>

## Step by Step Claiming

In order to claim your Quil tokens, qClient must be ready for use. Please check the relevant page for [qClient installation](cli-commands.md#installing-qclient).

If qClient is installed, you can claim your wQuil tokens via ETH Mainnet by following the steps below.

<mark style="color:yellow;">Please note that, you will be claiming %100 of your rewards. It is impossible to claim partially.</mark>

1. Visit [Quilibrium.com](https://quilibrium.com)

<figure><img src=".gitbook/assets/1.jpeg" alt=""><figcaption></figcaption></figure>

2. Go to Resulst / Claim Rewards Page and search Peer ID of your node&#x20;

<figure><img src=".gitbook/assets/2.jpeg" alt=""><figcaption></figcaption></figure>

3. Connect your EVM Wallet

<figure><img src=".gitbook/assets/3.jpeg" alt=""><figcaption></figcaption></figure>

4. After connecting your wallet, website will give you cross mint code for qClient.&#x20;

<figure><img src=".gitbook/assets/4.jpeg" alt=""><figcaption></figcaption></figure>

Go to your qClient and past that command.&#x20;

```bash
cd ceremonyclient/client
client cross-mint 0x000000000000000000000000000000
```

When you run that command, please copy output and paste your response as shown at the screenshot.

{% code title="Example" overflow="wrap" %}
```bash
cd ceremonyclient/client
client cross-mint 0x000000000000000000000000000000
result: ("peerPublicKey": "LLP/9tYlqlsV8AgTky|pK9zjN+OfXNmYMDhmDeEagCMAhjfpPPWDyWDq9w6uM19hGyDKYB10EVOA", "peerSignature": "2ybumA9VuSrnr5nYPcjehGo/PK6uNI4¡VaOWXkEGms5ChqPFgOJX6Z5eng8U6VSHy85zbeZBukiANE3j2EBxrk4TAf4Z+5uuNMCQ6DasKpkgsxulOGWKhOcBa|2CDicinuMqafU 3YOrXH9cck/OkivwA""proverPublicKev": "CAk3innisW2Bocar/75/3dwiRSaFMRbhYhtCWd@Th77aDvOWFGaoMXIvKHw3B4+vFsmYlVaQ7/iA" "proverSianature".
```
{% endcode %}



6. One last step, press claim buttun and approve necessary transactions

<figure><img src=".gitbook/assets/5.jpeg" alt=""><figcaption></figcaption></figure>

5. Voila, wQuil tokens will be in your wallet shortly. You can check status of transaction.

<figure><img src=".gitbook/assets/6.jpeg" alt=""><figcaption></figcaption></figure>

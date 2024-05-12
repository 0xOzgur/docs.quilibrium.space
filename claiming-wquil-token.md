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
qclient cross-mint <code that website gave>
```

When you run that command, please copy output and paste your response as shown at the screenshot.

6. One last step, press claim buttun and approve necessary transactions

<figure><img src=".gitbook/assets/5.jpeg" alt=""><figcaption></figcaption></figure>

5. Voila, wQuil tokens will be in your wallet shortly. You can check status of transaction.

<figure><img src=".gitbook/assets/6.jpeg" alt=""><figcaption></figcaption></figure>

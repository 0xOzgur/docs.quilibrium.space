---
description: >-
  With the release of Quilibrium v2, the node application will come with the
  /client folder to have better visibility on your node's conditions, earned
  rewards and perform transfer transactions.
---

# ⌨️ CLI Commands

## Installing qClient

_If you already use Quilibrium for Dummies, qClient must have been installed already._

Before you begin, ensure you have qClient installed on your system.&#x20;

Go inside the client folder

```bash
cd ~/ceremonyclient/client
```

Then download binary of cqlient

```bash
wget https://releases.quilibrium.com/qclient-2.0.0-linux-amd64
chmod u+x qclient-2.0.0-linux-amd64
mv qclient-2.0.0-linux-amd64 qclient


```

{% hint style="info" %}
Please note that, you must downlod cqlient binary depends on your system architecture. There are 3 different binary option based on systems.

<pre class="language-markup" data-line-numbers><code class="lang-markup"><strong>qclient-2.0.0-darwin-arm64
</strong>qclient-2.0.0-linux-arm64
qclient-2.0.0-linux-amd64
</code></pre>
{% endhint %}

## CLI Commands

### 1. General Command Syntax

The CLI tooling itself will be relatively simple, and the commands can be run as follows (assuming a build in the accompanying _/client_ folder rather than `go run ./...`:

{% code overflow="wrap" %}
```bash
./qclient [--config=<other path than ../node/.config/>] <app> <cmd> <param1> <param2> <...>
```
{% endcode %}

### 2. Querying Balance

The command line tool takes arguments in either decimal (xx.xxxxx) format or raw unit (0x00000) format. Note that raw units are a multiple of QUIL: 1 QUIL = 0x1DCD65000 units\
\
Command:

```bash
./qclient token balance
```

Response:

{% code overflow="wrap" %}
```bash
$ ./qclient token balance
50.0 QUIL (Account 0x23c0f371e9faa7be4ffedd616361e0c9aeb776ae4d7f3a37605ecbfa40a55a90)
```
{% endcode %}

### 3. Querying Individual Coins

Users may wish to view the individual coins:\
\
Command:

```bash
./qclient token coins
```

Response:

```bash
$ client token coins
25.0 QUIL (Coin 0x1148092cdce78c721835601ef39f9c2cd8b48b7787cbea032dd3913a4106a58d)
25.0 QUIL (Coin 0x2dda9dc9770a1e5a01974fcd5af2a77147d0f19fb4935a1df677ec6050be0a9e)
```

### 4. Creating a Pending Transaction

Quilibrium's token application has two modes: a two-stage transfer/accept (or reject), or a single-stage mutual transfer.\
\
Command:

```bash
./qclient token transfer <ToAccount> <RefundAccount> <Amount|OfCoin>
```

Response:\
To perform a two-stage transfer, you have two options:

{% code overflow="wrap" %}
```bash
./qclient token transfer <ToAccount> <RefundAccount> <Amount>
<Amount> QUIL (Pending Transaction 0x0382e4da0c7c0133a1b53453b05096272b80c1575c6828d0211c4e371f7c81bb)
```
{% endcode %}

or

{% code overflow="wrap" %}
```bash
./qclient token transfer <ToAccount> <RefundAccount> <OfCoin>
<Amount> QUIL (Pending Transaction 0x0382e4da0c7c0133a1b53453b05096272b80c1575c6828d0211c4e371f7c81bb)
```
{% endcode %}

Omitting the RefundAccount will simply provide your own originating account. The option to specify exists so that you can maintain anonymity when sending by creating a fresh account to receive the refund. The RefundAccount cannot be the same as the ToAccount.

The first is a user-friendly version of a transfer, akin to what account-based networks like Ethereum and Solana do, where you operate on a balance. Behind the scenes, the client is actually splitting and/or merging coins as needed in order to create the requisite amount to send as a discrete coin. The second is an application-aware version of a transfer, akin to what UTXO-based networks like Bitcoin do, where you operate on the raw coin balance under a specific address. If you have good reason to manage coins separately (yet under the control of the same managing account), you will want to use the second option in conjunction with split/merge operations if needed:

{% code overflow="wrap" %}
```bash
./qclient token split <OfCoin> <LeftAmount> <RightAmount>
<LeftAmount> QUIL (Coin 0x024479f49f03dc53fd702198cd9b548c9e96004e19ef6a4e9c5211a9795ba34d)
<RightAmount> QUIL (Coin 0x0140e01731256793bba03914f3844d645fbece26553acdea8ac4de4d84f91690)

./qclient token merge <LeftCoin> <RightCoin>
<Total> QUIL (Coin 0x151f4ae225e20759077e1724e4c5d0feae26c477fd10d728dfea962eec79b83f)
```
{% endcode %}

### 5. Accepting a Pending Transaction

To accept a pending transaction, you simply run:

{% code overflow="wrap" %}
```bash
./qclient token accept <PendingTransaction>
<Amount> QUIL (Coin 0x2688997f2776ab5993894ed04fcdac05577cf2494ddfedf356ebf8bd3de464ab)
```
{% endcode %}

The same applies for rejecting a pending transaction

{% code overflow="wrap" %}
```bash
./qclient token reject <PendingTransaction>
<Amount> QUIL (PendingTransaction 0x27fff099dee515ece193d2af09b164864e4bb60c19eb6719b5bc981f92151009)
```
{% endcode %}

\
This creates a separate pending transaction because if the refund address is specified by the originator, and were they to specify another of your own addresses, it would be no different than accepting.

### 6. Performing a Mutual Transfer

Pending transactions introduce friction, but without that friction, users can be spammed coins they don't want, or sent coins from an address they do not wish to interact with. If both parties agree in advance to transact, they can perform a mutual transfer, where both parties must be online, but can avoid having to deal with the two-phase transaction. This is great for maintaining privacy (each party's account is private) as well as ensuring a timely completion of a transaction:\
\
On the receiver's side:

{% code overflow="wrap" %}
```bash
./qclient token mutual-receive <ExpectedAmount>
Rendezvous: 0x2ad567e4fc1ac335a8d3d6077de2ee998aff996b51936da04ee1b0f5dc196a4f
Awaiting sender...
```
{% endcode %}

and after the sender connects:

{% code overflow="wrap" %}
```bash
Awaiting sender... OK
<Amount> QUIL (Coin 0x0525c76ecdc6ef21c2eb75df628b52396adcf402ba26a518ac395db8f5874a82)
```
{% endcode %}

On the sender's side:

```bash
./qclient token mutual-transfer <Rendezvous> <Amount>
Confirming rendezvous... OK
<Amount> QUIL (Coin [private])
```

or if using the raw Coin address:

```bash
./qclient token mutual-transfer <Rendezvous> <OfCoin>
Confirming rendezvous... OK
<Amount> QUIL (Coin [private])
```

This will likely be the first unique experience Quilibrium provides to users already familiar with other networks, as privacy preservation is an immediately obvious and first class experience here by showing the user what it can (or _cannot_) see.

### 7. Claiming Rewards

_Claiming rewards as wQuil, please visit_ [Claiming wQuil Token section](claiming-wquil-token.md).

Tokens issued after v2.0 are issued by nodes providing their proofs to the Mint Authority functionality of the token application. Claiming those rewards can be configured to be performed automatically (default, generates a new Coin every claim and merges them), or in lump sums at intervals, manually. It is recommended for ease of management that the defaults are applied, so that in the event of hardware failure no rewards go unclaimed.\
\
If you wish to do it manually however, you will need to run:

{% code overflow="wrap" %}
```bash
./qclient token mint all
<Amount> QUIL (Coin 0x162ad88c319060b4f5ea6dbf9a0c2cd82d3d70dfc22d5fc99ca5371083d68416)
```
{% endcode %}

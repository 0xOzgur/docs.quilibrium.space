# ⁉️ FAQ



<details>

<summary><strong>What's my balance? Why am I still at 0?</strong></summary>

This question is the most frequently asked of them all. Depending on when you joined in on the project, the answer will vary. At this point, fully-synchronized nodes should start accumulating a balance. We are working to improve synchronization so it's not an incredibly long slog to get there. Everyone who has been participating should see some balance when Dusk launches.

</details>

<details>

<summary><strong>How do I know my node is operating properly?</strong></summary>

You should see the frame number your node is processing continually increase. This may be slow at first due to heavy fork reconciliation – think of each node with a prover key as being able to send valid frames, but the timereel as a means to choose between them. Because we have had many iterations and lots of early networking issues, we may be initial forks that your node must churn through. We're working on improving sync to be smarter about this so you don't have to reconcile all of them.

</details>

<details>

<summary><strong>Where do I run a node?</strong></summary>

Don't run it on IaaS/PaaS providers like AWS/GCP/Azure. The egress fees will eat you alive. If you don't want to/can't run on your own hardware, VPS/ bare metal hosts do well. Be advised: Q uses a lot of bandwidth in this stage.

</details>

<details>

<summary><strong>How do I run a node?</strong></summary>

If you have git and go 1.20 installed, it's just these three steps:

```
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
cd ~/ceremonyclient/node
GOEXPERIMENT=arenas go run ./...
```

If you don't, follow whatever guide for your operating system to install go 1.20 (MUST be 1.20.X, 1.19- won't work, 1.21+ won't either).

</details>

<details>

<summary><strong>I'm getting cannot load arena: malformed module path "arena" when I run it, what am I doing wrong?</strong></summary>

Wrong version of golang. Make sure you installed 1.20 (and it has priority on your PATH environment variable)

</details>

<details>

<summary><strong>-balance tag:</strong></summary>

The -balance command tag when calling the GOEXPERIMENT=arenas go run ./... -balance has been deprecated since Dawn v1.2.9. Unless your node was upgraded to the latest version, yet upgraded from a version <1.2.9 (e.g. 1.2.7) (i.e. the very first version your node ran with was one older than 1.2.9), the -balance command tag will not work anymore and will face the error below.

</details>

<details>

<summary><strong>Who is developing Quilibrium?</strong></summary>

Officially the core Quil dev team is just Cassie as of right now (making this an oddly third person statement), plus many folks who have submitted PRs to the core project, as well as related projects like Agost with quilibrium-rs, and Sir0uk with the nodekeeper dashboard. If you'd like to join in, there's loads of things to work on, please DM!

</details>


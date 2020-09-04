# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) goes deeply into the math and implementation considerations. A harvester can harvest multiple plots on the same machine. A farmer can then control multiple harvesters across many machines to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. You can examine the [Space Required section](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes).

# What k-size should you plot?

We believe that plots created with Beta 1.8 and newer versions of the chia software will work on mainnet at launch. We are certain that the minimum plot size will be at most k=32. The original design assumed that k=30 will be the minimum plot size but recent testing has ruled it out. We intend to speed up the time it takes to plot and that will mean we choose a minimum k value of 31 or 32.

The simple answer is:
* k30 (and smaller) plots are definitely not going to work on mainnet.
* k31 plots MIGHT work on mainnet.
* k32 (and larger) plots will definitely work on mainnet.

("Definitely" has the caveat understanding that this is beta software. There are no _plans_ to change the plot format, but if bugs are found, they'll need to be squashed.)

The Chia dev team is going to great lengths to speed up the plotting process--from now until mainnet. The goal is to keep it so that the top-of-the-line hardware takes at least 1 hour to plot the minimum k-size, and there's no way to cheat the system. That's why the minimum k-size isn't clear yet--because the plotting speed improvements haven't been implemented yet.

# What is recommended for plotting?

We think you will want to use NVMe SSD drives to create your plots on. Then you can migrate your plots off to whatever storage you want to keep them on long term. You could even load them on a Raspberry Pi with outdated USB 2.0 drives attached and they will Harvest and Farm just fine. PC World offers this great [background on current storage technologies](https://www.pcworld.com/article/2899351/everything-you-need-to-know-about-nvme.html) but this graph gives you a quick view of why we recommend NVMe SSD:
![NVMe SSD vs SATA](images/plotting-nvme-ssd.png "NVMe SSD is 5.5 times faster than SATA SSD")

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof by which the result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups, which are explained in this [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

# How do I tell if I'm farming correctly?

If you see plots in the Plots section of the Farm page in the GUI - your plots are being farmed. You will see challenges as they come through in the Challenges section however you usually will not have a proof worth sending to the network. You can additionally check the tooltip on the top right of the "Total size of local plots" on the Farm view and it will tell you how much space is being farmed and statistically how long it should take - on average - to win a block.

# How do I know if my plots are OK?

Run `chia plots check -n 100` to try 100 sample challenges for each plot. Each of your plots should return a number around 100, which means it found 100 proofs of space. If some of your plots are missing for some reason you may need to add the directory they are in to your config.yaml file. That can be done in the GUI with the MANAGE PLOT DIRECTORIES button or on the command line with `chia plots add -d [directory]`.

# How do I send or receive a transaction?

The wallet will show you your address and provide an interface for you to spend your chia funds.
Read about how to build and start the wallet GUI in our [quickstart guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide#run-a-full-node--farmer--harvester--wallet). There is now some wallet functionality available on the command line. Try `chia show -w`.

The wallet software also provides features related to coloured coins, and trade offers.

# Why is it recommended that a winning plot be deleted on mainnet?

There is a possible attack where an attacker who can co-ordinate N deep from the tip of the chain can try to coerce a winning farmer to re-write a historical transaction block. Additionally, having more than one set of rewards go to the same plot and farmer lowers the farmer's pseudonymity. We expect that by mainnet, plotting will both be much faster and most farmers will have large enough farms that re-plotting the space opened up by a winning plot will be quick enough. We plan to have the software automate the process up to and including kicking off a remote plotting process if the current hardware that a farmer or harvester are on is not up to the task of re-plotting.

# What are the next milestones?

We are now in the Beta testnet blockchain phase. During Beta you should expect continued improvements in ease of install, and support for and user interface for our reference smart transactions. We will have some over the wire protocol changes that will require hard forks but should migrate your existing plots and installations easily to the newer chain. Starting with Beta 8, the new plot format is intended to be the same as mainnet so you will be able to get your plots in order before mainnet launch. We expect to launch mainnet at the end of 2020. We also plan to have an 4-6 week period after mainnet launch when no transactions are allowed but farming rewards will be occurring. This is to help the storage network stabilize and to reward our space farmers first.

# How can I contribute?

You should check out [CONTRIBUTING.md](https://github.com/Chia-Network/chia-blockchain/blob/master/CONTRIBUTING.md) in the repository but the quick answer is to please base your pull requests off the dev branch. The dev branch will only accept rebase merges or squash merges.

# How do I upgrade and keep my keys and plots?

If you use the GUI, it will migrate from release to release for you. For both the GUI and the command line, your keys are stored on OS specific keychains. If running services from the command line only, `chia init` will migrate your config.yaml and dbs if appropriate to the new version. Keys and plots made before Beta 1.8 are deprecated and useless.

# Can I run this on a Raspberry Pi 3 or 4?

Yes, and here are the [instructions](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi). This project requires a 64 bit OS. Pi 3 and Pi 4. One can install and run harvesters, farmers, and full nodes on the Pi. Plotting on a Pi is feasible now with Chacha8 instead of AES but the Pi isn't quick. Modern desktops and laptops plot in the 0.07 - 0.10 GiB/minute range and the Pi 4 plots at 0.025 GiB/minute. Pi is also not a candidate for Timelords or VDF clients... We have not tested Pi 3 yet so any feedback would be welcomed.

# Why won't the GUI start on my linux distribution?

Some linux distros don't allow Electron to install correctly. The error is some variant of `The SUID sandbox helper binary was found, but is not configured correctly`. Details and fixes are in [Electron Issue 17972](https://github.com/electron/electron/issues/17972). Thanks [@kargakis](https://github.com/kargakis). If you use the `sysctl` workaround it does not persist so you'll want to see how to change that [here](https://unix.stackexchange.com/questions/25382/make-changes-to-sys-persistent-between-boots).

# Why does chia-blockchain require python 3.7 or greater?

The codebase takes advantage of the newest async generators, especially async/await, which requires 3.7 or better. Python has a [walk through of Async IO](https://realpython.com/async-io-python/) and the related python 3.7 requirements.

# What is this UPnP Error?

UPnP is an optional setting that allows users to open a port in their router and therefore allow other nodes to connect to them. This is not required, since your node can still make outgoing connections without UPnP.

For some routers, UPnP is enabled automatically, but for others, you might have to go into your router settings and enable UPnP manually. Sometimes restarting the router is also necessary.

Another option is port forwarding, where you tell your router/NAT to forward requests on port 8444 to your machine.

# How do I rotate logs?

In the past we recommended using logrotate but python did not interact nicely with it. Starting with 1.0 beta 6, chia-blockchain will automatically rotate every 20MB and save 7 previous log segments before deleting the oldest so you don't have to do anything.
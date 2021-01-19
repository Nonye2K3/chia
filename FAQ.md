# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) goes deeply into the math and implementation considerations. A plot is a large set of proofs of space. A harvester can harvest multiple plots on the same machine. A farmer can then control [multiple harvesters across many machines](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. In practice this means that final size is roughly `((2 * k) + 1) * (2 ** k - 1) * 0.772` though that constant is estimated. You can examine the [Space Required section](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes). Current working space needed for the default plotting options of a k=32 is 332GiB and the final file is approximately 101.4 GiB. There is small natural variation in temp space needed and the final file size of each plot.

# What k-size should you plot?

Plots created with Beta 8 and newer versions of the Chia software will work on mainnet at launch. The minimum plot size is k=32. The original design assumed that k=30 would be the minimum plot size but recent testing has ruled it and k=31 out.

There is only one reason why you might want to plot larger than k=32 and that is to optimize the total utilization of a given drive or space. A couple of k=33 plots with a majority of k=32 plots can bring down the leftover unused space on a drive. There is also a possible, though theoretical at this point, value in larger plots as they may be accessed less often due to the plot filter. That could lead to being able to spin the drives down but there is debate about whether that can be done or lead to savings in practice versus shortening drive life.

The Chia dev team will continue to enhance the plotter though many of the theoretical methods to speed up plotting have been implemented. Bram believes we may be able to cut plot time in half once more - but that's likely the maximum improvement in plotting time remaining. The goal is to keep it so that the top-of-the-line hardware takes at least 1 hour to plot the minimum k-size, and Phase 1 takes at least ~10 minutes so there is no way to cheat the system.

# What is recommended for plotting?

We think you will want to use Data Center grade NVMe SSD drives to create your plots. Regular consumer NVMe SSD generally has too low of a TBW rating. One of our community members keeps this handy [SSD Endurance document](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance) up to date so you can compare various SSDs. You should never use your root/OS SSD to plot as it can lead to drive failure and loss of booting. Also, you can plot directly to hard drives and get good results, especially if you plot in parallel to different drives. You use non-root SSD over Thunderbolt 3 and migrate your plots off to whatever storage you want to keep them on long term. You could even load them on a Raspberry Pi 3 or 4 with outdated USB 2.0 drives attached and they will Harvest and Farm just fine. PC World offers this great [background on current storage technologies](https://www.pcworld.com/article/2899351/everything-you-need-to-know-about-nvme.html) but this graph gives you a quick view of why we recommend NVMe SSD:
![NVMe SSD vs SATA](images/plotting-nvme-ssd.png "NVMe SSD is 5.5 times faster than SATA SSD")

# Can I plot more than one plot at a time?

Yes and starting with Beta 19 you can either use the GUI or CLI. Over the short run you have a bit more control of plotting using the CLI. There are [tips for Windows users](https://github.com/Chia-Network/chia-blockchain/wiki/Windows-Tips) and Mac users can find their CLI commands in the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide#macos). You may have better results if you stagger the start time of parallel plotting processes depending on your hardware setup.

# Can I move my plot files around?

Yes. The [Moving plots](https://github.com/Chia-Network/chia-blockchain/wiki/Moving-plots) topic here on the wiki gives you the details. You may also want to consider running a [remote harvester](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines).

# My plotting attempt got "Caught plotting error: Not enough memory..."?

If you see something like `Caught plotting error: Not enough memory for sort in memory.  Need to sort X.XXGiB` then you need to either select more memory buffer or more buckets. More buckets require less memory but will create more temp files and more sporadic disk writing. Try 128 buckets or try increasing the RAM max usage/`-b` to 4608MiB.

# My computer/drive went into power save or rebooted while plotting?

Unfortunately, resuming a plot is not yet supported but likely will be later in 2021. We suggest that you disable power saving mode - especially for external drives - and try to limit other possible causes of interruptions. Plotting a k=32 is going to take between 6 and 20 hours, depending on your hardware, so these interruptions can be painful. They are also a part of why we don't recommend plotting plots larger than k=32 as each increment in k generally doubles the time to complete a single plot.

# What are the plans for the project and what are its tokenomics?

This is the Repository FAQ which focuses on how to use the software. For lots more on the motivations, technology, and plans for the project we suggest you read the [Project FAQ](https://www.chia.net/faq/).

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof whose result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups. You can learn about them in our [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

# How do I tell if I'm farming correctly?

If you see plots in the Plots section of the Farm page in the GUI - your plots are being farmed. You will see challenges as they come through in the Challenges section however you usually will not have a proof worth sending to the network. You can additionally check the tooltip on the top right of the "Total size of local plots" on the Farm view and it will tell you how much space is being farmed and statistically how long it should take - on average - to win a block.

# Does it matter how fast my internet connection is?

No. You have at least 30 seconds to respond to challenges.

# Do I have to be connected to the internet or synced to plot?

No. Plotting can be done entirely offline and needs nothing from the blockchain to complete. The only time you have to be online and synced is when you're farming so that you receive new challenges for the next sub blocks and transactions to include in a transaction block if you're lucky enough to win one of them and get the transaction fees.

# Is there any advantage in plotting larger k sizes?

No. As long as you plot at least k=32, those plots will be eligible to win on mainnet. In a decade or more, k=32 may become too small, but that's speculative. Usually the only reason to plot larger than k=32 is to optimize using all of the space on a given drive. For example, it may make sense to have two k=33's and the rest k=32 so that you only leave 10 GB free on a given drive.

# Is the final size of the plotted space the only variable in how often I can win block rewards?

Yes. 

# How do I know if my plots are OK?

Run `chia plots check -n 30` to try 30 sample challenges for each plot. Each of your plots should return a number around 100, which means it found around 100% of the attempted proofs of space. If you're still worried try `-n 100` as more random attempts will give you a more valid assessment that the plots is fine. It really is ok if your plot is within 90%-110%. If some of your plots are missing for some reason you may need to add the directory they are in to your config.yaml file. That can be done in the GUI with the MANAGE PLOT DIRECTORIES button or on the command line with `chia plots add -d [directory]`.

# I have only 10 TB, will I ever win XCH on mainnet?

Starting with the new consensus algorithm in Beta 19, there are 4608 chances per day to win 1 TXCH (and thus XCH on mainnet.) If you have 10TB and there are 30PB of total storage on mainnet then you would expect to win ~1.5 TXCH per day on average. The math is .010 PB/30 PB * 4608 = 1.536.

# Can I join a farming pool?

Not yet. Currently the plots you generate are plotted to your own self pool key. Bram has much more on our [plans around pooling](https://www.chia.net/2020/11/10/pools-in-chia.html) and pool support will become high priority as soon as mainnet is released. You will not be able to change the pool of your current plots so you can just continue to farm them or slowly replace them over time with new plots using one of the upcoming pooling methods.

# How do I send or receive a transaction?

The wallet will show you your address and provide an interface for you to spend your chia funds.
Read about how to build and start the wallet GUI in our [quickstart guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide#run-a-full-node--farmer--harvester--wallet). There is now some wallet functionality available on the command line. Try `chia show -w`.

The wallet software also provides features related to coloured coins, and trade offers. Support for signing and sending transactions from the command line will be available soon.

# Why is it recommended that a winning plot be deleted on mainnet?

There is a possible attack where an attacker who can co-ordinate N deep from the tip of the chain can try to coerce a winning farmer to re-write a historical transaction block. This attack is much more difficult and thus less of a risk in new consensus. Additionally, having more than one set of rewards go to the same plot and farmer lowers the farmer's pseudonymity. We expect that by mainnet, plotting will both be much faster and most farmers will have large enough farms that re-plotting the space opened up by a winning plot will be quick enough. We plan to have the software automate the process up to and including kicking off a remote plotting process if the current hardware that a farmer or harvester are on is not up to the task of re-plotting. But to repeat, deleting winning plots is, and will always be, totally optional.

# What are the next milestones?

We are about to complete the Beta testnet blockchain phase. During Beta you should expect continued improvements in ease of install, and support for and user interface for our reference smart transactions. We will have some over the wire protocol changes that will require hard forks but should migrate your existing plots and installations easily to the newer chain. Starting with Beta 8, the new plot format is the same as mainnet so you will be able to get your plots in order before mainnet launch. After a short period of testing of the new consensus algorithm we will leave the Beta phase and enter the Release Candidate phase. These are releases that we believe are consensus critical feature complete but that we may need to add minor functionality to and fix bugs found. We expect to launch mainnet at the end of 2020 (but see below.) We also plan to have a 4-6 week period after mainnet launch when no transactions are allowed but farming rewards will be occurring. This is to help the storage network stabilize and to reward our space farmers first.

# When mainnet?

The original plan was for December 2020 or January 2021 and our development of our original consensus met our expected timeline. However, in February of 2020 we were introduced to some new ideas at the Stanford Blockchain 2020 event that lead to our [new consensus algorithm](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) - which is a significant upgrade in security and usability of our chain. Implementing that has set us back a few weeks and now we are targeting the second half of January 2021 - February 2021 though there remains some timing uncertainty in the order of a few more weeks as we work out bugs in the new consensus. Our intent is to not have to hard fork once mainnet is started so we have to be very certain that consensus critical items are complete and our first security audits are complete. Our community members warn that each time you ask in [Keybase](https://keybase.io/team/chia_network.public) it gets moved back a week.

# How can I contribute?

You should check out [CONTRIBUTING.md](https://github.com/Chia-Network/chia-blockchain/blob/master/CONTRIBUTING.md) in the repository but the quick answer is to please base your pull requests off the dev branch. The dev branch will only accept rebase merges or squash merges.

# How do I upgrade and keep my keys and plots?

If you use the GUI, it will migrate from release to release for you. For both the GUI and the command line, your keys are stored on OS specific keychains. If running services from the command line only, `chia init` will migrate your config.yaml and dbs if appropriate to the new version. Keys and plots made before Beta 8 are deprecated and useless.

# Can I run this on a Raspberry Pi 3 or 4?

Yes, and here are the [instructions](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi). This project requires a 64 bit OS. Pi 3 and Pi 4. One can install and run harvesters, farmers, and full nodes on the Pi. Plotting on a Pi is feasible now with Chacha8 instead of AES but the Pi isn't quick. Modern desktops and laptops plot in the 0.07 - 0.10 GiB/minute range and the Pi 4 plots at 0.025 GiB/minute. Pi is also not a candidate for Timelords or VDF clients...

# Why won't the GUI start on my linux distribution?

Some linux distros don't allow Electron to install correctly. The error is some variant of `The SUID sandbox helper binary was found, but is not configured correctly`. Details and fixes are in [Electron Issue 17972](https://github.com/electron/electron/issues/17972). Thanks [@kargakis](https://github.com/kargakis). If you use the `sysctl` workaround it does not persist so you'll want to see how to change that [here](https://unix.stackexchange.com/questions/25382/make-changes-to-sys-persistent-between-boots).

# Why does chia-blockchain require python 3.7 or greater?

The codebase takes advantage of the newest async generators, especially async/await, which requires 3.7 or better. Python has a [walk through of Async IO](https://realpython.com/async-io-python/) and the related python 3.7 requirements.

# What is this UPnP Error?

[UPnP](ttps://www.homenethowto.com/ports-and-nat/upnp-automatic-port-forward/) is an optional setting that allows users to open a port in their router and therefore allow other nodes to connect to them. This is not required, since your node can still make outgoing connections without UPnP.

For some routers, UPnP is enabled automatically, but for others, you might have to go into your router settings and enable UPnP manually. Sometimes restarting the router is also necessary.

Another option is port forwarding, where you tell your router/NAT to forward requests on port 8444 or 5844 for testnet to your machine.

# Why should I not run more than one node on a home network and what's this about UPnP?

First, running more than one node on your home network is wasting bandwidth by syncing two copies of the blockchain over your download link. You can get the same results by running one node and [using multiple harvesters](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) on multiple computers. Second, if you have [uPnP](https://www.homenethowto.com/ports-and-nat/upnp-automatic-port-forward/) enabled on both nodes and your home router supports uPnP (and most do) it will cause both of your nodes to not sync the blockchain. You need to disable uPnP on all or all but one node behind a uPnP enabled router. The CLI command `chia configure --enable-upnp False` will turn uPnP off on a node. It requires a restart of the node to take effect. If you disable UPnP on all but one of your nodes then your local router will forward inbound 8444 traffic to the one node and the rest will now be able to connect to the network but just will not accept inbound connections from the network.

# How do I rotate logs?

In the past we recommended using logrotate but python did not interact nicely with it. Starting with 1.0 beta 6, chia-blockchain will automatically rotate every 20MB and save 7 previous log segments before deleting the oldest so you don't have to do anything.
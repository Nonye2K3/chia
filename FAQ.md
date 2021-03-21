# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture). The [new consensus document](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) is the most current documentation however. You can also check out our [Timelord documentation](https://github.com/Chia-Network/chia-blockchain/wiki/Timelords).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) goes deeply into the math and implementation considerations to mitigate [Hellman's Time - Memory tradeoff](https://pdfs.semanticscholar.org/f0ba/66072ac10d9898b8a79171ec726d45ec804b.pdf) problem. A plot is a large set of proofs of space. A harvester can harvest multiple plots on the same machine. A farmer can then control [multiple harvesters across many machines](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. In practice this means that final size is roughly `((2 * k) + 1) * (2 ** (k - 1)) * 0.762` though that constant is estimated. You can examine the [Space Required section](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes). Current working space needed for the default plotting options of a k=32 is 332GiB and the final file is approximately 101.4 GiB. There is small natural variation in temp space needed and the final file size of each plot. Note that 332GiB is 356.5GB.

# What k-size should you plot?

Plots created with Beta 8 and newer versions of the Chia software will work on mainnet at launch. The minimum plot size is k=32. 

There is only one reason why you might want to plot larger than k=32 and that is to optimize the total utilization of a given drive or space. A couple of k=33 plots with a majority of k=32 plots can bring down the leftover unused space on a drive. 

The Chia dev team will continue to enhance the plotter though many of the theoretical methods to speed up plotting have been implemented. Bram believes we may be able to cut plot time in half once more - but that's likely the maximum improvement in plotting speed remaining. The goal is to keep it so that the top-of-the-line hardware takes at least 1 hour to plot the minimum k-size, and Phase 1 takes at least ~10 minutes so there is no way to cheat the system.

# What is recommended for plotting?

We think you will want to use used Data Center grade NVMe SSD drives to create your plots. Regular consumer NVMe SSD generally has too low of a [TBW](https://www.enterprisestorageforum.com/storage-hardware/ssd-lifespan.html) rating. One of our community members keeps this handy [SSD Endurance document](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance) up to date so you can compare various SSDs. You should never use your root/OS SSD to plot as it can lead to drive failure and loss of booting. You can plot directly to hard drives and get good results, especially if you plot in parallel to different drives. You can use non-root SSD over Thunderbolt 3 and migrate your plots off to whatever storage you want to keep them on long term. You could even load them on a Raspberry Pi 3 or 4 with outdated USB 2.0 drives attached and they will harvest and farm just fine. PC World offers this great [background on current storage technologies](https://www.pcworld.com/article/2899351/everything-you-need-to-know-about-nvme.html) but this graph gives you a quick view of why we recommend NVMe SSD:
![NVMe SSD vs SATA](images/plotting-nvme-ssd.png "NVMe SSD is 5.5 times faster than SATA SSD")

# Can I plot more than one plot at a time?

Yes and starting with Beta 19 you can either use the GUI or CLI. Over the short run you have a bit more control of plotting using the CLI. There are [tips for Windows users](https://github.com/Chia-Network/chia-blockchain/wiki/Windows-Tips) and Mac users can find their CLI commands in the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide#macos). You may have better results if you stagger the start time of parallel plotting processes depending on your hardware setup.

# Can I make plots on one machine and move it to another machine?

Yes. The [moving plots](https://github.com/Chia-Network/chia-blockchain/wiki/Moving-plots) topic here on the wiki gives you the details. You may also want to consider running a [remote harvester](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines). You can also use the same private key set to plot on more than one machine at a time but be aware of the [uPnP issues](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#why-should-i-not-run-more-than-one-node-on-a-home-network-and-whats-this-about-upnp).

# What is the -2, directory and how should I set it?

`-2` is in use during phase 3 and 4. It is the file being built into the resulting .plot file. As it is done compressing tables during phase 3, it will move them into the `.plot.2.tmp` file (`-2`), and phase 4 will scan through the entire `.plot.2.tmp` (`-2`) file, and write table headers for easy access by the harvester. When phase 4 is done, if `-2` = `-d`, it will simply rename the `.plot.2.tmp` to `.plot`. If `-2` != `-d`, it will copy the file into place, then rename, and finally remove the `-2` file. The amount of writing is about 110% of the resulting `.plot` file size. It is a setup dependent option - is your setup faster at moving the compressed tables into the `.plot.2.tmp` file, and then scan through the entire file, and write table headers during phase 4 - and then copy to `-d` (`-2` = `-t`) - or is it faster to send the compressed tables directly into the `-d` (`-2` = `-d`) directory, and then in phase 4, scan through the entire file, and write table headers inside `-d` (`-2` = `-d`) thereby skipping the final copy into place. The `-2` directory can be set in the Advanced Options for Step 3 in the GUI.

# My plotting attempt got "Caught plotting error: Not enough memory..."?

If you see something like `Caught plotting error: Not enough memory for sort in memory.  Need to sort X.XXGiB` then you need to either select more memory buffer or more buckets. More buckets require less memory but will create more temp files and more sporadic disk writing. You will almost always want to use 128 buckets and you should try increasing the RAM max usage/`-b` to 4608MiB.

# My computer/drive went into power save or rebooted while plotting?

Unfortunately, resuming a plot is not yet supported but likely will be later in 2021. We suggest that you disable power saving mode - especially for external drives - and try to limit other possible causes of interruptions. Plotting a k=32 is going to take between 6 and 20 hours, depending on your hardware, so these interruptions can be painful. They are also a part of why we don't recommend plotting plots larger than k=32 as each increment in k generally doubles the time to complete a single plot.

# What are the plans for the project and what are its tokenomics?

This is the Repository FAQ which focuses on how to use the software.  We have released our [Business Whitepaper](https://www.chia.net/2021/02/10/chia-businesss-whitepaper.html) that goes into details of both the tokenomics, the pre-farm, and our go to market strategy. Additionally you can review the [Project FAQ](https://www.chia.net/faq/).

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof whose result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups. You can learn about them in our [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf). [Timelords](https://github.com/Chia-Network/chia-blockchain/wiki/Timelords) usually run three VDFs at a time for the three internal blockchains of the Chia blockchain. They run as `vdf_client` processes.

# How do I tell if I'm farming correctly?

If you see plots in the Plots section of the Farm page in the GUI - your plots are being farmed. You will see challenges and proof attempts as they come through in the Last Attempted Proof section however you usually will not have a proof worth sending to the network due to the [plot filter](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#what-is-the-plot-filter-and-why-didnt-my-plot-pass-it). You can additionally see the Total Size of Plots on the Farm view and it will tell you how much unique space is being farmed and statistically how long it should take - on average - to win a block.

# Does it matter how fast my internet connection is?

No. You have 30 seconds to respond to challenges.

# Do I have to be connected to the internet or synced to plot?

No. Plotting can be done entirely offline and needs nothing from the blockchain to complete. The only time you have to be online and synced is when you're farming so that you receive new challenges for the next blocks and transactions to include in a transaction block if you're lucky enough to win one of them and get the transaction fees.

# Is there any advantage in plotting larger k sizes?

No. As long as you plot at least k=32, those plots will be eligible to win on mainnet. In a decade or more, k=32 may become too small, but that's speculative. Usually the only reason to plot larger than k=32 is to optimize using all of the space on a given drive. For example, it may make sense to have two k=33's and the rest k=32 so that you only leave 10 GB free on a given drive.

# Is the final size of the plotted space the only variable in how often I can win block rewards?

Yes. 

# How do I know if my plots are OK?

Run `chia plots check -n 30` to try 30 sample challenges for each plot. Each of your plots should return a number around 100, which means it found around 100% of the attempted proofs of space. If you're still worried try `-n 100` as more random attempts will give you a more valid assessment that the plots is fine. It really is ok if your plot is within 90%-110%. If some of your plots are missing for some reason you may need to add the directory they are in to your config.yaml file. That can be done in the GUI with the MANAGE PLOT DIRECTORIES button or on the command line with `chia plots add -d [directory]`.

# I have only 10 TB, will I ever win XCH on mainnet?

Starting with the new consensus algorithm and rewards schedule change in Beta 27, there are 4608 chances per day to win 2 TXCH (and thus XCH on mainnet.) If you have 10TB and there are 50PB of total storage on mainnet then you would expect to win ~1.8 TXCH per day on average. The math is .010 PB/50 PB * 4608 * 2 = 1.84. That means that over a long enough period of time you will expect to average out to generally winning every other day.

# What is the plot filter and why didn't my plot pass it?

Farmers compute a plot filter based on the signage point, their plot id, and the sub-slot challenge - which are hashed together to create the plot filter bits. If the plot filter bits start with 9 zeroes, that plot passes the filter for that signage point, and can proceed. This disqualifies around 511/512 of all proofs of space on the network, for each signage point. There are 4608 * 2 or 9216 signage points per day so the average plot should pass the filter 18 times per 24 hours on average. Once a plot passes the plot filter it then competes for the best proof of space with every other plot that also passed that plot filter for that signage point. For reasons that aren't super simple to intuit, the only thing each plot is competing on is to have the best proof of space and thus the chances of getting a reward depend on total size of plots on the farm - even with the plot filter in place.

As long as the plot passes the filter, and do not have any internal file errors, the plot will always be eligible to compete for the best proof of space. Moving the plot to another directory or server will not change its eligibility.

# Can I join a farming pool?

Not yet. Currently the plots you generate are plotted to your own self pool key. Bram has much more on our [plans around pooling](https://www.chia.net/2020/11/10/pools-in-chia.html) and pool support will become high priority as soon as mainnet is released. You will not be able to change the pool of your current plots so you can just continue to farm them or slowly replace them over time with new plots using one of the upcoming pooling methods.

# How do I send or receive a transaction?

The Wallets page in the GUI will show you your receive address and provide an interface for you to spend your chia funds. You can also obtain a new wallet receive address any time you would like and those funds will all come to the same place as they are based on [HD Keys](https://www.investopedia.com/terms/h/hd-wallet-hierarchical-deterministic-wallet.asp).

There is growing wallet functionality available on the command line. Try `chia wallet -h`. Wallet software also provides features related to coloured coins, and trade offers. You can get a receive address on the cli with `chia keys show`.

# What are HD Keys?

HD or Hierarchical Deterministic keys are a type of public key/private key scheme where one private key can have a nearly infinite number of different public keys (and therefor wallet receive addresses) that will all ultimately come back to and be spendable by a single private key.

# How many confirmations do I need to trust a chia transaction is final?

The new consensus uses correlated randomness (an idea introduced in [Proof-of-Stake Longest Chain Protocols: Security vs Predictability](http://tselab.stanford.edu/downloads/PoS_LC_SBC2020.pdf)) to prevent double dipping, and this converges much faster than a chain like Bitcoin. unless there is an attack or network split there is never a reorg. One still needs to wait for more than 6 blocks to get comparable confidence as one would get in Bitcoin for 6 blocks because in Chia the transactions are somewhat decoupled from the proofs (to prevent grinding), and as a consequence the farmers who contributed the x last blocks can collude to rewrite the transactions of the last x blocks. So you should wait for some x blocks, where x is large enough so you're confident that *at least one* of the x (randomly chosen) farmers who contributed the last x blocks will not collude (i.e., be malicious or get bribed). As this is a different type of attack as the one used to justify the 6 blocks Bitcoin rule, one can not just give a number that would correspond to the same confidence in Chia, but the 32 blocks (approx. 10 minutes) suggested in the current [new consensus document](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) (the "Farmer bribe foliage reorg attack " section on page 23) seems conservative. 

# I have heard that it's recommended that a winning plot be deleted on mainnet?

There is a possible attack where an attacker who can co-ordinate N deep from the tip of the chain can try to coerce a winning farmer to re-write a historical transaction block. This attack is much more difficult and thus less of a risk in new consensus and thus we only recommend deleting and re-plotting a plot to farmers with in excess of 1PB of farm size. Anyone smaller than that would be difficult for an attacker to locate and can more safely continue to farm plots that have already won. We plan to have the software automate the process up to and including kicking off a remote plotting process if the current hardware that a farmer or harvester are on is not up to the task of re-plotting. But to repeat, deleting winning plots is, and will always be, totally optional.

# When mainnet?

The original plan was for December 2020 or January 2021 and our development of our original consensus met our expected timeline. However, in February of 2020 we were introduced to some [new ideas](https://arxiv.org/abs/1910.02218) at the Stanford Blockchain 2020 event that lead to our [new consensus algorithm](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) - which is a significant upgrade in security and usability of our chain. Per our [recent blog post](https://www.chia.net/2021/03/15/mainnet-update.html), we currently expect to release 1.0 during the day of March 17, 2021. Mainnet green flag launch is planned for 7AM PDST (14:00 UTC) on Friday March 19, 2021. Our intent is to not have to hard fork once mainnet is started so we have to be very certain that consensus critical items are complete and our first security audits are complete. Our community members warn that each time you ask in [Keybase](https://keybase.io/team/chia_network.public) it gets moved back a week.

# What is XCH, TXCH, and mojos?

XCH is the currency symbol for Chia. TXCH is the currency symbol currently being used for testnet chias. TXCH has no value and is only used for testing purposes. Chias and testnet chias can be divided up to 12 decimal places (trillionths). The smallest unit of chia, a trillionth of a chia, is called a mojo, as a tribute to [Mojo Nation](https://en.wikipedia.org/wiki/Mnet_(peer-to-peer_network)#Evil_Geniuses_for_a_Better_Tomorrow), a decentralized file storage platform created in the early 2000s by Zooko Wilcox, Bram Cohen, and others.


# How can I contribute?

You should check out [CONTRIBUTING.md](https://github.com/Chia-Network/chia-blockchain/blob/master/CONTRIBUTING.md) in the repository but the quick answer is to please base your pull requests off the dev branch. The dev branch will only accept rebase merges or squash merges. You can help [translate the application](https://crowdin.com/project/chia-blockchain/) as well. Translating the GUI is especially useful and pretty easy to do with our Crowdin [Chia-Blockchain-GUI](https://crowdin.com/project/chia-blockchain) tool.

# How do I upgrade and keep my keys and plots?

If you use the GUI, it will migrate from release to release for you. For both the GUI and the command line, your keys are stored on OS specific keychains. If running services from the command line only, `chia init` will migrate your config.yaml and dbs - if appropriate - to the new version. Keys and plots made before Beta 8 are deprecated and useless.

# Can I run this on a Raspberry Pi 3 or 4?

Yes, and here are the [instructions](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi). This project requires a 64 bit OS so a Pi 3 or Pi 4. One can install and run harvesters, farmers, and full nodes on the Pi. Plotting on a Pi is feasible now with Chacha8 instead of AES but the Pi isn't quick. Modern desktops and laptops plot in the 0.07 - 0.10 GiB/minute range and the Pi 4 plots at 0.025 GiB/minute. Pi is also not a candidate for Timelords or VDF clients...

# What is this UPnP Error?

[UPnP](ttps://www.homenethowto.com/ports-and-nat/upnp-automatic-port-forward/) is an optional setting that allows users to open a port in their router and therefore allow other nodes to connect to them. This is not required, since your node can still make outgoing connections without UPnP.

For some routers, UPnP is enabled automatically, but for others, you might have to go into your router settings and enable UPnP manually. Sometimes restarting the router is also necessary.

Another option is port forwarding, where you tell your router/NAT to forward requests on port 8444 or 58444 for testnet to your machine.

# Why should I not run more than one node on a home network and what's this about UPnP?

First, running more than one node with the same private keys on your home network is wasting bandwidth by syncing two copies of the blockchain over your download link. You can get the same results by running one node and [using multiple harvesters](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) on multiple computers. Second, if you have [uPnP](https://www.homenethowto.com/ports-and-nat/upnp-automatic-port-forward/) enabled on both nodes and your home router supports uPnP (and most do) it will cause both of your nodes to not sync the blockchain. You need to disable uPnP on all or all but one node behind a uPnP enabled router. The CLI command `chia configure --enable-upnp false` will turn uPnP off on a node. It requires a restart of the node to take effect. If you disable UPnP on all but one of your nodes then your local router will forward inbound 8444 traffic to the one node and the rest will now be able to connect to the network but just will not accept inbound connections from the network.

# Why does my node have no connections?

If your node has no connections, it could be one of many reasons. You might need to disable upnp in the config file (~/.chia/VERSION/config/config.yaml) or by using the cli command `chia configure -upnp false`. You might have multiple nodes running on the same machine, or in the same wifi network. Make sure to close all chia appliations on your computer. Also check your firewall or antivirus software, which might be blocking connections. 

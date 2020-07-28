# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) goes deeply into the math and implementation considerations. A harvester can harvest multiple plots on the same machine. A farmer can then control multiple harvesters across many machines to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. You can examine the [Space Required section](https://www.chia.net/assets/proof_of_space.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes).

# What k-size should you plot?

We believe that plots created with Beta 1.8 and newer versions of the chia software will work on mainnet at launch. We are certain that the minimum plot size will be at most k=32. The original design assumed that k=30 will be the minimum plot size but there are no guarantees as we intend to speed up the time it takes to plot and that may mean we choose a minimum k value of 31 or 32.

The simple answer is:
k29 (and smaller) plots are definitely not going to work on mainnet.
K30 plots MIGHT work on mainnet.
K31 plots MIGHT work on mainnet.
k32 plots will definitely work on mainnet.

The Chia dev team is going to great lengths to speed up the plotting process--from now until mainnet. The goal is to keep it so that the top-of-the-line hardware takes at least 1 hour to plot the minimum k-size, and there's no way to cheat the system. That's why the minimum k-size isn't clear yet--because the plotting speed improvements haven't been implemented yet.

# What is recommended for plotting?

We think you will want to use NVMe SSD drives to create your plots on. Then you can migrate your plots off to whatever storage you want to keep them on long term. You could even load them on a Raspberry Pi with outdated USB 2.0 drives attached and they will Harvest and Farm just fine. PC World offers this great [background on current storage technologies](https://www.pcworld.com/article/2899351/everything-you-need-to-know-about-nvme.html) but this graph gives you a quick view of why we recommend NVMe SSD:
![NVMe SSD vs SATA](images/plotting-nvme-ssd.png "NVMe SSD is 5.5 times faster than SATA SSD")

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof by which the result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups, which are explained in this [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

# How do I tell if I'm farming correctly?

There are a couple of ways. First note that you'll need some k=29 or better plots on Beta chain (circa 4/4/20) to be able to win chia. One of the most obvious ways is to monitor your Wallet to see if you have coins coming in. Chia, like Bitcoin, locks new farming rewards for 200 blocks for security and re-org reasons. If you want chia more quickly, ask in #testnet with your Wallet Deposit Address and you'll usually get some 'seasoned' chia.

You can see what your farmer thinks its farming by looking in your debug.log for lines that look like:
```bash
Harvester src.harvester         : INFO     21:14:36.228 Farming plot plots/plot-0-27-87f55b056018e049276b59e571107dac848fcff250575c84b961a9e6bab3ad48.dat of size 27
Harvester src.harvester         : INFO     21:14:36.228 Farming plot plots/plot-1-27-c1977c1741bf36eda34c09e5fe81e6b701add1eac2772a32d9e30303737aac83.dat of size 27
```
Those will show up about every 20 minutes as harvester reporting which plots it's actively looking at. If you need to add new plots after plotting but while already harvesting other plots you can just run (the correctly renamed in Beta3) `chia-restart-harvester` which will restart harvester which re-reads your plots.yaml file and starts harvesting/farming all currently completed plots.

The other thing you can look for are proofs of space being sent from your harvester to your farmer in response to the next block challenge. Those look like this in the debug.log:
```bash
Harvester src.server.server     : INFO     21:14:47.041 -> challenge_response to peer ('127.0.0.1', 8447)
Harvester src.server.server     : INFO     21:14:47.042 -> challenge_response to peer ('127.0.0.1', 8447)
```
The command `grep` is very handy. To see all your harvester activity you can `grep Harvester ~/.chia/RELEASEDIR/log/debug.log` or to see which plots Harvester thinks it's farming you can `grep "Farming plot" ~/.chia/RELEASEDIR/log/debug.log`. If you're trying to match things with spaces, then you need the quotes for `grep`.


# How do I know if my plots are OK?
Run `chia-check-plots -n 100` to try 100 sample challenges for each plot. Each of your plots should return a number around 100, which means it found 100 proofs of space. If the plots are not found, check your `~/.chia/RELEASEDIR/config/plots.yaml` file, and make sure you have run `chia init`.

# How do I send or receive a transaction?

To send or receive chia, you should use the wallet software.
The wallet will show you your address and provide an interface for you to spend your chia funds.
Read about how to build and start the wallet GUI in our [quickstart guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide#run-a-full-node--farmer--harvester--wallet).

The wallet software also provides features related to coloured coins, and trade offers.

# Why is it recommended that a winning plot be deleted on mainnet?

There is a possible attack where an attacker who can co-ordinate N deep from the tip of the chain can try to coerce a winning farmer to re-write a historical transaction block. Additionally, having more than one set of rewards go to the same plot and farmer lowers the farmer's pseudonymity. We expect that by mainnet, plotting will both be much faster and most farmers will have large enough farms that re-plotting the space opened up by a winning plot will be quick enough and we plan to have the software automate the process up to and including kicking off a remote plotting process if the current hardware that a farmer or harvester are on is not up to the task of re-plotting.

# What are the next milestones?

We are now in the Beta testnet blockchain phase. During Beta you should expect continued improvements in ease of install, and support for and user interface for our reference smart transactions. We will have some over the wire protocol changes that will require hard forks but should migrate your existing plots and installations easily to the newer chain. Between now and quite a few months before mainnet launch, we will have to make (hopefully only) one change to the file format of proof of space plots. This will require re-plotting but we expect to support both plot formats for about a month. The new plot format is intended to be the same as mainnet so you will be able to get your plots in order before mainnet launch. We expect to launch mainnet at the end of 2020. We also plan to have an 4-6 week period after mainnet launch when no transactions are allowed but farming rewards will be occurring. This is to help the storage network stabilize and to reward our space farmers first.

# How can I contribute?

You should check out [CONTRIBUTING.md](https://github.com/Chia-Network/chia-blockchain/blob/master/CONTRIBUTING.md) in the repository but the quick answer is to please base your pull requests off the dev branch. The dev branch will only accept rebase merges or squash merges.

# How do I upgrade and keep my keys and plots?

Updating between beta releases is simple.

Make sure all chia processes (including plotting) are closed in task manager / activity monitor / terminals.

Then just follow the normal [installation instructions](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL). Keys, plots, and configs will auto-migrate.

Your main configuration files are stored in ~/.chia/$VERSION/config. During installation, these files will auto-migrate to the updated version. After installation, it's safe to delete the old $VERSION folder.

# Can I run this on a Raspberry Pi 3 or 4?

Yes, and here are the [instructions](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi). This project requires a 64 bit OS. Pi 3 and Pi 4 don't have AES acceleration on the CPU and plotting currently relies upon that acceleration. This means that the Pi will never be a good plotting host, but we have made AES acceleration optional so one can install and run harvesters, farmers, and full nodes on the Pi. Pi is also not a candidate for Timelords or VDF clients... We have not tested Pi 3 yet so any feedback would be welcomed.

# Wallet GUI won't start on my linux distribution.

Some linux distros don't allow Electron to install correctly. The error is some variant of `The SUID sandbox helper binary was found, but is not configured correctly`. Details and fixes are in [Electron Issue 17972](https://github.com/electron/electron/issues/17972). Thanks [@kargakis](https://github.com/kargakis). If you use the `sysctl` workaround it does not persist so you'll want to see how to change that [here](https://unix.stackexchange.com/questions/25382/make-changes-to-sys-persistent-between-boots).

# Why does chia-blockchain require python 3.7 or greater?

The codebase takes advantage of the newest async generators, especially async/await, which requires 3.7 or better. Python has a [walk through of Async IO](https://realpython.com/async-io-python/) and the related python 3.7 requirements.

# Upnp Error

Upnp is an optional setting that allows users to open a port in their router and therefore allow other nodes to connect to them. This is not required, since your node can still make outgoing connections without Upnp.

For some routers, Upnp is enabled automatically, but for others, you might have to go into your router settings and enable Upnp manually. Sometimes restarting the router is also necessary.

Another option is port forwarding, where you tell your router/NAT to forward requests on port 8444 to your machine.
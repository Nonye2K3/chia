# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) goes deeply into the math and implementation considerations. A harvester can harvest multiple plots on the same machine. A farmer can then control multiple harvesters across many machines to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. You can examine the [Space Required section](https://www.chia.net/assets/proof_of_space.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes).

# What is recommended for plotting?

We think you will want to use NVMe SSD drives to create your plots on. Then you can migrate your plots off to whatever storage you want to keep them on long term. You could even load them on a Raspberry Pi with outdated USB 2.0 drives attached and they will Harvest and Farm just fine. PC World offers this great [background on current storage technologies](https://www.pcworld.com/article/2899351/everything-you-need-to-know-about-nvme.html) but this graph gives you a quick view of why we recommend NVMe SSD:
![NVMe SSD vs SATA](images/plotting-nvme-ssd.png "NVMe SSD is 5.5 times faster than SATA SSD")

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof by which the result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups, which are explained in this [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

# What are the next milestones?

We are now in the Beta testnet blockchain phase. During Beta you should expect continued improvements in ease of install, and support for and user interface for our reference smart transactions. We will have some over the wire protocol changes that will require hard forks but should migrate your existing installations easily to the newer chain. Between now and quite a few months before mainnet launch, we will have to make (hopefully only) one change to the file format of proof of space plots. This will require re-plotting but we expect to support both plot formats for a month or two. The new plot format will be the same as mainnet so you will be able to get your plots in order before mainnet launch. We expect to launch mainnet at the end of 2020. We also plan to have an 8-10 week period after mainnet launch when no transactions are allowed but farming rewards will be occurring. This is to help the storage network stabilize and to reward our space farmers first.

# How can I contribute?

You should check out [CONTRIBUTING.md](https://github.com/Chia-Network/chia-blockchain/blob/master/CONTRIBUTING.md) in the repository but the quick answer is to please base your pull requests off the next branch. For example, after the beta-1.0 release, the beta-1.1 branch is used for development, etc. The master branch is updated with the latest release only.

# How do I upgrade and keep my keys and plots?

With the move from Alpha to Beta you should review these [notes on upgrading](https://github.com/Chia-Network/chia-blockchain/wiki/Upgrading-from-Alpha-to-Beta). We will be supporting tools to make upgrading between Beta releases easier though you may want to place your plots directory somewhere that's not the directory that 'config/config.yaml' of the 'chia-blockchain' directory for easier future migrations. You can set the path to each file in 'config/plots.yaml' individually or adjust set a 'plot_root' by uncommenting line 18 of 'config.yaml' and specifying the directory there.

An alternate method is to keep your plots/ directory outside of the chia-blockchain directory and symbolically link to them: `ln -s /Volumes/BigStorage/chia/plots /home/username/plots` 

# Can I run this on a Raspberry Pi 3 or 4?

Yes. Pi 3 and Pi 4 don't have AES acceleration on the CPU and plotting currently relies upon that acceleration. This means that the Pi will never be a good plotting host, but we have made AES acceleration optional so one can install and run harvesters, farmers, and full nodes on the Pi. We have not tested Pi 3 yet so any feedback would be welcomed.

# Why does chia-blockchain require python 3.7 or greater?

The codebase takes advantage of the newest async generators, especially async/await, which requires 3.7 or better. Python has a [walk through of Async IO](https://realpython.com/async-io-python/) and the related python 3.7 requirements.
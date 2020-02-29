# What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/wiki/Network-Architecture).

# What is a proof of space?

A proof of space is a proof that a farmer has allocated a portion of their storage in a way that is very difficult to create in real time but efficient to pre-compute and store on a hard drive. The [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) goes deeply into the math and implementation considerations. A harvester can harvest multiple plots on the same machine. A farmer can then control multiple harvesters across many machines to manage the whole "farm."

# What is k?

"k" is the space parameter that controls the size of plots. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. You can examine the [Space Required section](https://www.chia.net/assets/proof_of_space.pdf#page=15) of the [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf) for the calculation of how much space is required for a given k.

# How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes).

# What is a VDF/proof of time?

A VDF, also known as a proof of time, is a sequential operation that takes a prescribed amount of time to compute (and which cannot be accelerated by parallelism) and which produces an accompanying proof by which the result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups, which are explained in this [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

# How do I upgrade and keep my keys and plots?

The easiest method is to change the name of the existing chia-blockchain directory e.g. `mv chia-blockchain chia-blockchain-old` and then install the new release via `git clone https://github.com/Chia-Network/chia-blockchain.git`. Then copy the contents of your old plots/ and config/ directory into the new installation directory `cp -r chia-blockchain-old/config chia-blockchain/config` and the same for plots. Note that when moving to Alpha 1.4 it's not advisable to copy `config/config.yaml` directly as the file format has been updated.

An alternate method is to keep your plots/ and config/ directories outside of the chia-blockchain directory and symbolically link to them: `ln -s /Volumes/BigStorage/chia/plots chia-blockchain/plots`.

# Can I run this on a Raspberry Pi 3 or 4?

Yes. Pi 3 and Pi 4 don't have AES acceleration on the CPU and plotting currently relies upon that acceleration. This means that the Pi will never be a good plotting host, but we have made AES acceleration optional so one can install and run harvesters, farmers, and full nodes on the Pi. We have not tested Pi 3 yet so any feedback would be welcomed.

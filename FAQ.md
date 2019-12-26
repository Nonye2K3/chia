# Frequently asked questions
## What are harvesters, farmers, full nodes, and timelords?

You can read about each of them and the architecture in the [network architecture document](https://github.com/Chia-Network/chia-blockchain/blob/master/docs/3-chia-network-architecture.md).

## What are VDFs?

A VDF is a sequential operation that takes a prescribed amount of time to compute (and which
cannot be accelerated by parallelism) and which produces an accompanying proof by which the
result may be quickly verified. This must be done in a group, for which Chia uses ideal class groups, which are explained in this [class group document](https://github.com/Chia-Network/oldvdf-competition/blob/master/classgroups.pdf).

## What is k?

"k" is the space parameter. It is an integer for the following equation: `plot_size_bytes = C1 * 2^k(k + C2)` where C1 is constant 1 and C2 is constant 2. You can  read the details in the [Chia Proof of Space Construction document](https://www.chia.net/assets/proof_of_space.pdf).

## How big are plot sizes (k)?

You can see some example plot sizes, times to plot, and working space needed based on various k's in these [k size tables](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes).

## How do I start mongo on linux? 

We suggest using the [Community DB install instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) from Mongo. If you're having issues starting `mongod` see if mongod is already running - e.g. `ps ax | grep mongo`

## Can I run this on a Raspberry Pi 3 or 4?

Not yet. Pi 3 and Pi 4 don't have AES acceleration on the CPU and plotting currently relies upon that acceleration. This means that the Pi will never be a good plotting host, but we intend to make AES acceleration optional in a future release so one can install and run harvesters, farmers, and full nodes on the Pi.

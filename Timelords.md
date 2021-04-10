## Types of Timelords

There are two primary types of Timelords: Regular and Blueboxes.

The first is the core Timelord that takes in Proofs of Space and uses a single fastest core to doing squaring in a [class group of unknown order](https://github.com/Chia-Network/vdf-competition/blob/master/classgroups.pdf) as fast as possible. Beside each running VDF (referred to as a vdf_client in the application and source) is a set of proof generation threads that accumulate the proof that the time calculation's number of iterations was done correctly.

The second are Bluebox Timelords. Blueboxes are most any machine - especially things like old servers or gaming machines - that scour the historical chain looking for uncompressed proofs of time. So that the chain moves quickly, the regular Timelords use a faster method of generating proofs of time but the proofs are larger and take your Raspberry Pi that you're trying to sync up a lot more time and effort to validate and sync the blockchain. A Bluebox picks up an uncompressed Proof of Time and recreates it, but this time with the slower and more compact proofs generated at the end. Those are then gossiped around to everyone so they can replace the large and slow to verify Proofs of Time with the compact and much quicker to validate version of exactly the same thing.

## The Fastest Timelords

There are three known fastest Timelords seen so far. The fastest known and seen on the testnet blockchain was in approximately September of 2020 where it was generating VDF iterations at about 360,000 iterations per second (or ips.) It disappeared after a few weeks. We speculate that it was an Intel cloud customer playing with pre-release Rocket Lake CPUs that have two channels of [AVX-512 IFMA](https://en.wikipedia.org/wiki/AVX-512#IFMA) support. The Timelord source code has an implementation of IFMA but it's not enabled by default as the very few CPUs that do have one channel of IFMA don't gain much speed from it - primarily because they're mostly found in power saving laptops. The second "known" fast Timelord is an [academic research project](https://ieeexplore.ieee.org/abstract/document/9301680). They and we speculate that it may be in the 400k-500k ips range once modified to run our 1024 bit width version. They used the [TSMC](https://www.tsmc.com/english) 28-nm CMOS technology for their prototype. Until Rocket Lake is generally available (which is soon as of this writing in early March) the fastest Timelord is a water cooled Intel Core i9-10900K running un-overclocked with 16 GiB or RAM. Sooner or later we will overclock it... It maintains about 200k-210k ips.

## Running a Timelord

First of all, the network only requires one running Timelord to keep moving (liveness.) The way Timelords race is like they are on a series of 100 meter dashes. Each one takes off with the last good Proof of Space and tries to get to the total number of iterations required to complete a given Proof of Space. Better Proofs of Space require less iterations to prove. When the fastest Timelord announces the Proof of Time for this Proof of Space all of the other Timelords stop racing and are magically teleported to the starting line of the next 100 meter dash to start it all over again.

It's good to have a few Timelords out there. There can be things like routing flaps or the overzealous backhoe that takes large swaths of the internet offline. If the fastest Timelord was just about to win the current dash when its internet blinked off in a fury of construction misadventure, then the second fastest will win that dash and the next dashes - until the fastest returns. One of the key things about Proofs of Time is that given the same Proof of Space, their output and proof are always the same (though the proofs can be larger or smaller and harder or easier to validate - they all end up with the same outcome.)

The Company plans to run a few Timelords around the world - and some backups too - just to make sure that all Farmers and nodes can hear the beat that the Timelords are calling.

## Installing a Timelord

### Regular Timelords

Due to restrictions on how [MSVC](https://en.wikipedia.org/wiki/Microsoft_Visual_C%2B%2B) handles 128 bit numbers and how Python relies upon MSVC, it is not possible to build and run Timelords of all types on Windows - yet. We have a plan to use GCC and some tools to enable vdf_client on Windows in a way that will be compatible with a Windows install of chia-blockchain. However it's a bit convoluted to get it working right. On MacOS x86_64 and all Linux distributions, building a Timelord is as easy as running `sh install-timelord.sh` in the venv of a `git clone` style chia-blockchain install. Try `./vdf_bench square_asm 400000` once you've built Timelord to give you a sense of your optimal and unloaded ips. Each run of `vdf_bench` can be surprisingly variable and, in production, the actual ips you will obtain will usually be about 20% lower due to load of creating proofs. The default configuration for Timelords is good enough to just let you start it up. Set your log level to INFO and then grep for "Estimated IPS:" to get a sense of what actual ips your Timelord is achieving. We will shortly modify the Timelord build process to support MacOS ARM64 as well - which is a cakewalk compared to Windows...

### Bluebox Timelords

For now, Blueboxes are also restricted to basically anything but Windows. Our plans to port to Windows will make Blueboxes available there as well though. Once you build the Timelord with `sh install-timelord.sh` in the venv, you will need to make two changes to `~/.chia/VERSION/config.yaml`. In the `timelord:` section you will want to set `sanitizer_mode:` to `True`. Then you need to proceed to the `full_node:` section and set `send_uncompact_interval:` to something greater than 0. We recommend `300` seconds there so that your Bluebox has some time to prove through a lot of the un-compacted Proofs of Time before the node drops more into its lap. The default settings may otherwise work but if the total effort is a little too much for whatever machine you are on you can also lower the `process_count:` from 3 to 2, or even 1, in the `timelord_launcher:` section. You know it is working if you see `VDF Client: Sent proof` in your logs at INFO level.

## The Future of Timelords

Having an open source ASIC Timelord that everyone can buy inexpensively is the Company's goal. We had originally expected that we would proceed from general purpose CPUs to FPGAs and then ASICs. It turns out that squaring in class groups of unknown order at 1024 bit widths is both FPGA hard and slightly ASIC hard. It also was a pleasant surprise that Intel's AVX-512 IFMA was almost perfectly created for this application. As such we will be fostering ASIC efforts over the medium term. We are happy to lose money on an ongoing project to create and enhance an open source PCI card that would be available for say $250 for anyone who wishes to run the fastest Timelords in the world too.

## Timelords and Attacks

One of the things that is great about the [Chia new consensus](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) is that it makes it almost impossible for a Farmer with a maliciously faster Timelord to selfishly Farm. Due to the way new consensus works, a Farmer with a faster Timelord is basically compelled to prove time for all the farmers winning blocks around him also. Having an "evil" faster Timelord can give a benefit when attempting to 51% attack the network, so it is still important that over time we push the Timelord speeds as close to the maximum speeds of the silicon processes available. We expect to have the time and the resources to do that right and make open source hardware versions widely available.

## Terminology
* VDF: verifiable delay function, another way to say "proof of time"
* Timelord launcher: a small program which launches "vdf client" processes over and over, to perform the actual vdf calculations.
* VDF client: a c++ process which performs a VDF computation and then shuts down
* Timelord: The timelord is communicates with the node, and is what decides which VDF tasks to assign to which clients. The vdf clients connect through HTTP to the timelord. So you can have the timelord in a separate machine as the timelord launcher
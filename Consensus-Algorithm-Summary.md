**NOTE**: The core consensus components of Chia are changing to version 1.1 as documented in our [new consensus working doc](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit).

The Chia blockchain and consensus algorithm aims to provide a more environmentally friendly,
decentralized, and secure alternative to proof of work or proof of stake, while
maintaing some of the key properties that make Nakamoto consensus desirable. A
description of the algorithm can be reviewed in the [Chia Network greenpaper](https://www.chia.net/assets/ChiaGreenPaper.pdf) and an updated view is available in our [new consensus working doc](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit).

The main idea is that nodes called **Farmers** (as opposed to Bitcoin's miners), use
their disk space to compete on finding blocks. Whereas in Bitcoin, owning 5% of the hashpower, or
CPU power allows you to win 5% of the blocks, in Chia having 5% of the allocated hard drive
*space* will allow you to win these blocks. Furthermore, coinbase rewards and fees in blocks
are given to the farmers (and/or pools).

This allocated hard drive space is stored in a series of files referred to as "plots".
Plots are lookup tables filled with
hashes and pointers, which allow farmers to efficiently look up and find proofs of space which are cryptographic
proofs of storage of arbitrary data. Farmers can create plots through the plotting process, which can take
days of intensive CPU and disk usage. After that, they can farm with almost no cpu or electricity
usage. More information on proofs of space and the exact construction can be found [here](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) and the running source code is in the [chiapos repository](https://github.com/Chia-Network/chiapos).

Whenever a new block is propagated through the network, all farmers check their hard drives to
see if they have any very good proofs of space (analogous to someone checking their bingo card to
see if they've won), and propagate these proofs if they've found a lucky number. Proofs of Space are judged on how little time a timelord would need to prove on top of them. Farmers also propagate a block
with these proofs, and sign it with a private key that is associated with their plot.

In order to prevent grinding attacks and long range attacks amongst others, these proofs of space must be put through a
set of proofs of time as well. Each block has one proof of space and one proof of time.
**Proofs of time**, or verifiable delay function proofs ("VDFs"), are cryptographic proofs that a sequential
computation was performed on a given input, for a given number of iterations. These proofs of
time create time between blocks, and make generating an alternative blockchain take a very long time. The nodes
that create proofs of time are called **Timelords**, and they don't get any direct rewards for doing this. The
idea is that they help the network operate, and as long as there is one honest timelord that is close
enough to the fastest timelord, then grinding resistance is preserved.

A block which does not yet have a proof of time on it, is called an **unfinished block**.

In summary, blocks are created by farmers/timelord with only a proof of space, and the best ones are sent to timelords which can add proofs of time to them, where the time required is a random number that depends on the block time, difficulty, proof of space quality, etc.

## Difficulty and Iterations

In Chia, there is also a difficulty parameter which is a constant factor that can increase or decrease
the number of iterations required for the proof of time, and therefore change expected block times.

### Signage points and infusion points

Each sub-slot in the challenge and reward chains is divided into SIGNAGE_POINTS_PER_SUB_SLOT, which is set to 64, smaller VDFs, and between each of these small VDFs is a point called a signage point. Timelords publish the VDF output and proof when they reach each signage point. Note that both the challenge chain and the reward chains have signage points (but not the infused challenge chain). The number of iterations between each signage point is sp interval iterations, which is equal to floor(sub slot iterations / SIGNAGE_POINTS_PER_SUB_SLOT).

When a challenge is broadcast to nodes in the network at the start of each sub-slot, farmers check each of their plots to see if they pass the plot filter, and then as each of the 64 signage points is reached, they are broadcast through the network by timelords and nodes, and farmers compute a plot filter based on the signage point, their plot id, and the sub-slot challenge. If the plot filter bits start with 9 zeroes, that plot passes the filter for that signage point, and can proceed. This disqualifies around 511/512 of all plot files in the network, for that signage point.

`plot filter bits = sha256(plot id + sub slot challenge + signage point)`

The proof of space challenge is computed as the hash of the plot filter bits:

`pos challenge = sha256(plot filter bits)`

Using this challenge, the farmers fetch quality strings for each plot that made it past the filter from disk. Recall that this process is almost instant, and that the signage point is a hash derived from part of the proof of space (but the whole proof of space is not retrieved yet).

The farmer computes the required iterations for each proof of space. If the required iterations < sp interval iterations, the proof of space is eligible for inclusion into the blockchain, so the farmer fetches the entire proof of space from disk (which takes longer than only fetching the quality), creates an unfinished sub block, and broadcasts it to the network. Note that the vast majority of required iterations will be way too high, since on average 32 will qualify for the whole network for each sub slot. This is a random process so it's possible for a large number of proofs to qualify, but very unlikely. The signage point iterations is the number of iterations from the start of the sub-slot to the signage point.

The infusion iterations is the number of iterations from the start of the sub slot at which the block with the quality above can be included into the blockchain. This is calculated as:

```
infusion iterations =( signage point iterations + 3 * sp interval iterations + required iterations)  %  sub slot iterations
```

Therefore, the infusion iterations will be between 3 and 4 signage points after the signage point. Farmers must submit their proofs and blocks before the infusion point is reached. The modulus is there to allow overflows into the next sub-slot, if the signage point is near the end of the sub-slot. This is expanded on later.

At the infusion point, the farmer's block gets combined with the infusion point VDF output to create a new input for the VDF from that point on, i.e. we infuse the farmer’s block into the VDF. The block is only fully valid after the infusion iterations has been reached, and the VDF proof has been attached to the block.

For the b1 block to be valid/finished, two VDF proofs must be included: one from r1 to the signage point and one from r1 to b1. (actually it’s more since there are three VDF chains, explained later).  In Figure 5, the farmer creates at the time of the signage point, (let’s call it B1’). However, B1’ is not finished yet, since it needs the infusion point VDF. Once the infusion iterations VDF is released, it is added to B1’ to form the finished block at B1.

* **Sp interval iterations:** Defined as floor(sub-slot iterations / SIGNAGE_POINTS_PER_SUB_SLOT=32).

Signage points: 64 intermediary points in time within a sub-slot in the challenge and reward chains, for which VDFs are periodically released. At each signage point, a VDF output is created and broadcast through the network. The first signage point in the sub-slot is the challenge itself. Each block has a signage point such that the proof of space in the block must be eligible for that signage point.

* **Required iterations:** A number computed using the quality string, used to choose proofs of space which are eligible to make blocks. The vast majority of proofs of space will have required iterations which are too high, and thus not eligible for inclusion into the chain. This number is used to compute the infusion point.

* **Infusion point:** the point in time at infusion iterations from the challenge point, for a proof of space with a certain challenge and infusion iterations. At this point, the farmer’s block gets infused into the reward chain VDF. The infusion point of a block is always between 3 and 4 signage points after the signage point of that block. Computed as signage point iterations + 3 * sp interval iterations + required iterations.
 
The delay between the signage point and infusion point has many benefits, including defense against orphaning and selfish farming, decreased forks, and no VDF pauses. This delay of around 30 seconds is given so that farmers have enough time to sign without delaying the slot VDF. Well behaving farmers sign only one signage point with each proof of space, meaning that attackers cannot easily reorg the chain.

* **qual_str** is the quality string, is a variable sized bytestring that can be efficiently retrieved from
the plot (it's actually a subset of the proof of space). This allows farmers to efficiently check whether
a proof of space is "good" (requires low iterations), without fetching the whole proof from disk. A quality
lookup takes around 50ms on a slow HDD, while a proof of space lookup takes around 500ms. Note that this
is like finding a good hash in Bitcoin, but does not require elecricity, and can be done instantly.

* **Challenge chain:** The VDF chain based on each challenge for each sub-slot, which does not infuse anything in the middle of each sub-slot. The challenges are also used for the proofs of space. The signage points in this chain are used for the SP filter.

* **Reward chain:** The VDF chain that contains infusions of all blocks. This chain pulls in the challenge chain and optionally the infused challenge chain at the end of each sub-slot.

* **Infused challenge chain:** A VDF chain which starts at the first block infused in a slot (which is not based on the previous slot’s challenge, this is called the challenge block) and ends at the end of the slot. 

* **Slot:** the list of sub-slots which contain at least 16 reward-chain blocks based on the challenge of the first sub-slot, or later sub-slots. At the end of the slot, the infused challenge chain stops, the challenge chain pulls in the result of the infused challenge chain, and the deficit is reset to 16.

* **Block:** a block is a collection of data infused into the rewards chain which contains: a proof of space for a challenge hash with less iterations than the slot iterations, sp and ip VDFs for both chains, optional ip VDF for the infused challenge chain, and a rewards address. All blocks are also blocks. There is a maximum of 128 blocks per slot.

* **Transaction Block:** A block that is eligible to create transactions, along with an associated list of transactions.

* **Challenge block:** The first block to be infused in each slot, which is not based on a previous slot's challenge. The challenge block always has a deficit of 15, and always starts off the infused challenge chain. 

* **k** is an integer between 32 and 59 which determines the size of a plot. For testnet **k** may be a lower value.

* **expected_plot_size** is a function from k to the number of bytes on disk to store a plot of that size.
Increasing k by one roughly doubles the size of the plot.

* **Peak:** The peak of the blockchain as seen by a node is the block with the greatest weight. Weight is the sum of the difficulty of a block and all it’s ancestors, which is similar to height, but a shorter chain can have heavier weight, due to difficulty adjustments.

For a block to be considered valid, it has to provide VDFs for the challenge chain and reward chain, and optionally for the infused challenge chain if it is present. Forcing all VDFs to be included means that all three chains are guaranteed to move forward at the same rate.

## Minimum block requirement

A minimum of 16 current-slot challenge blocks must be infused into the rewards chain in order for a slot to be finished.

The deficit is a number between 0 and 16 that is present at the start of a sub-slot. This is defined as the number of reward chain blocks that we need to infuse in order to finish a slot. It is reset to 16 whenever we start a slot (so there must be at least 16 total blocks per challenge chain infusion). The deficit goes down for each reward chain infusion that is based on a current-slot challenge. 

The block with deficit 15 is a challenge block.

The normal case is where the deficit starts at 16, and goes down to zero within the sub-slot, and resets back to 16 as we finish the slot and start a new one. In the case that we don't manage to reduce it to 0 within the end of the slot, the challenge chain and infused challenge chain (if present) continue, and the deficit does not reset to 16. Blocks (including overflow blocks now), keep subtracting from the deficit until we reach 0. When we finish a sub-slot with a zero deficit, the infused challenge chain is included into the challenge chain, and the deficit is reset to 16.

This requirement is added to prevent long range attacks, and is described in detail in the Countermeasures section below. The vast majority of sub-slots will have >=5 blocks, therefore it does not affect normal operation very much. 

## Weight

The weight of a block is the sum of the difficulty of this block, plus all previous blocks that are ancestors of this block. Honest full nodes must choose the peak of the blockchain such that the peak is the block with the heaviest weight that they know of. This is a crucial requirement, and is identical to Bitcoin’s heaviest chain rule. Due to this rule, an attacker with less than 50% of the space and no VDF advantage will have trouble earning more than their fair share, since they must get lucky and create more reward chain blocks than the honest chain. Furthermore, farmers only farm on the challenges that correspond to the heaviest chain.

Both VDF speed and total amount of space are important for weight, and changes in these can trigger difficulty adjustments. If the amount of space increases, more than 32 blocks per slot will be created, so the difficulty has to be increased. If the network VDF speed increases, more than 32 blocks are created every 10 minutes, and thus the difficulty (and the sub-slot iterations) has to be increased.  

A farmer with exclusive access to a slightly faster VDF, however, cannot easily get more rewards than a farmer with the normal speed VDF. If an attacker tries to orphan one of the blocks on the chain, having a faster VDF will not help, since the attacker’s chain will have less blocks (and thus a lower weight). Farmers must sign the block which they are building on top of, and they will only build on top of the highest weight chain. 

The VDF speed comes into play when the attacker wishes to launch a 51% attack, however. In this case, an attacking farmer can use the VDF to create a completely alternate chain with no honest blocks, and overtake the honest chain.

## Epochs & difficulty adjustment

* **Sub-epoch:** Sub-epoch N starts when sub-epoch N-1 ends (except for 0th sub-epoch), and it ends at the end of the first slot where 384 * (N+1) blocks have been included since genesis. 

* **Epoch:** Epoch N starts when epoch N-1 ends (except for 0th epoch), and it ends at the end of the first slot where 32256 * (N + 1) blocks have been included since genesis.

* **Difficulty:** A constant that scales the number of iterations for a given proof of space. Iterations are computed as difficulty / quality. 

Every 32256 blocks, the difficulty adjustment kicks in. This modifies two parameters: The slot_iterations parameter, and the difficulty parameter. 

The sub_slot_iterations parameter is reset so a 300 second slot requires close to slot_iterations many iterations. The reset is done using the values from the last epoch to approximate the iterations per second ration, concretely.

For the remainder of the explanation of Difficulty resets please start a the top of page 16 in the [new consensus working document](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit).



The next document in the tutorial is the [Block Format](https://github.com/Chia-Network/chia-blockchain/wiki/Block-Format).
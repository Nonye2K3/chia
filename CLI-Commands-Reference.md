This page should provide additional high-level documentation and explanation beyond just `chia -h`.

This is not meant to be comprehensive, because often the `-h` text is clear enough. We recommend fully investigating `-h` before looking elsewhere.

As with the rest of this project, this doc is a work-in-progress. Feel free to browse the [source code](https://github.com/Chia-Network/chia-blockchain/tree/master/src/cmds) for more insight in the meantime.

# [init](https://github.com/Chia-Network/chia-blockchain/blob/master/src/cmds/init.py)

First, `init` checks for old versions of chia installed in your ~/.chia directory.

If so, `init` migrates these old files to the new version:
* config (including old SSL files)
* db
* wallet
* Using config.yaml, updates wallet keys and ensures coinbase rewards go to the right wallet puzzlehash.

If no old version exists, `init`:
* Creates a default chia configuration
* Initializes a new SSL key and cert (for secure communication with the GUI)

# plots

## [create](https://github.com/Chia-Network/chia-blockchain/blob/master/src/plotting/create_plots.py)

Command: `chia plots create [add flags and parameters]`

**Flags**

-k [size]: Define the size of the plot(s). For a list of k-sizes and creation times on various systems check out: [k-Sizes](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)

-n [number of plots]: The number of plots that will be made. Be sure you calculate both final plot and temporary plot sizes.

-b [memory buffer size MiB]: Define memory/RAM usage. Default is 2000 (2G). More RAM will marginally increase speed of plot creation.

-f [farmer pk]: Development tool. Disregard for non-development usage.

-p [pool pk]: Development tool. Disregard for non-development usage.

-t [tmp dir]: Define the temporary directory for plot creation.

-2 [tmp dir 2]: Define a secondary temporary directory for plot creation.

-d [final dir]: Define final location for plot(s)

## [check](https://github.com/Chia-Network/chia-blockchain/blob/master/src/plotting/check_plots.py)

First, this looks in all plot directories from your config.yaml. You can check those directories with `chia plots show`.

This will scan all of your plots sequentially. There's currently no way to specify to check any particular plot. If you'd like to do so, you'll either need to manually edit your config.yaml, or temporarily move all your other plots (except the desired plot) to another directory or subdirectory.

If you don't include an `-n` integer, the default is 20. `-n` represents the number of challenges given. They're sequential from 0 to `-n`, not random.

For instance, if `-n` is 20, then 20 challenges will be given to each plot.

Each plot will take each challenge and:
* Get the quality for the challenge (Is there a proof of space? There should be 1, but there may be 0 or more than 1.)
* Get the full proof(s) for the challenge if a proof was present
* Validate that the # of full proofs matches the # of expected quality proofs.

Finally, you'll see a report the final true proofs vs. expected proofs.

Therefore, if `-n` is 20, you would expect 20 proofs, but your plot may have more or fewer.

Running the command with `-n 10` or `-n 20` is good for a very minor check, but won't actually give you much information about if the plots are actually high-quality or not.

Consider using `-n 1000` to get a better idea, but be warned that this will take a fairly long time if you're scanning multiple plots.

For more detail, you can read about the DiskProver commands in [chiapos](https://github.com/Chia-Network/chiapos/blob/master/src/prover_disk.hpp)

**What does the ratio of full proofs vs expected proofs mean?**
* If the ratio is >1, your plot is relatively lucky, and probably has better odds of winning a block than another plot of equal space.
* If the ratio is <1, your plot is relatively unlucky, and probably has worse odds of winning a block than another plot of equal space.
    * This shouldn't really concern you unless your ratio is <0.70
This page should provide additional high-level documentation and explanation beyond just `chia -h`.

This is not meant to be comprehensive, because often the `-h` text is clear enough. We recommend fully investigating with the `-h` switch before looking elsewhere.

As with the rest of this project, this doc is a work-in-progress. Feel free to browse the [source code](https://github.com/Chia-Network/chia-blockchain/tree/master/src/cmds) or the [Chia Proof of Space Construction Document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) for more insight in the meantime.

# [init](https://github.com/Chia-Network/chia-blockchain/blob/master/src/cmds/init.py)

Command: `chia init`

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

`-k` [size]: Define the size of the plot(s). For a list of k-sizes and creation times on various systems check out: [k-Sizes](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)

`-n` [number of plots]: The number of plots that will be made, in sequence. Once a plot is finished, it will be moved to the final location `-d`, before starting the next plot in the sequence.

`-b` [memory buffer size MiB]: Define memory/RAM usage. Default is 2048 (2GiB). More RAM will marginally increase speed of plot creation. Please bear in mind that this is what is allocated to the plotting algorithm alone. Code, container, libraries etc. will require additional RAM from your system.

`-f` [farmer pk]: Development tool. Disregard for non-development usage.

`-p` [pool pk]: Development tool. Disregard for non-development usage.

`-t` [tmp dir]: Define the temporary directory for plot creation. This is where Plotting Phase 1 (Forward Propagation) and Phase 2 (Backpropagation) both occur. The `-t` dir requires the largest working space: normally about 5 times the size of the final plot.

`-2` [tmp dir 2]: Define a secondary temporary directory for plot creation. This is where Plotting Phase 3 (Compression) and Phase 4 (Checkpoints) occur. Depending on your OS, `-2` might default to either `-t` or `-d`. Therefore, if either `-t` or `-d` are running low on space, it's recommended to set `-2` manually. The `-2` dir requires an equal amount of working space as the final size of the plot.

`-d` [final dir]: Define the final location for plot(s). Of course, `-d` should have enough free space as the final size of the plot. This directory is automatically added to your `~/.chia/VERSION/config/config.yaml` file. You can use `chia plots remove -d` to remove a final directory from the configuration.

**Example**

Example below will create a k30 plot and use 4GB (note - not GiB) of memory.

`chia plots create -k 30 -b 4000 -t /path/to/temporary/directory -d /path/to/final/directory`

**Additional Plotting Notes**

* During plotting, Phase 1 (Forward Propagation) and Phase 3 (Compression) tend to take the most time. Therefore, to maximize plotting speed, `-t` and `-2` should be on your fastest drives, and `-d` can be on a slow drive.

* Currently, plotting only uses 1 CPU thread. Therefore, most Chia users have determined it's more efficient to plot in parallel, rather than series. You can do this by just having multiple plotting instances open.

* It's objectively faster to plot on SSD's instead of HDD's. However, SSD's have significantly more limited lifespans, and early Chia testing has seemed to indicate that plotting on SSD's wears them out pretty quickly. Therefore, many Chia users have decided it's more "green" to plot in parallel on many HDD's at once.

* Plotting is designed to be as efficient as possible. However, to prevent grinding attacks, farmers should not be able to create a plot within the average block interval. That's why the minimum k-size is k32 on mainnet.

## [check](https://github.com/Chia-Network/chia-blockchain/blob/master/src/plotting/check_plots.py)

Command: `chia plots check -n [num checks] -g [substring]`

First, this looks in all plot directories from your config.yaml. You can check those directories with `chia plots show`.

`-g` allows you to specify to check only plots with directory or file name containing case-sensitive [substring]. If `-g` isn't specified all plots in every directory will be checked.

`-n` represents the number of challenges given. If you don't include an `-n` integer, the default is 20. For instance, if `-n` is 20, then 20 challenges will be given to each plot. The challenges count from 0 to `-n`, and are not random.

Each plot will take each challenge and:
* Get the quality for the challenge (Is there a proof of space? You should expect 1 proof per challenge, but there may be 0 or more than 1.)
* Get the full proof(s) for the challenge if a proof was present
* Validate that the # of full proofs matches the # of expected quality proofs.

Finally, you'll see a report the final true proofs vs. expected proofs.

Therefore, if `-n` is 20, you would expect 20 proofs, but your plot may have more or fewer.

Running the command with `-n 10` or `-n 20` is good for a very minor check, but won't actually give you much information about if the plots are actually high-quality or not.

Consider using `-n 1000` to get a better idea, but be warned that this will take a fairly long time if you're scanning multiple plots.

Examples for using `-g`

> Long directory name like `/mnt/chia/DriveA` can use `chia plots check -g DriveA`
> Check only k33 plots you can use `chia plots check -g k33`
> Check plots created on October 31, 2020 can use `chia plots check -g 2020-10-31`

For more detail, you can read about the DiskProver commands in [chiapos](https://github.com/Chia-Network/chiapos/blob/master/src/prover_disk.hpp)

**What does the ratio of full proofs vs expected proofs mean?**
* If the ratio is >1, your plot was relatively lucky for this run of challenges.
* If the ratio is <1, your plot was relatively unlucky.
    * This shouldn't really concern you unless your ratio is <0.70

In theory, a plot with a ratio >> 1 would be more likely to win challenges on the blockchain. Likewise, a plot with a ratio << 1 would be less likely to win. However, in practice, this isn't actually going to be noticeable. "Number of plots" and "k-size" are much more influential factors at winning blocks than "proofs produced per challenge". Therefore, don't worry if your plot check ratios are less than 1, unless they're _significantly_ less than 1.
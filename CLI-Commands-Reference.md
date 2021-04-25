This page should provide additional high-level documentation and explanation beyond just `chia -h`.

This is not meant to be comprehensive, because often the `-h` text is clear enough. We recommend fully investigating with the `-h` switch before looking elsewhere.

As with the rest of this project, this doc is a work-in-progress. Feel free to browse the [source code](https://github.com/Chia-Network/chia-blockchain/tree/main/chia/cmds) or the [Chia Proof of Space Construction Document](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf) for more insight in the meantime.

# Locate the `chia` binary executable

## Mac

If you installed `Chia.app` in your `/Applications` directory, you can find the `chia` binary at `/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon/chia`.

Do a sanity check in `Terminal.app` with

`/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon/chia -h`

You can use that if you augment your `PATH` with

`
PATH=/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon:$PATH
`

and then `chia -h` should work.


## Windows

There is more than one `chia.exe` binary; the GUI is `Chia.exe` (two of these!) and the CLI is `chia.exe`. They are found in different places. Note the big C versus the little c.

The CLI one is the one referred to in this document, and for version 1.1.1 it can be found at

`~\AppData\Local\chia-blockchain\app-1.1.1\resources\app.asar.unpacked\daemon\chia.exe`

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

# start

Command: `chia start {service}`

* Service `node` will start only the full node.
* Service `farmer` will start the farmer, harvester, a full node, and the wallet.
* positional arguments:
  {all,node,harvester,farmer,farmer-no-wallet,farmer-only,timelord,timelord-only,timelord-launcher-only,wallet,wallet-only,introducer,simulator}

**Flags**

`-r, --restart`: Restart of running processes 

# plots

## [create](https://github.com/Chia-Network/chia-blockchain/blob/master/src/plotting/create_plots.py)

Command: `chia plots create [add flags and parameters]`

**Flags**

`-k` [size]: Define the size of the plot(s). For a list of k-sizes and creation times on various systems check out: [k-Sizes](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)

`-n` [number of plots]: The number of plots that will be made, in sequence. Once a plot is finished, it will be moved to the final location `-d`, before starting the next plot in the sequence.

`-b` [memory buffer size MiB]: Define memory/RAM usage. Default is 4608 (4.6 GiB). More RAM will marginally increase speed of plot creation. Please bear in mind that this is what is allocated to the plotting algorithm alone. Code, container, libraries etc. will require additional RAM from your system.

`-f` [farmer pk]: This is your "Farmer Public Key". Utilise this when you want to create plots on other machines for which you do not want to give full chia account access. To find your Chia Farmer Public Key use the following command: `chia keys show`

`-p` [pool pk]: This is your "Pool Public Key". Utilise this when you want to create plots on other machines for which you do not want to give full chia account access. To find your Chia Pool Public Key use the following command: `chia keys show`

`-a` [fingerprint]: This is the key Fingerprint used to select both the Farmer and Pool Public Keys to use. Utilize this when you want to select one key out of several in your keychain. To find your Chia Key Fingerprint use the following command: `chia keys show`

`-t` [tmp dir]: Define the temporary directory for plot creation. This is where Plotting Phase 1 (Forward Propagation) and Phase 2 (Backpropagation) both occur. The `-t` dir requires the largest working space: normally about 4 times the size of the final plot.

`-2` [tmp dir 2]: Define a secondary temporary directory for plot creation. This is where Plotting Phase 3 (Compression) and Phase 4 (Checkpoints) occur. Depending on your OS, `-2` might default to either `-t` or `-d`. Therefore, if either `-t` or `-d` are running low on space, it's recommended to set `-2` manually. The `-2` dir requires an equal amount of working space as the final size of the plot.

`-d` [final dir]: Define the final location for plot(s). Of course, `-d` should have enough free space as the final size of the plot. This directory is automatically added to your `~/.chia/VERSION/config/config.yaml` file. You can use `chia plots remove -d` to remove a final directory from the configuration.

`-r` [number of threads]: 2 is usually optimal. Multithreading is only in phase 1 currently.

`-u` [number of buckets]: More buckets require less RAM but more random seeks to disk. With spinning disks you want less buckets and with NVMe more buckets. There is no significant benefit from using smaller buckets - just use 128.

`-e` [bitfield plotting]: Using the `-e` flag will disable the bitfield plotting algorithm, and revert back to the older b17 plotting style. Using the `-e` flag (bitfield disabled) lowers memory requirement, but also writes about 12% more data during creation of the plot. For now, SSD temp space will likely plot faster with `-e` (bitfield back propagation disabled) and for slower spinning disks, i.e SATA 5400/7200 rpm, **not** using `-e` (bitfield enabled) is a better option.

**Example**

Example below will create a k32 plot and use 4GB (note - not GiB) of memory.

`chia plots create -k 32 -b 4000 -t /path/to/temporary/directory -d /path/to/final/directory`

Example 2 below will create a k34 plot and use 8GB of memory, 2 threads and 64 buckets

`chia plots create -k 34 -e -b 8000 -r 2 -u 64 -t /path/to/temporary/directory -d /path/to/final/directory`

Example 3 below will create five k32 plots one at a time using 4GB (note - not GiB) of memory.

`chia plots create -k 32 -b 4000 -t /path/to/temporary/directory -d /path/to/final/directory -n 5`


**Additional Plotting Notes**

* During plotting, Phase 1 (Forward Propagation) and Phase 3 (Compression) tend to take the most time. Therefore, to maximize plotting speed, `-t` and `-2` should be on your fastest drives, and `-d` can be on a slow drive.

* Currently, plotting only uses 1 CPU thread. Therefore, most Chia users have determined it's more efficient to plot in parallel, rather than series. You can do this by just having multiple plotting instances open.

* It's objectively faster to plot on SSD's instead of HDD's. However, SSD's have significantly more limited lifespans, and early Chia testing has seemed to indicate that plotting on SSD's wears them out pretty quickly. Therefore, many Chia users have decided it's more "green" to plot in parallel on many HDD's at once.

* Plotting is designed to be as efficient as possible. However, to prevent grinding attacks, farmers should not be able to create a plot within the average block interval. That's why the minimum k-size is k32 on mainnet.

## [check](https://github.com/Chia-Network/chia-blockchain/blob/master/src/plotting/check_plots.py)

Command: `chia plots check -n [num checks] -l -g [substring]`

First, this looks in all plot directories from your config.yaml. You can check those directories with `chia plots show`. This command will check whether plots are valid given the plot's associated keys and your machine's stored Chia keys, as well as test the plot with challenges to identify found plots vs. expected number of plots.

`-g` check only plots with directory or file name containing case-sensitive [substring].
**If `-g` isn't specified all plots in every plot directory in your config.yaml will be checked.**

Examples for using `-g`

* Check plots within a long directory name like `/mnt/chia/DriveA` can use `chia plots check -g DriveA`
* Check only k33 plots can use `chia plots check -g k33`
* Check plots created on October 31, 2020 can use `chia plots check -g 2020-10-31`

`-l` allows you to find duplicate plots by ID. It checks all plot directories listed in config.yaml and lists out any plot filenames with the same filename ending; `*-[64 char plot ID].plot`. You should use `-l -n 0` if you only want to check for duplicates.

`-n` represents the number of challenges given. If you don't include an `-n` integer, the default is 30. For instance, if `-n` is 30, then 30 challenges will be given to each plot. The challenges count from 5 (minimum) to `-n`, and are not random.

Each plot will take each challenge and:
* Get the quality for the challenge (Is there a proof of space? You should expect 1 proof per challenge, but there may be 0 or more than 1.)
* Get the full proof(s) for the challenge if a proof was present
* Validate that the # of full proofs matches the # of expected quality proofs.

Finally, you'll see a report the final true proofs vs. expected proofs.

Therefore, if `-n` is 20, you would expect 20 proofs, but your plot may have more or fewer.

Running the command with `-n 10` or `-n 20` is good for a very minor check, but won't actually give you much information about if the plots are actually high-quality or not.

Consider using `-n 30` to get a statistically better idea.

For more detail, you can read about the DiskProver commands in [chiapos](https://github.com/Chia-Network/chiapos/blob/master/src/prover_disk.hpp)

**What does the ratio of full proofs vs expected proofs mean?**
* If the ratio is >1, your plot was relatively lucky for this run of challenges.
* If the ratio is <1, your plot was relatively unlucky.
    * This shouldn't really concern you unless your ratio is <0.70 # If so, do a more thorough `chia plots check` by increasing your `-n` 

The plots check challenge is a static challenge. For example if you run a plots check 20 times, with 30 tries against the same file, it will produce the same result every time. So while you may see a plot ratio << 1 for a plot check with `x` number of tries, it does not mean that the plot itself is worthless. It just means that given these static challenges, the plot is producing however many proofs. As the number of tries (`-n`) increases, we would expect the ratio to not be << 1. Since Mainnet is live, and given that the blockchain has new challenges with every signage point - just because a plot is having a bad time with one specific challenge, does not mean it has the same results versus another challenge.  "Number of plots" and "k-size" are much more influential factors at winning blocks than "proofs produced per challenge". 

**In theory**, a plot with a ratio >> 1 would be more likely to win challenges on the blockchain. Likewise, a plot with a ratio << 1 would be less likely to win. However, in practice, this isn't actually going to be noticeable.  Therefore, don't worry if your plot check ratios are less than 1, unless they're _significantly_ less than 1 for _many_ `-n`. 


# Other commands (not yet documented)

```sh
$ chia

Options:
  --root-path PATH  Config file root  [default: /home/mariano/.chia/mainnet]
  -h, --help        Show this message and exit.

Commands:
  configure   Modify configuration
  farm        Manage your farm
  init        Create or migrate the configuration
  keys        Manage your keys
  netspace    Estimate total farmed space on the network
  plots       Manage your plots
  run_daemon  Runs chia daemon
  show        Show node information
  start       Start service groups
  stop        Stop services
  version     Show chia version
  wallet      Manage your wallet

```
To see what you can do with each of these commands, use the help flag -h. For example, `chia show -h`.

To check your full node status, do `chia show -s` and you'll see something like this. To figure how close
you are look at your height. Once fully synced it'll say `Full Node Synced` at the top.

```
Current Blockchain Status: Full Node Synced

Peak: Hash: 34554a10aff6b52545623e18667c9487758fa93a3b2345974da0d263939189dc
      Time: Tue Mar 23 2021 20:54:46 JST                  Height:      19882

Estimated network space: 136.225 PiB
Current difficulty: 9
Current VDF sub_slot_iters: 112197632
Total iterations since the start of the blockchain: 63291534050

  Height: |   Hash:
    19882 | 34554a10aff6b52545623e18667c9487758fa93a3b2345974da0d263939189dc
    19881 | f53c052cd7ac58539ff5c35cb9d515bc521308a49cec7566b23dba84f76009d8
    19880 | 924d825a7fdbfd61e4582efbbe1d977bb554b368eea58c349a71e688e43fcc49

```

You can add and remove directories for your plots with `chia plots add -d 'your_dir'` or `chia plots remove -d 'your_dir'`, help can be found for respective add/remove with `chia plots add/remove -h`

You can check contents of your wallet with: `chia wallet`, and status of your farmer with `chia farm summary`.

Check harvester and farmer logs: `grep ~/.chia/mainnet/log/debug.log -e harvester`

```
17:08:03.191 harvester harvester_server        : INFO     <- harvester_handshake from peer 214b269a425b8223cb50fbd458dab056599348e255f07a018c13ea9efb509ee5 127.0.0.1
17:08:03.194 farmer farmer_server              : INFO     -> harvester_handshake to peer 127.0.0.1 65f3fa0b0407a07da8ccf04dfa0f64c28f714726312aa051d3a8529390db4d7a
17:08:03.218 harvester src.plotting.plot_tools : INFO     Searching directories ['/home/user/slab1/plots']
17:08:03.227 harvester src.plotting.plot_tools : INFO     Found plot /home/user/slab1/plots/plot-k32-2021-01-11-17-26-bf2363828e469a3417b89eb98cfa9d694809e1ce8bef0ffd1d12853d4227aa0a.plot of size 32
17:08:03.227 harvester src.plotting.plot_tools : INFO     Loaded a total of 1 plots of size 0.09895819725716137 TiB
```


Maybe follow logs: `tail -F ~/.chia/mainnet/log/debug.log`. Chia is nice enough to rotate logs for you.
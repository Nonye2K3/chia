# Is my farm healthy? (*NIX and Linux Headless Edition)

Alot of smaller farmers are concerned about the health of their farm, when they can't find any proofs for days at a time.
This document was created to provide metrics to smaller farmers, so they can ensure their farm is working, even without finding any proofs.

## Check if your farm thinks its farming
Before going further, please make sure wether your farm actually considers itself to be farming. Theres a good chance that you might not since you are still syncing blocks.

To check the status of your farm,  `../activate` as usual and then type `chia farm summary`. If the first line of the ouput looks like like this:

```
Farming status: Farming
```
..then you know no broader errors have occured.

## Check if your plots are passing the filter
The most important metric to look out for is, wether your plots are passing the plot filter on your harvesting machines. In a usual setup, this involves checking the logs under `~/.chia/mainnet/log` to see if at least for some rounds, plots are marked as **eligible for farming** by the harvester.

Your `~/.chia/mainnet/log` directory may look like this:

```
username@chia-farmer:~/.chia/mainnet/log$ tree
.
├── debug.log
├── debug.log.1
├── debug.log.2
├── debug.log.3
├── debug.log.4
├── debug.log.5
├── debug.log.6
└── debug.log.7

0 directories, 8 files
```
Each logfile contains log information about all the services ran by chia. If you're running a full node, these can be convoluted. We're only interested wether or not plots pass the plot filter. We can check this, by running a command like:

``` cat debug.log | grep "[0-9] plots were eligible for farming"```

`cat` is a *nix program to get content of a file. With the pipe operator `|`we "pipe" the output to another program called `grep`which can filter textual input. We filter for `"[0-9] plots were eligible for farming"` to see if we already had eligible plots.

Example output may look like this:
```
09:55:43.847 harvester src.harvester.harvester : INFO     1 plots were eligible for farming 2d8b1c58a0... Found 0 proofs. Time: 0.13772 s. Total 100 plots
09:55:52.737 harvester src.harvester.harvester : INFO     3 plots were eligible for farming 2d8b1c58a0... Found 0 proofs. Time: 0.43679 s. Total 100 plots
09:56:01.646 harvester src.harvester.harvester : INFO     2 plots were eligible for farming 2d8b1c58a0... Found 0 proofs. Time: 0.14055 s. Total 100 plots
```
**If youre seeing output like above here, this is already good!**

It means that plots are passing the plot filter and your farm seems to work as intended. Do this for each logfile to see wether or not you had any outages or wether something went wrong.

## Checking for proofs
If you have had elligible plots in the past, theres a chance that you might have already found a proof, but it didn't get accepted by the network. 

**Please keep in mind that finding a proof does not constitute to winning a plot (getting a payout). Even if you find a proof, it needs to compete with other proofs and win to actually receive a reward.**

To check wether you have already found proofs, you can run the same command as before, but with a different filter:

`cat debug.log | grep "Found [1-9] proofs"`

If you do this for all your logfiles and get a result, **great!** This means your farm is 100% working as expected. You might now have won a block yet, but you already came very close once, or a few times!

**Note:** The author would like to give example output, but as of writing of this doc has not yet found a proof.
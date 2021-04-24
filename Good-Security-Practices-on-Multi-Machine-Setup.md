(This is basically repeating what @mariano54 said in [this discussion](https://github.com/Chia-Network/chia-blockchain/discussions/1116#discussioncomment-420398).)

Security is about making better choices. You can never be 100% secure, but you can always make better choices to become more secure.

# Keep Your Keys Separate

In other words, _only use the keys specific to your machine's purpose_. 
* Your master/farming key should not be in your plotting machine(s). 
* Your master/farming key should not be in your harvester machine(s).

## Farming On Multiple Machines

### Plotting On Multiple Machines
Buried in the [Farming on many machines](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) wiki page is this relevant tidbit:
> When creating plots on the other harvesters, use `chia plots create -f farmer_key -p pool_key`, inserting the farmer and pool keys from your main machine. Alternatively, you could copy your private keys over by using chia keys add, but this is less secure.

### Harvesting On Multiple Machines
Follow the instructions on setting up certificates on harvesters on the [Farming on many machines](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines) wiki page.

# Keep Your Wallet Separate

One way to not get your wallet hacked is to not have it accessible to the internet. 

> Your reward address for chia rewards should be a separate key as well, kept in an offline machine. You can generate an address on a different computer, and put this address in the config.yaml (farmer.xch_target_address and pool.xch_target_address), so if your farming machine gets hacked, you don't lose past rewards. ([Source](https://github.com/Chia-Network/chia-blockchain/discussions/1116#discussioncomment-420398))

## How to Find Your Keys
You'll need to [use the CLI](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference). Use this command to list all your keys: `chia keys show`
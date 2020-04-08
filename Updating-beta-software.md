If you have not yet upgraded from the alpha releases, please follow this guide:
[Upgrade from alpha to beta](https://github.com/Chia-Network/chia-blockchain/wiki/Upgrading-from-Alpha-to-Beta).

If you do not have the software or plots, and want to install from scratch, follow the instructions in [README.md](https://github.com/Chia-Network/chia-blockchain/blob/master/README.md).

Otherwise, to update to the newest beta release (from an earlier beta release), do the following, from a terminal in the chia-blockchain directory:
1. `. ./activate && chia-stop-all`
2. `git pull origin master`
3. chia init 
4. Follow instructions to start your node in [README.md](https://github.com/Chia-Network/chia-blockchain/blob/master/README.md) (`chia init`, `chia-start-farmer &`, etc)

This should copy the plot configuration (plots.yaml), key configuration (keys.yaml), and node configuration (config.yaml) from the old chia directories into the new chia directory. Note that your plots are not moved, only the references to them.

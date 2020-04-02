Before upgrading we suggest you shut down the alpha with e.g. `sh scripts/stop_all_servers.sh` and then rename the `chia-blockchain/` directory to `chia-blockhain-alpha`. In MacOS and Linux the command is `mv chia-blockchain chia-blockchain-alpha` from your home directory (if that's where you put chia-blockchain.) You may also want to make a backup copy of the alpha directory - e.g. `tar -czvf chia-blockchain-alpha.tgz chia-blockchain-alpha` from your home directory on MacOS or Linux.

If you don't have a lot of plots or are going to re-plot anyway, you can just follow the instructions in [INSTALL.md](https://github.com/Chia-Network/chia-blockchain/blob/master/INSTALL.md) for your platform, and then follow the [README.md](https://github.com/Chia-Network/chia-blockchain/blob/master/README.md). Note that plot's remain functional on the beta but the key files changed slightly.

If you want to use the same plots as before, do the following:

* Make sure you've changed the name of the chia-blockchain directory as above.

* Once you have installed the Beta and activated the virtual environment with `. ./activate` go ahead and generate a new key file with `chia-generate-keys`, That will create a config.yaml and a keys.yaml that is usually found in `~/.chia/beta-1.0/config/config.yaml` and `~/.chia/beta-1.0/config/keys.yaml`.

* Copy the `chia-blockchain-alpha/config/plots.yaml` file to the new config directory - `cp chia-blockchain-alpha/config/plots.yaml ~/.chia/beta-1.0/config/`

* Move the plots if necessary to `~/.chia/beta-1.0/plots`. You may have to create the new plots directory with `mkdir ~/.chia/beta-1.0/plots`. If you've used a symbolic link you can now just create a symbolic link in `~/.chia/beta-1.0/`. Double check that all of the paths to your plots are correct in the just copied over plots.yaml for your new plot location (if you move them to `~/.chia/beta-1.0/plots`.)

* In your new keys.yaml, you want to copy *only* the two pool_sks, and sk_seed from the old config in `chia-blockchain-alpha/config/keys.yaml` into the new config at `~/.chia/beta-1.0/config/keys.yaml`. Make sure to keep the format of this file correct.

That should allow you to run `chia-start-farmer &` and have farming begin. Then run `chia-start-wallet-gui &` on MacOS or Linux desktops or `chia-start-wallet-server&` and run the chia.exe on Windows WSL 2.
If you have not yet upgraded from the alpha releases, please follow this guide:
[Upgrade from alpha to beta](https://github.com/Chia-Network/chia-blockchain/wiki/Upgrading-from-Alpha-to-Beta).

If you do not have the software or plots, and want to install from scratch, follow the instructions in [INSTALL](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL) from the wiki.

Before you update, if you have set the CHIA_ROOT env variable, you should unset it before continuing [checking and unsetting](https://www.cyberciti.biz/faq/linux-osx-bsd-unix-bash-undefine-environment-variable/). Also remove it from ~/.bashrc or ~/.zshrc if you have added it before.

Otherwise, to update to the newest beta release (from an earlier beta release), do the following, from a terminal in the chia-blockchain directory:
1. `. ./activate && chia-stop-all`
2. `git fetch && git pull origin master`
3. `sh install.sh`
4. `. ./activate`
5. It is wise to make a backup of your keys.yaml and plots.yaml before continuing.
6. `chia init`
7. Follow instructions to start your node in the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) (`chia start farmer &`, etc)

This should copy the plot configuration (`plots.yaml`), key configuration (`keys.yaml`), and node configuration (`config.yaml`) from the old chia directories into the new chia directory. Note that your plots are not moved, only the references to them, but farming should still work.

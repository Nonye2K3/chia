# Large plots fail issue

In modifying proof of space to make it plot faster on Windows native we introduced a regression such that plots of k=32 or larger error out when attempting to farm them. While debugging that issue we found a memory leak/run out of memory problem on some invalid larger plots. Both issues are fixed with the 0.12.10 release of chiapos.

If you have plots of k=32 or greater here are the quick steps you can take to work around the issue until beta 1.5 is released.

## Windows native

Stop all chia services.
```
cd "~\AppData\Local\Programs\Chia Network\Chia Blockchain\"
.\venv\Scrips\Activate.ps1
pip uninstall chiapos
pip install chiapos
```
That will give you a warning about the chiapos version but will run.

## Ubuntu, WSL 2, & MacOS
Stop all chia services.
```
cd chia-blockchain
. ./activate
pip uninstall chiapos
nano setup.py
```
In nano (or the editor of your choice) you want to change `chiapos==0.12.8` to `chiapos==0.12.10` and then save setup.py (control-s, control-x.)
```
sh install.sh
export CHIA_ROOT=~/.chia/beta1.0b4
```

And now you can go back to plotting and farming. You may want to add `export CHIA_ROOT=~/.chia/beta1.0b4` to your .bashrc or .zshrc. When you upgrade you will need to unset the $CHIA_ROOT environment variable.

If you experienced the memory leak, you should run `chia-check-plots` to make sure all of your plots are valid.

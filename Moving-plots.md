Plots can be moved from one folder to another, and even to another machine. If you want to move plots within the same machine:

1. Move the plot file 
2. Add the new plot directory. If you use command line, do `chia plots add -d '/Users/example/folder'`. If you use the UI, go to the Farmer tab and click "Add plots", and select the new directory.

If you want to move plots to a different machine:
1. Install chia on the new machine
2. Find your private keys using `chia keys show` on the old machine, or on the UI by clicking on "Keys".
3. Copy the 24 words (this is your private key) and add them to the new machine using `chia keys add -m word1 word2 ... word24`
4. Move the plot file
5. Add the new plot directory. If you use command line, do `chia plots add -d '/Users/example/folder'`. If you use the UI, go to the Farmer tab and click "Add plots", and select the new directory.


After following these steps, and restarting your farmer, the new plots should be visible. You can check if they are working by running `chia plots check` or using the UI and checking if the plots are visible. If you would like to farm on many machines, but only run one full node and farmer (to save disk space, bandwidth, and keep private keys safer), you should run one harvester per machine.

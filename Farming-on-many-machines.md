This guide allows you to run a harvester on each machine, without having to run a full node, wallet, and farmer on each one. This keeps your system simpler, uses less bandwidth, space, CPU, and also keeps your keys safer.

The architecture is composed of one machine which runs the farmer, full node, and wallet, and other machines which run only the harvester. Only your main machine will connect to the Chia network.

```                                          
                                       _____  Harvester 1
                                      /
other network peers  --------   Main machine ------  Harvester 2
                                      \_____  Harvester 3
```
1. First, make sure Chia is installed on all machines. Ensure you have private keys by running CLI `chia keys show`. 
2. When creating plots on the other harvesters, use `chia plots create -f farmer_key -p pool_key`, inserting the farmer and pool keys from your main machine. Alternatively, you could copy your private keys over by using `chia keys add -m 24words`, but this is less secure. After creating a plot, run `chia plots check` to ensure everything is working correctly.
3. Copy the `trusted.crt` and `trusted.key` file from `~/.chia/beta-1.0bx/config/` in your main machine, to all harvester machines in the same location. This allows connection the the main machine, securely.
4. Make sure your main machines IP address on port 8447 is accessible by your harvester machines
5. Open the `~/.chia/beta-1.0bx/config/config.yaml` file in each harvester, and enter your main machine's IP address in the harvester's farmer_peer section
6. Launch the harvester by running CLI `chia start harvester` and you should see a new connection on your main machine's Farming tab.
7. To stop the harvester, you run CLI `chia stop harvester`

Note:
Currently (beta14), the GUI doesn't show harvester plots currently. Best way to see if it's working is to check the log on your main machine `~/.chia/beta-1.0bx/log/debug.log` and see if you get messages like `[time stamp] harvester src.harvester           : WARNING  KeyError plot [directory of your harvester + plot file name] does not exist.`. That means it knows you have a plot that has a proof that can solve the challenge hash, but it can't find the file locally but it's reading the information from your harvesters properly, knowing the file exists somewhere in your harvester network.
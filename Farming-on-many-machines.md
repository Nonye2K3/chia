**`As of beta 26 release, this process needs additional updating - you can no longer remote connect harverster as described below, they need ssl certificates generated from the farmers CA. This is after the introduction of ssl to the connections.`**


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
Currently (beta22), the GUI doesn't show harvester plots. The best way to see if it's working is shut down Chia full node and set your logging level to `INFO` in your `config.yaml` on your main machine and restart Chia full node. Now you can check the log `~/.chia/beta-1.0bx/log/debug.log` and see if you get messages like the following:
```
[time stamp] farmer farmer_server   : INFO   -> new_signage_point to peer [harvester IP address] [peer id - 64 char hexadecimal]
[time stamp] farmer farmer_server   : INFO   <- new_proof_of_space from peer [peer id - 64 char hexadecimal] [harvester IP address]
```
The new_signage_point message states the farmer sent a challenge to your harvester. The new_proof_of_space message states the harvester found a proof for the challenge. You will get more new_signage_point messages than new_proof_of_space messages.
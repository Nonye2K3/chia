This guide allows you to run a harvester on each machine, without having to run a full node, wallet, and farmer on each one. This keeps your system simpler, uses less bandwidth, space, CPU, and also keeps your keys safer.

The architecture is composed of one machine which runs the farmer, full node, and wallet, and other machines which run only the harvester. Only your main machine will connect to the Chia network.

```                                          
                                       _____  Harvester 1
                                      /
Chia network  --------   Main machine ------  Harvester 2
                                      \_____  Harvester 3
```
1. First, make sure Chia is installed on all machines. Ensure you have private keys by running `chia keys show`. 
2. When creating plots on the other harvesters, use `chia plots create -f farmer_key -p pool_key`, inserting the farmer and pool keys from your main machine. Alternatively, you could copy your private keys over by using `chia keys add -m 24words`, but this is less secure. After creating a plot, run `chia plots check` to ensure everything is working correctly.
3. Copy the trusted.crt file from `~/.chia/beta-1.0bx/config/trusted.crt` in your main machine, to all harvester machines in the same location. This allows connection the the main machine, securely.
4. Make sure your main machines IP address on port 8447 is accessible by your harvester machines
5. Open the `~/.chia/beta-1.0bx/config/config.yaml` file in each harvester, and enter your main machine's IP address in the harvester's farmer_peer section


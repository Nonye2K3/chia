_Updated for beta 27 release to support TLS_

This guide allows you to run a harvester on each machine, without having to run a full node, wallet, and farmer on each one. This keeps your system simpler, uses less bandwidth, space, CPU, and also keeps your keys safer.

The architecture is composed of one main machine which runs the farmer, full node, and wallet, and other machines which run only the harvester. Only your main machine will connect to the Chia network.

To secure communication between your harvester and **main** machine, TLS is used where your **main** machine will be the private Certification Authority (CA) that signs all certificates. Each harvester must have it's own signed certificate to properly communicate with your **main** machine.

```                                          
                                       _____  Harvester 1 (certificate A)
                                      /
other network peers  --------   Main machine (CA) ------  Harvester 2 (certificate B)
                                      \_____  Harvester 3 (certificate C)
```
* First, make sure Chia is installed on all machines and initialized by running the CLI `chia init`. 
* When creating plots on the other harvesters, use `chia plots create -f farmer_key -p pool_key`, inserting the farmer and pool keys from your main machine. Alternatively, you could copy your private keys over by using `chia keys add -m 24words`, but this is less secure. After creating a plot, run `chia plots check` to ensure everything is working correctly.
* Make a copy of your **main** machine CA directory located in `~/.chia/beta-1.0bx/config/ssl/ca` to be accessible by your harvester machines; you can share the `ssl/ca` directory on a network drive, USB key, or do a network copy to each harvester.

Then for each harvester, follow these steps:
1. Make sure your **main** machines IP address on port 8447 is accessible by your harvester machines
2. Open the `~/.chia/beta-1.0bx/config/config.yaml` file in each harvester, and enter your main machine's IP address in the remote harvester's farmer_peer section
3. Shut down all chia daemon processes with `chia stop all -d`
4. Run `chia init -c [directory]` on your remote harvester, where `[directory]` is the the copy of your **main** machine CA directory. This command creates a new certificate signed by your **main** machine's CA.
5. Launch the harvester by running CLI `chia start harvester` and you should see a new connection on your main machine in your INFO level logs.
6. To stop the harvester, you run CLI `chia stop harvester`

Warning:
You cannot copy the entire `config/ssl` directory from one machine to another. Each harvester must have a different set of TLS certificates for your **main** machine to recognize it as different harvesters. Unintended bugs can occur, including harvesters failing to work properly when the **same** certificates are shared among different machines.

Security Concern:
In beta27, the CA files are copied to each harvester. This is not ideal, and a new way to distribute certificates will be implemented in a subsequent release.

Note:
Currently (beta27), the GUI doesn't show harvester plots. The best way to see if it's working is shut down Chia full node and set your logging level to `INFO` in your `config.yaml` on your main machine and restart Chia full node. Now you can check the log `~/.chia/beta-1.0bx/log/debug.log` and see if you get messages like the following:
```
[time stamp] farmer farmer_server   : INFO   -> new_signage_point to peer [harvester IP address] [peer id - 64 char hexadecimal]
[time stamp] farmer farmer_server   : INFO   <- new_proof_of_space from peer [peer id - 64 char hexadecimal] [harvester IP address]
```

The new_signage_point message states the farmer sent a challenge to your harvester. The new_proof_of_space message states the harvester found a proof for the challenge. You will get more new_signage_point messages than new_proof_of_space messages.

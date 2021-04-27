_Updated for mainnet release to support TLS_

This guide allows you to run a harvester on each machine, without having to run a full node, wallet, and farmer on each one. This keeps your system simpler, uses less bandwidth, space, CPU, and also keeps your keys safer. It also makes your overall farm quicker and more efficient when replying to challenges.

The architecture is composed of one main machine which runs the farmer, full node, and wallet, and other machines which run only the harvester. Only your main machine will connect to the Chia network.

To secure communication between your harvester and **main** machine, TLS is used where your **main** machine will be the private Certification Authority (CA) that signs all certificates. Each harvester must have its own signed certificate to properly communicate with your **main** machine.

```                                          
                                       _____  Harvester 1 (certificate A)
                                      /
other network peers  --------   Main machine (CA) ------  Harvester 2 (certificate B)
                                      \_____  Harvester 3 (certificate C)
```
* First, make sure Chia is installed on all machines and initialized by running the CLI `chia init`. 
* When creating plots on the other harvesters, use `chia plots create -f farmer_key -p pool_key`, inserting the farmer and pool keys from your main machine. Alternatively, you could copy your private keys over by using `chia keys add`, but this is less secure. After creating a plot, run `chia plots check` to ensure everything is working correctly.
* Make a copy of your **main** machine CA directory located in `~/.chia/mainnet/config/ssl/ca` to be accessible by your harvester machines; you can share the `ssl/ca` directory on a network drive, USB key, or do a network copy to each harvester. You must copy the new `ssl/ca` directory with each version of `chia-blockchain`, so if you are upgrading from `beta` -> `mainnet` you must copy the new `ca` contents.

Then for each harvester, follow these steps:

1. Make sure your **main** machines IP address on port 8447 is accessible by your harvester machines
2. Shut down all chia daemon processes with `chia stop all -d`
3. Make a backup of any settings in your harvester
4. Run `chia init -c [directory]` on your harvester, where `[directory]` is the copy of your **main** machine CA directory. This command creates a new certificate signed by your **main** machine's CA.
5. Open the `~/.chia/mainnet/config/config.yaml` file in each harvester, and enter your main machine's IP address in the remote **`harvester`**'s farmer_peer section (NOT `full_node`).  
EX:
``` 
harvester:
  chia_ssl_ca:
    crt: config/ssl/ca/chia_ca.crt
    key: config/ssl/ca/chia_ca.key
  farmer_peer:
    host: Main.Machine.IP
    port: 8447
```
6. Launch the harvester by running CLI `chia start harvester -r` and you should see a new connection on your main machine in your INFO level logs.
7. To stop the harvester, you run CLI `chia stop harvester`

*Warning:*

You cannot copy the entire `config/ssl` directory from one machine to another. Each harvester must have a different set of TLS certificates for your **main** machine to recognize it as different harvesters. Unintended bugs can occur, including harvesters failing to work properly when the **same** certificates are shared among different machines.

*Security Concern:*

Since beta27, the CA files are copied to each harvester, as the daemon currently needs it to startup correctly. This is not ideal, and a new way to distribute certificates will be implemented in a subsequent release post mainnet launch. Please be careful when running your harvester that is accessible from the open internet.

*Note:*

Currently (mainnet), the GUI doesn't show harvester plots. The best way to see if it's working is shut down Chia full node and set your logging level to `INFO` in your `config.yaml` on your main machine and restart Chia full node. Now you can check the log `~/.chia/mainnet/log/debug.log` and see if you get messages like the following:
```
[time stamp] farmer farmer_server   : INFO   -> new_signage_point to peer [harvester IP address] [peer id - 64 char hexadecimal]
[time stamp] farmer farmer_server   : INFO   <- new_proof_of_space from peer [peer id - 64 char hexadecimal] [harvester IP address]
```

The new_signage_point message states the farmer sent a challenge to your harvester. The new_proof_of_space message states the harvester found a proof for the challenge. You will get more new_signage_point messages than new_proof_of_space messages.

If you are running the GUI and want to run multiple Harvesters
* Shut down Chia on main computer 
* Find your IP address on computer
* Make a copy of your **main** machine CA directory located in `c:\users\(your user name)\.chia\mainnet\ssl- copy the CA file` ; you can share the `ssl/ca` directory on a network drive, USB key, or do a network copy to each harvester. You must copy the new `ssl/ca` directory with each version of `chia-blockchain`-- copy the CA file to the harvester machine -- know its location 

* In new Harvester - follow steps below
* Load Chia and use your regular 24 key words to see that it works.  Then shut down chia
* In c:\users\(your user name)\.chia\mainnet\config file-- open it with notepad
* Change   enable_upnp: true-- change that to **false**
* Locate harvester: farmer_peer: host: localhost-- change only this location-- type in your main pc ip address  (ex 192.192.x.x)
* Locate the CA folder you copied from main computer-- know its network location.
* Go to command prompt.  Type in or copy **cd C:\Users\(your username)\AppData\Local\Chia-Blockchain\app-1.1.1\resources\app.asar.unpacked\daemon\**
* Make sure the (app-1.1.1) is the current version-- this is when version 1.1.1 is active
* Run `chia init -c [directory]` on your harvester, where `[directory]` is the copy of your **main** machine CA directory and its network lcoation. This command creates a new certificate signed by your **main** machine's CA.
* [directory] this is where you type the link to where your CA folder is stored-- if on the c drive then type for example c:\ca.  The full line would look like `chia init -c c:\ca`
* Then press enter.  Once that process is complete
*Start up both your main pc and the new harvester
* The new harvester may take a 10-20 minutes to start the sync procees- it will be a little bit slower- but should start to sync
and will make a full copy of the blockchain to get to normal sync.  You can create plots on that machine or copy plots over.  It will only farm once full sync is completed. 

To know its working
* On your main pc under Farm  tab- at the bottom select "Hide Advanced Options"- scroll down and " Your Harvester Network" wil now show (2) Node ID-- (1) your main pc and (2) your harvester
* also under farm tab under "Last Attempted Proof" your qty of  plots on your harvester will also show up there

**IMPORTANT**
* Chia does upgrades- if you are finding your harvester is not sync with blockchain or wallet-- you may have to re-copy the CA files again from main pc
* Run `chia init -c [directory]` on your harvester, where `[directory]` is the copy of your **main** machine CA directory and its network lcoation. This command creates a new certificate signed by your **main** machine's CA.

If you want to see that its working in the logs-- go to beginner guide -- see on right side and it will have directions on how to turn on logs to see transactions
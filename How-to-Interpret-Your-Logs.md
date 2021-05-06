A Chia blockchain node consists of several components that each handle different aspects of farming, harvesting, the wallet and general management of a node.  Each component creates entries in a single log file `debug.log`.  

## Log file location:
|OS|Location|
|---|---|
|Linux|`~/.chia/mainnet/log/debug.log`|
|Windows|`%systemdrive% %homepath% \.chia\mainnet\debug.log` (C:\Users\<username>\.chia…)|
|MacOS|`/Users/<username>/.chia/mainnet/log/debug.log`|

## Log file management:
By default, Chia allows debug.log to grow to 20MB, and then rotates the file by closing debug.log, renaming it do debug.log.1, and renames any existing older log files to debug.log.x, to a maximum of 7 logs.  After the seventh log is full, it is overwritten with the next earliest file using a maximum of 140MB of the most recent messages.  

## Log detail level:
Chia is shipped with the debug.log only containing messages at the WARN or ERROR level.  Many of the messages needed to fully monitor a node are only visible at the INFO level.  By stopping the node, changes to the logging level can be done in the `config.yaml` file in the `mainnet/config` folder.

## Change the log level output:
You are looking for a part of the file that looks like this:
```
farmer:
  logging: &id001
    log_filename: log/debug.log
    log_level: INFO
    log_stdout: false
```
If `log_level` has any other value than `INFO`, change it to `INFO`, save the file and restart the node.


## Node Components:
|Component|Function|
|---|---|
|farmer_server | Signage stuff about signs and things|<br>
|harvester_server|Gathers and shares plot information|<br> 
|timelord_server|Manages Verifiable Delay Functions for the node|<br>
|wallet_server|Controls wallet functions|<br>
|full_node_server|This component manages the node|<br>

## Log message format:
|Field|Content|
|---|---|
|Date/time | in ISO format, in local timezone|<br>
|Node Component | see the list above|<br>
|Log Level | ERROR, WARN, INFO|<br>
|Directional Arrow |<br>
|Message |see below<br>

## Log messages confirming node health:
1. “x plots were eligible for farming” – this message from harvester shows how the node responds to challenges.  The x value shows how many plots passed the initial filter [more on filters here.](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#what-is-the-plot-filter-and-why-didnt-my-plot-pass-it)  
> * The block prefix is shown, and the “Found y proofs.” The y value shows how many plots were accepted as proofs, and usually the value is zero. Most of the time if there is a proof you win, but not always as described [in the FAQ.](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#is-it-possible-to-have-a-proof-but-not-get-a-reward)
> * Next is “Time: x.xxx s.” which shows how long the node took to respond to the challenge.  The network requires a response in less than 28 seconds from the time the challenge was originated, so a recommended response time is less than 5 seconds. If this value is greater than 3 seconds a warning will be displayed in the GUI. (Read more here:
> * Finally “Total x plots” shows the number of plots recognized by the node.  If this doesn't look right [check your plots are valid.](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#how-do-i-know-if-my-plots-are-ok)
2. “Updated Wallet peak to height x, weight y” message from the wallet_blockchain component.  Value x is the current height of the blockchain, and should match the Height shown in the `chia show -s` command.  This indicates that the node wallet is fully synced with the network.  If that is not the case [check here for a common solution.](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#why-is-my-wallet-not-synced-why-can-i-not-connect-to-wallet-from-the-gui)
3. <other key messages>

## Other normal log messages:

|Component	|Message	|Direction	|Destination	|Cross component|Comment|
|---|---|---|---|---|---|
mempool_manager	|add_spendbundle took x seconds|||||
mempool_manager	|It took x to pre validate transaction|||||
full_node|Added unfinished_block x, not farmed by us|||||
full_node|Already compactified block:|||||
full_node|Duplicate compact proof.  Height: x||||
full_node|Finished signage point x/64: ||||
full_node|Scanning the blockchain for uncompact blocks.||||
full_node|Updated peak to height x|||||
full_node_server|new_compact_vdf|to/from|peer|||
full_node_server|new_peak|to/from|peer|||
full_node_server|new_peak_timelord|to|localhost|from timelord_server||
full_node_server|new_peak_wallet|to|localhost|from wallet_server||
full_node_server|new_signage_point|to|localhost|from farmer_server||
full_node_server|new_signage_point_or_end_of_sub_slot|to/from|peer|||
full_node_server|new_transaction|to/from|peer|||
full_node_server|new_unfinished_block|to/from|peer|||
full_node_server|new_unfinished_block_timelord|to/from|localhost|||
full_node_server|request_block|to/from|peer|||
full_node_server|request_block_header|from|localhost|to wallet_server||
full_node_server|request_compact_vdf|to/from|peer|||
full_node_server|request_compact_proof_of_time|to|localhost|from timelord_server||
full_node_server|request_signage_point_or_end_of_sub_slot|to/from|peer|||
full_node_server|request_transaction|to/from|peer|||
full_node_server|request_unfinished_block|to/from|peer|||
full_node_server|respond_block|to/from|peer|||
full_node_server|respond_compact_vdf|to/from|peer|||
full_node_server|respond_signage_point|to/from|peer|||
full_node_server|respond_transaction|to/from|peer|||
full_node_server|respond_unfinished_block|to/from|peer|||
wallet_server|request_block_header|to|localhost|from full_node||
wallet_server|respond_block_header|from|localhost|to full_node||
wallet_server|new_peak_wallet|from|localhost|to full_node||
wallet_blockchain|Updated Wallet peak to height x, weight y||||
timelord_server|new_peak_timelord|from|localhost|to full_node||
timelord_server|new_unfinished_block_timelord|from|localhost|to full_node||
timelord_launcher|VDF client x: VDF Client: Discriminant =||||
VDF Client|Sending Proof, Sent Proof, Stopped everything!||||
harvester_server|farming_info|to/from|localhost|||
harvester_server|new_signage_point_harvester|from|localhost|to farmer_server||
harvester|x plots were eligible for farming||||
plot_tools|Loaded a total of x plots of size y in z seconds||||
plot_tools|Searching directories||||
farmer_server|new_signage_point|from|localhost|to full_node||
farmer_server|farming_info |from|localhost|to full_node||
farmer_server|new_signage_point_harvester|to|localhost|from harvester||

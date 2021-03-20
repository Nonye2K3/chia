The chia node and services come with a JSON rpc api server that allows you to access information and control the services. These are accessible via HTTP, WebSockets, or a python client. The ports can be configured in ~/.chia/mainnet/config/config.yaml. The rpc ports should not be exposed to the internet. 
TLS certificates are used to secure the communitation.

### Default Ports:
- Daemon: 55400
- Full Node: 8555
- Farmer: 8559
- Harvester: 8560
- Wallet: 9256

### HTTP/JSON
The certificates must be used when calling the RPCs from the command line, make sure to use the correct certificates for the services you are calling.
All endpoints are made with POST with JSON data. The response is a JSON dictionary with a success field, which can be true or false. 

### WebSockets
If you are using the Websockets API, you can go directly through the daemon, which routes requests. Each WebSocket message contains the following fields:

```json
{
    "command": "get_blockchain_state",
    "ack": false,
    "data": {},
    "request_id": "123456",
    "destination": "wallet",
    "origin": "ui",
}
```

### Python
Most of the rpc methods are accessible through the different client objects in src/rpc.
For examples of usage, see the command line implementation (chia wallet, chia show, etc).

### Javascript
A javascript client can be found here: https://github.com/freddiecoleman/chia-client.

# Some examples:

### Get blockchain state
```bash
# Request

curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key \
-d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_blockchain_state | python -m json.tool

# Response:

{
    "blockchain_state": {
        "difficulty": 7,
        "genesis_challenge_initialized": true,
        "mempool_size": 0,
        "peak": {... },
        "space": 73659118,
        "sub_slot_iters": 134217728,
        "sync": {
            "sync_mode": false,
            "sync_progress_height": 0,
            "sync_tip_height": 0,
            "synced": false
        }
    },
    "success": true
}


```

### Get next address

```bash
# Request

curl --insecure --cert ~/.chia/testnet/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet/config/ssl/wallet/private_wallet.key -d '{"wallet_id": 1, "new_address":true}'\
-H "Content-Type: application/json" -X POST https://localhost:9256/get_next_address | python -m json.tool

# Response

{
    "address": "xch1d67xq5ed99l4fk2cr33xlq7sj705amnjz9ymc68kw68x3m9qa69sk3ngur",
    "success": true,
    "wallet_id": 1
}

```
### Get Wallet Balance
```bash
# Request

curl --insecure --cert ~/.chia/testnet/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet/config/ssl/wallet/private_wallet.key -d '{"wallet_id": 1}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_wallet_balance | python -m json.tool

# Response
{
    "success": true,
    "wallet_balance": {
        "confirmed_wallet_balance": 0,
        "max_send_amount": 0,
        "pending_change": 0,
        "spendable_balance": 0,
        "unconfirmed_wallet_balance": 0,
        "wallet_id": 1
    }
}
(
```

# Full Api Reference

## Full Node
### get_blockchain_state
> Returns current information about the blockchain, including the peak, sync information, difficulty, mempool size, etc.
> Response
> ```json
> {"blockchain_state": {...}}
> ```
### get_block
> Gets a full block by header hash.
> Params
> ```
> header_hash: the header hash of the block to get
> ```
> Response
> ```json
> {"block": {...}}
> ```

### get_blocks
> Gets a list of full blocks.
> Params
> ```
> start: the start height
> end: the end height (non-inclusive)
> exclude_header_hash: whether to exclude the header hash in the response (default false)
> ```
> Response
> ```json
> {"blocks": [...]}
> ```

### get_block_record_by_height
> Retrieves a block record by height (assuming the height <= peak height).
> Params
> ```
> height: the height to get
> ```
> Response
> ```json
> {"block_record": [...]}
> ```

### get_block_record
> Retrieves a block record by header hash.
> Params
> ```
> header_hash: the block's header_hash
> ```
> Response
> ```json
> {"block_record": [...]}
> ```

### get_block_records
> Retrieves block records in a range.
> Params
> ```
> start: the start height
> end: the end height (non-inclusive)
> ```
> Response
> ```json
> {"block_records": [...]}
> ```

### get_unfinished_block_headers
> Retrieves recent unfinished header blocks.
> 
> Response
> ```json
> {"headers": [...]}
> ```

### get_network_space
> Retrieves an estimate of the total plotted space of all farmers, in bytes.
>
> Params
> ```
> older_block_header_hash: the start header hash
> newer_block_headre_hash: the end header hash
> ```
> Response
> ```json
> {"space": 100000000000000000}
> ```

### get_additions_and_removals
> Retrieves the additions and removals (state transitions) for a certain block. 
> Returns coin records for each addition and removal.
>
> Params
> ```
> header_hash: header hash of the block
> newer_block_headre_hash: the end header hash
> ```
> Response
> ```json
> {"additions": [...], "removals": [...]}
> ```

### get_initial_freeze_period
> Retrieves the initial freeze period for the blockchain (no transactions allowed).
>
> Response
> ```json
> {"INITIAL_FREEZE_PERIOD": 193536}
> ```

### get_network_info
> Retrieves some information about the current network.
>
> Response
> ```json
> {"network_name": "mainnet", "network_prefix":  "xch"}
> ```

### get_coin_records_by_puzzle_hash
> Retrieves a list of coin records with a certain puzzle hash.
>
> Params
> ```
> puzzle_hash: puzzle hash to search for
> start_height (optional): confirmation start height for search
> end_height (optional): confirmation end height for search
> include_spend_coins: whether to include spent coins too, instead of just unspent
> ```
> Response
> ```json
> {"coin_records": [...]}
> ```

### get_coin_record_by_name
> Retrieves a coin record by its name/id.
>
> Params
> ```
> name: coin name
> ```
> Response
> ```json
> {"coin_record": {...}}
> ```

### push_tx
> Pushes a transaction / spend bundle to the mempool and blockchain.
> Returns whether the spend bundle was successfully included into the mempool.
>
> Params
> ```
> spend_bundle: spend bundle to submit, in JSON
> ```
> Response
> ```json
> {"status": "SUCCESS"}
> ```

### get_all_mempool_tx_ids
> Returns a list of all transaction IDs (spend bundle hashes) in the mempool.
>
> Response
> ```json
> {"tx_ids":  [...]}
> ```

### get_all_mempool_items
> Returns all items in the mempool.
>
> Response
> ```json
> {"mempool_items":  [...]}
> ```

### get_mempool_item_by_tx_id
> Gets a mempool item by tx id.
>
> Params
> ```
> tx_id: spend bundle hash
> ```
> Response
> ```json
> {"mempool_item": {...}}
> ```

## Wallet
### log_in
> Sets a key to active.
### get_public_keys
> Get all root public keys accessible by the wallet.
### get_private_key
> Get all root private keys accessible by the wallet.
### generate_mnemonic
> Generate a 24 word mnemonic phrase, used to derive a private key.
### add_key
> Add a private key to the keychain
### delete_key
> Delete a private key from the keychain
### delete_all_keys
> Delete all private keys from the keychain
### get_sync_status
> Gets the sync status of the wallet.
### get_height_info
> Gets information about the current height of the wallet.
### farm_block
> Farms a block, only available with the simulator.
### get_initial_freeze_period
> Retrieves the initial freeze period for the blockchain (no transactions allowed).
>
> Response
> ```json
> {"INITIAL_FREEZE_PERIOD": 193536}
> ```

### get_network_info
> Retrieves some information about the current network.
>
> Response
> ```json
> {"network_name": "mainnet", "network_prefix":  "xch"}
> ```
### get_wallets
> Gets a list of wallets for this key.
### create_new_wallet
> Creates a new wallet for this key.
### get_wallet_balance
> Retrieves balances for a wallet
### get_transaction
> Gets a transaction record by transaction id
### get_transactions
> Gets transaction records
### get_next_address
> Gets a new (or not new) address
### send_transaction
> Sends a standard transaction to a target puzzle_hash.
### create_backup
> Creates a backup for this wallet.
### get_transaction_count
> Gets the number of transactions in this wallet.
### get_farmed_amount
> Gets information about farming rewards for this wallet.

## Harvester
### get_plots
> Gets a list of plots being farmed on this harvester.
>
> Response
> ```json
> {"plots": [...], "failed_to_open_filenames": [...], "not_found_filenames": [...]}
> ```

### refresh_plots
> Refreshes the plots, forces the harvester to search for and load new plots.

### delete_plot
> Deletes a plot file and removes it from the harvester.
>
> Params
> ```
> filename: name of the file to delete
> ```

### add_plot_directory
> Adds a plot directory (not including sub-directories) to the harvester and configuration.
> Plots will be loaded and farmed eventually.
>
> Params
> ```
> dirname: absolute path of the directory to add
> ```

### get_plot_directories
> Returns all of the plot directoried being farmed.
>
> Response
> ```json
> {"directories": []}
> ```

### remove_plot_directory
> Removes a plot directory from the config, does not actually delete the directory.
>
> Params
> ```
> dirname: absolute path of the directory to remove
> ```

## Farmer
### get_signage_point
> Gets a signage point by signage point hash, as well as any winning proofs.
>
> Params
> ```
> sp_hash: the hash of the challenge chain signage point
> ```
> Response
> ```json
> {"signage_point": {...}, "proofs": [...]}
> ```

### get_signage_points
> Gets a list of recent signage points as well as winning proofs.
>
> Response
> ```json
> {"signage_points": [...]}
> ```

### get_reward_targets
> Gets the addresses that the farmer is farming to.
> 
> Params
> ```
> search_for_private_key: whether to check if we own the private key 
> for these addreses. Can take a long time{, and not guaranteed to return True.
> ```
> Response
> ```json
> {
>   "farmer_target": "xch1..",
>   "pool_target": "xch1..", 
>   "have_farmer_sk": true,
>   "have_pool_sk":  true
> }
> ```

### set_reward_targets
> Sets the reward targets in the farmer and configuration file.
> 
> Params
> ```
> farmer_target: farmer target address
> pool_target: pool target address
> ```

## Shared
### get_connections
> Returns a list of peers that we are currently connected to.
> 
> Response
> ```json
> {"connections": [...]}
> ```


### open_connection
> Opens a connection to another peer.
> 
> Params
> ```
> host: ip or dns name of the peer
> port: port of the peer
> ```


### close_connection
> Closes a connection with a peer.
> 
> Params
> ```
> node_id: node id of the peer
> ```

### stop_node
> Stops the node

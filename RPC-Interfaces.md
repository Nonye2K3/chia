# RPCs
The chia node and services come with a JSON rpc api server that allows you to access information and control the services. These are accessible via HTTP, WebSockets, or a python client. The ports can be configured in ~/.chia/mainnet/config/config.yaml. The rpc ports should not be exposed to the internet. 

## Default Ports:
- Daemon: 55400
- Full Node: 8555
- Farmer: 8559
- Havester: 8560
- Wallet: 9256

## HTTP/JSON
The certificates must be used when calling the RPCs from the command line, make sure to use the correct certificates for the services you are calling.
All endpoints are made with POST with JSON data. The response is a JSON dictionary with a success field, which can be true or false. 

## WebSockets
If you are using the Websockets API, you can go directly through the daemon, which routes requsts. Each WebSocket message contains the following fields:

```json
    message = {
        "command": "get_blockchain_state",
        "ack": false,
        "data": {},
        "request_id": "123456",
        "destination": "wallet",
        "origin": "ui",
    }
```


# Some examples:

### Get blockchain state
```bash
# Request

curl --insecure --cert ~/.chia/mainnet/config/ssl/fullnode/private_full_node.crt \
--key ~/.chia/mainnet/config/ssl/fullnode/private_full_node.key \
-d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_blockchain_state

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

curl --insecure --cert ~/.chia/testnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/testnet/config/ssl/wallet/private_wallet.key -d '{"wallet_id": 1, "new_address":true}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_next_address | python -m json.tool

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

curl --insecure --cert ~/.chia/testnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/testnet/config/ssl/wallet/private_wallet.key -d '{"wallet_id": 1}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_wallet_balance | python -m json.tool

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
(v
```

# Full Api Reference

## Full Node
### get_blockchain_state
### get_block
### get_blocks
### get_block_record_by_height
### get_block_record
### get_block_records
### get_unfinished_block_headers
### get_network_space
### get_additions_and_removals
### get_initial_freeze_period
### get_network_info
### get_coin_records_by_puzzle_hash
### get_coin_record_by_name
### push_tx
### get_all_mempool_tx_ids
### get_all_mempool_items
### get_mempool_item_by_tx_id

## Wallet
### log_in
### get_public_keys
### get_private_key
### generate_mnemonic
### add_key
### delete_key
### delete_all_keys
### get_sync_status
### get_height_info
### farm_block
### get_initial_freeze_period
### get_network_info
### get_wallets
### create_new_wallet
### get_wallet_balance
### get_transaction
### get_transactions
### get_next_address
### send_transaction
### create_backup
### get_transaction_count
### get_farmed_amount

## Harvester
### get_plots
### refresh_plots
### delete_plot
### add_plot_directory
### get_plot_directories
### remove_plot_directory

## Farmer
### get_signage_point
### get_signage_points
### get_reward_targets
### set_reward_targets

## Shared
### get_connections
### open_connection
### close_connection
### stop_node

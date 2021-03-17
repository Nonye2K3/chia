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

curl --insecure --cert ~/.chia/testnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/testnet/config/ssl/wallet/private_wallet.key -d '{"wallet_id": 1, "new_address":true}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_next_address | python -m json.tool
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

The endpoints currently supported in Node:
```
/get_blockchain_state
/get_block
/get_header_by_height
/get_header
/get_unfinished_block_headers
/get_connections
/open_connection
/close_connection
/stop_node
/get_unspent_coins
/get_heaviest_block_seen
```
Documentation is limited but one can get a sense for parameters and actions from the [Node RPC server source](https://github.com/Chia-Network/chia-blockchain/blob/274f88ba144f18e27a2f8868c3c2d9035c2df66b/src/rpc/full_node_rpc_server.py#L366).

`chia show` [implements](https://github.com/Chia-Network/chia-blockchain/blob/master/src/cmds/show.py) most of the RPC functionality on the command line.

# Farmer RPC
```
# See the most recent challenges

curl -d '{"":""}' -H "Content-Type: application/json" -X POST http://localhost:8559/get_latest_challenges
```

# Harvester RPC
```
# See the current plots farming

curl -d '{"":""}' -H "Content-Type: application/json" -X POST http://localhost:8560/get_plots
```
You can refer to the [harvester rpc client source](https://github.com/Chia-Network/chia-blockchain/blob/main/src/rpc/harvester_rpc_client.py#L14) to see the additional endpoints available.
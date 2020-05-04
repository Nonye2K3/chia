Full node has an
[HTTP JSON-RPC](https://github.com/Chia-Network/chia-blockchain/wiki/Networking-and-Serialization#rpc)
api to access information and control the full node:

Some examples:
```bash
curl -X POST http://localhost:8555/get_blockchain_state
curl -d '{"header_hash":"4ba1f698836798bda170364d3b3e8bb9fe1134eb1af8260ab1319d3ede52555e"}' -H "Content-Type: application/json" -X POST http://localhost:8555/get_header
```

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
Documentation is limited but one can get a sense for parameters and actions from the [RPC server source](https://github.com/Chia-Network/chia-blockchain/blob/master/src/rpc/rpc_server.py#L247).

`chia show` [implements](https://github.com/Chia-Network/chia-blockchain/blob/master/src/cmds/show.py) most of the RPC functionality on the command line.
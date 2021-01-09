# Full node
Full node has an [HTTP JSON-RPC](https://github.com/Chia-Network/chia-blockchain/wiki/Networking-and-Serialization#rpc)
api to access information and control the full node:

Some examples:
```bash
# Get blockchain state
curl -d '{"":""}' -H "Content-Type: application/json" -X POST http://localhost:8555/get_blockchain_state

# Get blockchain state in Beta 19 and beyond.
curl --insecure --cert ~/.chia/VERSION/config/trusted.crt --key ~/.chia/VERSION/config/trusted.key\
 -d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_blockchain_state

# Get block header by header hash
curl -d '{"header_hash":"4ba1f698836798bda170364d3b3e8bb9fe1134eb1af8260ab1319d3ede52555e"}' -H "Content-Type: application/json" -X POST http://localhost:8555/get_header

# Get all unspent coins by puzzle hash
curl -X POST --data '{"puzzle_hash":"0xa4259182b4d8e0af21331fc5be2681f953400b6726fa4095e3b91ae8f005a836"}'   http://localhost:8555/get_unspent_coins

# Connect to introducer.beta.chia.net
curl -d '{"host":"introducer.beta.chia.net","port":"8444"}' -H "Content-Type: application/json" -X POST http://localhost:8555/open_connection
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
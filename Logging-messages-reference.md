# debug.log

| Module | Sub-module | Level | Message | Description |
| ---      | ---      | --- | --- | --- |
|daemon | asyncio  | ERROR |   Task exception was never retrieved future: `<Task finished coro=<WebSocketServer.statechanged() done, defined at src\daemon\server.py:316> exception=ValueError('list.remove(x): x not in list')>`
|full_node |asyncio                 | ERROR |   SSL error in data received protocol: `<asyncio.sslproto.SSLProtocol object at 0x7f762544a8>`
|full_node |full_node_server        | ERROR |   Exception: Failed to fetch block `N` from {'host': `IP ADDRESS`, 'port': 8444}, timed out, {'host': `IP ADDRESS`, 'port': 8444}. | Connection to peer failed, other peer connections will take over|
|full_node |full_node_server            | WARNING | [Errno 32] Broken pipe `IP Address`
|full_node |full_node_server            | INFO    | Connection closed: `IP Address`, node id: `hex`
|full_node |full_node_server            | WARNING | Cannot write to closing transport `IP Address`
|full_node |src.full_node.full_node     | INFO    | ⏲️  Finished signage point `N`/64: `hex`
|full_node |src.full_node.full_node     | INFO    | Added unfinished_block `hex`, not farmed
|harvester |src.plotting.plot_tools    | WARNING | Not farming plot `plotfilename`. Size is `filesize` GiB, but expected at least: 99.06 GiB. We assume the file is being copied.| Periodic scan for new plots have discovered partial file - OK if you are in the middle of copying a file|
|harvester |src.plotting.plot_tools | INFO |    Searching directories [`Dir1`,`Dir2`] ||
|harvester |src.plotting.plot_tools | WARNING | Directory: `Dir1` does not exist. | One of your monitored plot folders is no longer accessible - eg external drive offline - if permanent remove from GUI or `chia plots remove -d <Dir1>`|
|harvester |src.plotting.plot_tools | INFO |    Not checking subdirectory `Dir1/directory`, subdirectories not added by default ||
|harvester |src.plotting.plot_tools | WARNING | Have multiple copies of the plot `plotfilename`, not adding it.||
|harvester |src.plotting.plot_tools | INFO |    Loaded a total of `N` plots of size `size` TiB, in `time` seconds||
|harvester |src.harvester.harvester | INFO |`X` plots were eligible for farming `hex`... Found `Y` proofs. Time: `Time` s. Total `Z` plots|This is a vital message and should be seen at regular intervals. Note that `Time` is ideally < 1s. If drive is in sleep mode, may show ~10 seconds, and should be prevented |
| wallet   |src.wallet.wallet_blockchain | INFO | 💰 Updated wallet peak to height `HEIGHT`, weight `WEIGHT`, ||












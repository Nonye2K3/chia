## Introduction

The Chia protocol is an asynchronous peer to peer protocol running on top of Websockets on port 8444, where all nodes act as both clients and servers, and can maintain long term connections with other peers.

Every message in the Chia protocol is  composed of bytes, using the Serialized format, and sent as a Websocket message. Each message is composed of three parts: a 1 byte type which represent the type of message being transmitted, and how to decode the data. The data, which is a Streamable encoded representation of one of the protocol messages, and the id, which is a 2 byte identifier which is used per connection to keep track of requests and responses.


```python
class Message(Streamable):
    type: uint8  # one of ProtocolMessageTypes
    # message id
    id: Optional[uint16]
    # Message data for that type
    data: bytes

```

Chia protocol messages have a max length of `(4 + 2^32 - 1) = 4294967299` bytes, or around 4GB.


## Streamable Format
The streamable format is designed to be deterministic and easy to implement, to prevent consensus issues.

The primitives are:
* Sized ints serialized in big endian format, i.e uint64
* Sized bytes serialized in big endian format, i.e bytes32
* BLS public keys serialized in bls format (48 bytes)
* BLS signatures serialized in bls format (96 bytes)
* bool serialized into 1 byte (0x01 or 0x00)

An item is one of:
* streamable
* primitive
* List[item]
* Optional[item]
* Tuple[item1 .. itemx]
* bytes
* Program


A streamable is an ordered group of items.

1. A streamable with fields 1..n is serialized by appending the serialization of each field.
2. A List is serialized into a 4 byte size prefix (number of items) and the serialization of each item
3. An Optional is serialized into a 1 byte prefix of 0x00 or 0x01, and if it's one, it's followed by the serialization of the item
4. A tuple of x items is serialized by appending the serialization of each item.
5. A bytes  object is serialized as a list, with a 4 byte size prefix and then the bytes.
6. A Program is serialized according to CLVM serialization

This format can be implemented very easily, and allows us to hash objects like headers and proofs of space,
without complex serialization logic.

All objects in the Chia protocol are stored and trasmitted over the wire using the streamable format.

## Handshake

All peers in the Chia protocol (whether they are farmers, full nodes, timelords, etc) act as both servers and clients (peers).
As soon as a connection is initiated between two peers, both send a Handshake message, and a HandshakeAck message to complete the handshake.
A peer's node_id is the sha256 hash of their x509 DER certificate. 

```Python
class Handshake:
    network_id: bytes32      # Network id, usually the genesis challenge of the blockchain
    protocol_version: str    # Protocol version to determine which messages the peer supports
    software_version: str    # Version of the software, to debug and determine feature support
    server_port: uint16      # Which port the server is listening on
    node_type: uint8         # NodeType (full node, wallet, farmer, etc)
```

After the handshake is completed, both peers can send Chia protocol messages, and disconnect at any time by closing the Websocket.

## Heartbeat
Heartbeat messages are sent periodically by the Websocket libraries. Peers that are unresponsive will therefore be disconnected.

If a node does receive any messages from a peer for a certain period of time, even if heartbeats are being received, then the node will disconnect and remove the peer from the active peer list.

## Introducer

For a new peer to join the decentralized network, they must choose a subset of all online nodes to connect to.

To facilitate this process, a number of introducer nodes will temporarily be run by Chia and other users, which will crawl the network and support one protocol message: get_peers_introducer.
The introducer will then return a random subset of known recent peers that the calling node will attempt to connect to.

The plan is to switch to DNS and a more decentralized approach of asking different peers for their peers.
The introducer is only contacted if the peer database has no good peers.

## RPC
Aside from the Chia protocols described in the next page, there is also a local RPC protocol to allow simple control over a node or wallet through HTTP.
All requests and responses for the RPC protocol are in JSON, to simplify the interface.
This allows doing things like getting the tips of the chain, getting a specific block, adding connections, stopping the node, etc. The full node UI connects to the full node using the RPC. 
The RPC apis are provided in both Websocket and HTTP format.


The next document in the tutorial is [Protocols](https://github.com/Chia-Network/chia-blockchain/wiki/Protocols).
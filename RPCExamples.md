# RPC examples

Here you can see more examples if common uses of the chia RPC. This focuses on uses for creating, submitting, and viewing transactions.
To try them out, download chia at chia.net, and paste the commands into the command line.
More details on other RPC apis are providec in the RPC documentation.

1. Get public keys
2. Generate private key mnemonic
3. Add private key to wallet
4. Get private key
5. Generate and sign transaction
6. Broadcast transaction
7. Get mempool item by tx id
8. Get coin record by id


## 1. Get public keys
```bash

curl --insecure --cert ~/.chia/$VERSION/config/ssl/wallet/private_wallet.crt --key ~/.chia/$VERSION/config/ssl/wallet/private_wallet.key -d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_public_keys


# Response
{
    "public_key_fingerprints": [
        3967641394
    ],
    "success": true
}

```

## 2. Generate private key mnemonic
```bash

curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:9256/generate_mnemonic

# Response

{
    "mnemonic": [
        "consider",
        "toe",
        "board",
        "output",
        "coral",
        "demise",
        "latin",
        "avocado",
        "sight",
        "fluid",
        "bracket",
        "fortune",
        "assault",
        "cinnamon",
        "chronic",
        "demand",
        "fiscal",
        "sea",
        "target",
        "lake",
        "table",
        "found",
        "raise",
        "exhibit"
    ],
    "success": true
}
```

## 3. Add private key to wallet
```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"mnemonic": [
        "consider",
        "toe",
        "board",
        "output",
        "coral",
        "demise",
        "latin",
        "avocado",
        "sight",
        "fluid",
        "bracket",
        "fortune",
        "assault",
        "cinnamon",
        "chronic",
        "demand",
        "fiscal",
        "sea",
        "target",
        "lake",
        "table",
        "found",
        "raise",
        "exhibit"
    ], "type": "new_wallet"}' -H "Content-Type: application/json" -X POST https://localhost:9256/add_key
    

# Response 
{
    "fingerprint": 3886196528,
    "success": true
}

```

## 4. Get private key
```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"fingerprint":3967641394}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_private_key

# Response
{
    "private_key": {
        "fingerprint": 3967641394,
        "pk": "a8b1e7599038d548f0d1360b98f5c820bae3e107b7c819a3b847200b6dd3448ac9bf8c7dd9c9e689cca386332afa9e8e",
        "seed": "one juice raccoon doll network crime orient artwork zebra candy cry trouble almost brand harbor replace dune visit figure monitor wall monkey erode connect",
        "sk": "712dbb0bb483f6ad2d30f1d5dd0809ffdb2ede6b104b99f5c11cb6c6b710cb5a"
    },
    "success": true
}

```


## 5. Generate and sign transaction
```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"additions": [{"amount": 10000000, "puzzle_hash": "3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0"}]}' -H "Content-Type: application/json" -X POST https://localhost:9256/create_signed_transaction

# Response
{
    "signed_tx": {
        "additions": [
            {
                "amount": 10000000,
                "parent_coin_info": "0x7b9b5e52a52ee671385c1f51b4242747c9569a3201542bdbdefe1a02ddb3523a",
                "puzzle_hash": "0x3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0"
            },
            {
                "amount": 1749990000000,
                "parent_coin_info": "0x7b9b5e52a52ee671385c1f51b4242747c9569a3201542bdbdefe1a02ddb3523a",
                "puzzle_hash": "0x55b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08"
            }
        ],
        "amount": 10000000,
        "confirmed": false,
        "confirmed_at_height": 0,
        "created_at_time": 1618301854,
        "fee_amount": 0,
        "name": "0xa4fa635bd8c0500379760ac19e30eda0ad9811313e44ecef430dee349e793840",
        "removals": [
            {
                "amount": 1750000000000,
                "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
            }
        ],
        "sent": 0,
        "sent_to": [],
        "spend_bundle": {
            "aggregated_signature": "0xa5e5ea1f5ae2335a72fe0a7ed7ca39e8f142e2e1f6e37a348482290e88eb9cea2d973acf6145e34d0afeee7ba22f99850641e21a549b2c092bb49aa393acd938825bccca9413c1a268ba44367bc8433cd0fc0eb82e87bebe23817aa695bdb566",
            "coin_solutions": [
                {
                    "coin": {
                        "amount": 1750000000000,
                        "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                        "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
                    },
                    "puzzle_reveal": "0xff02ffff01ff02ffff01ff02ffff03ff0bffff01ff02ffff03ffff09ff05ffff1dff0bffff1effff0bff0bffff02ff06ffff04ff02ffff04ff17ff8080808080808080ffff01ff02ff17ff2f80ffff01ff088080ff0180ffff01ff04ffff04ff04ffff04ff05ffff04ffff02ff06ffff04ff02ffff04ff17ff80808080ff80808080ffff02ff17ff2f808080ff0180ffff04ffff01ff32ff02ffff03ffff07ff0580ffff01ff0bffff0102ffff02ff06ffff04ff02ffff04ff09ff80808080ffff02ff06ffff04ff02ffff04ff0dff8080808080ffff01ff0bffff0101ff058080ff0180ff018080ffff04ffff01b0aec9c2e5984fe928406abca942d55ec6b56340af8315bfefa55889dbaade669b9fd3f330af2af44c2a0626d383e64757ff018080",
                    "solution": "0xff80ffff01ffff33ffa03fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0ff840098968080ffff33ffa055b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08ff860197738845808080ff8080"
                }
            ]
        },
        "to_puzzle_hash": "0x3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0",
        "trade_id": null,
        "type": 1,
        "wallet_id": 1
    },
    "success": true
}

```


## 6. Broadcast transaction

```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt --key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key -d '{        "spend_bundle": {
            "aggregated_signature": "0xa5e5ea1f5ae2335a72fe0a7ed7ca39e8f142e2e1f6e37a348482290e88eb9cea2d973acf6145e34d0afeee7ba22f99850641e21a549b2c092bb49aa393acd938825bccca9413c1a268ba44367bc8433cd0fc0eb82e87bebe23817aa695bdb566",
            "coin_solutions": [
                {
                    "coin": {
                        "amount": 1750000000000,
                        "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                        "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
                    },
                    "puzzle_reveal": "0xff02ffff01ff02ffff01ff02ffff03ff0bffff01ff02ffff03ffff09ff05ffff1dff0bffff1effff0bff0bffff02ff06ffff04ff02ffff04ff17ff8080808080808080ffff01ff02ff17ff2f80ffff01ff088080ff0180ffff01ff04ffff04ff04ffff04ff05ffff04ffff02ff06ffff04ff02ffff04ff17ff80808080ff80808080ffff02ff17ff2f808080ff0180ffff04ffff01ff32ff02ffff03ffff07ff0580ffff01ff0bffff0102ffff02ff06ffff04ff02ffff04ff09ff80808080ffff02ff06ffff04ff02ffff04ff0dff8080808080ffff01ff0bffff0101ff058080ff0180ff018080ffff04ffff01b0aec9c2e5984fe928406abca942d55ec6b56340af8315bfefa55889dbaade669b9fd3f330af2af44c2a0626d383e64757ff018080",
                    "solution": "0xff80ffff01ffff33ffa03fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0ff840098968080ffff33ffa055b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08ff860197738845808080ff8080"
                }
            ]
        }}' -H "Content-Type: application/json" -X POST https://localhost:8555/push_tx


# Response
{"status": "SUCCESS", "success": true}
```

## 7. Get mempool item by tx id
Note: this is referred to as "name" in the response for `create_signed_transaction`.

```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"tx_id":"0xa4fa635bd8c0500379760ac19e30eda0ad9811313e44ecef430dee349e793840"}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_mempool_item_by_tx_id


# Response

{
    "mempool_item": {
        "additions": [
            {
                "amount": 10000000,
                "parent_coin_info": "0x7b9b5e52a52ee671385c1f51b4242747c9569a3201542bdbdefe1a02ddb3523a",
                "puzzle_hash": "0x3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0"
            },
            {
                "amount": 1749990000000,
                "parent_coin_info": "0x7b9b5e52a52ee671385c1f51b4242747c9569a3201542bdbdefe1a02ddb3523a",
                "puzzle_hash": "0x55b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08"
            }
        ],
        "cost_result": {
            "cost": 112633,
            "error": null,
            "npc_list": [
                {
                    "coin_name": "0x7b9b5e52a52ee671385c1f51b4242747c9569a3201542bdbdefe1a02ddb3523a",
                    "conditions": [
                        [
                            "0x32",
                            [
                                {
                                    "opcode": "AGG_SIG_ME",
                                    "vars": [
                                        "0xaec9c2e5984fe928406abca942d55ec6b56340af8315bfefa55889dbaade669b9fd3f330af2af44c2a0626d383e64757",
                                        "0xbdff8f03de60962e4fdacc805d8783bfd0693ba383653bb932184f2120a0e6c2"
                                    ]
                                }
                            ]
                        ],
                        [
                            "0x33",
                            [
                                {
                                    "opcode": "CREATE_COIN",
                                    "vars": [
                                        "0x3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0",
                                        "0x00989680"
                                    ]
                                },
                                {
                                    "opcode": "CREATE_COIN",
                                    "vars": [
                                        "0x55b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08",
                                        "0x019773884580"
                                    ]
                                }
                            ]
                        ]
                    ],
                    "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
                }
            ]
        },
        "fee": 0,
        "removals": [
            {
                "amount": 1750000000000,
                "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
            }
        ],
        "spend_bundle": {
            "aggregated_signature": "0xa5e5ea1f5ae2335a72fe0a7ed7ca39e8f142e2e1f6e37a348482290e88eb9cea2d973acf6145e34d0afeee7ba22f99850641e21a549b2c092bb49aa393acd938825bccca9413c1a268ba44367bc8433cd0fc0eb82e87bebe23817aa695bdb566",
            "coin_solutions": [
                {
                    "coin": {
                        "amount": 1750000000000,
                        "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                        "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
                    },
                    "puzzle_reveal": "0xff02ffff01ff02ffff01ff02ffff03ff0bffff01ff02ffff03ffff09ff05ffff1dff0bffff1effff0bff0bffff02ff06ffff04ff02ffff04ff17ff8080808080808080ffff01ff02ff17ff2f80ffff01ff088080ff0180ffff01ff04ffff04ff04ffff04ff05ffff04ffff02ff06ffff04ff02ffff04ff17ff80808080ff80808080ffff02ff17ff2f808080ff0180ffff04ffff01ff32ff02ffff03ffff07ff0580ffff01ff0bffff0102ffff02ff06ffff04ff02ffff04ff09ff80808080ffff02ff06ffff04ff02ffff04ff0dff8080808080ffff01ff0bffff0101ff058080ff0180ff018080ffff04ffff01b0aec9c2e5984fe928406abca942d55ec6b56340af8315bfefa55889dbaade669b9fd3f330af2af44c2a0626d383e64757ff018080",
                    "solution": "0xff80ffff01ffff33ffa03fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0ff840098968080ffff33ffa055b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08ff860197738845808080ff8080"
                }
            ]
        },
        "spend_bundle_name": "0xa4fa635bd8c0500379760ac19e30eda0ad9811313e44ecef430dee349e793840"
    },
    "success": true
}

```


## 8. Get coin record by ID
Use this with all created coins, to confirm that a transaction's outputs are all confirmed on the blockchain

```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt --key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key  -H "Content-Type: application/json" -X POST https://localhost:8555/get_coin_record_by_name -d '{"name":"1fd60c070e821d785b65e10e5135e52d12c8f4d902a506f48bc1c5268b7bb45b"}'

{
    "coin_record": {
        "coin": {
            "amount": 18375000000000000000,
            "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000000000",
            "puzzle_hash": "0xd23da14695a188ae5708dd152263c4db883eb27edeb936178d4d988b8f3ce5fc"
        },
        "coinbase": true,
        "confirmed_block_index": 1,
        "spent": false,
        "spent_block_index": 0,
        "timestamp": 1616162525
    },
    "success": true
}


```
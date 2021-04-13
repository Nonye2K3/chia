# RPC examples

Here you can see more examples if common uses of the chia RPC. This focuses on uses for creating, submitting, and viewing transactions.
To try them out, download chia at chia.net, and paste the commands into the command line.
More details on other RPC apis are provided in the [RPC Interfaces documentation](https://github.com/Chia-Network/chia-blockchain/wiki/RPC-Interfaces). 
These are just a few examples, there are more things you can do that these.

1. Get blockchain state
2. Get public keys
3. Generate private key mnemonic
4. Add private key to wallet
5. Get private key
6. Generate and sign transaction
7. Broadcast transaction
8. Get mempool item by tx id
9. Get coin record by id
10. Get block record by height
11. Get block

## 1. Get blockchain state
```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt --key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key -d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_blockchain_state

# Response

{
    "blockchain_state": {
        "difficulty": 20,
        "genesis_challenge_initialized": true,
        "mempool_size": 0,
        "peak": {
            "challenge_block_info_hash": "0x1145fc6914b8eaa5e13dea570e45926d0d2667bddec0921f0b34a3285d029f8b",
            "challenge_vdf_output": {
                "data": "0x0100d006028bb46edf6c72ff633870f7fea2e18b3aac538bbb92ceb12c3ff235407ec8ed323f0d8b505bac8e576322a6ddd92e6b9def8ca587196e76ef7aef01b3184dc87695e42731e64df85a84611f2b717ad7ca030e6fb19e2a8eb2eb4595db030200"
            },
            "deficit": 15,
            "farmer_puzzle_hash": "0x562469d301d49d306ee6aec4a680171f4590b5f4a1ae920f782abe7557b51c94",
            "fees": 0,
            "finished_challenge_slot_hashes": [
                "0xd80b5ab28a3c219019b65e80d9150525bc6baf6b1a60118264210833fbfaedfc"
            ],
            "finished_infused_challenge_slot_hashes": [
                "0x8824efc5d30caf5d465ead1100ac6830692d2f749bbe5ddfaf24566183562a63"
            ],
            "finished_reward_slot_hashes": [
                "0xa1a60027906da23c8386acda82afd9935d332a3aed199711915e11e46018f7d3"
            ],
            "header_hash": "0x36c15c1bf22e0d9b84f40f4eaf8070ec0b10a00e40d6fab2d31544bcc78bc561",
            "height": 123029,
            "infused_challenge_vdf_output": null,
            "overflow": false,
            "pool_puzzle_hash": "0x562469d301d49d306ee6aec4a680171f4590b5f4a1ae920f782abe7557b51c94",
            "prev_hash": "0xe9aa0c40ac529bbfd258ff7c457860a99dac6afcdbb4ffb3da8f2290d377271e",
            "prev_transaction_block_hash": "0xe9aa0c40ac529bbfd258ff7c457860a99dac6afcdbb4ffb3da8f2290d377271e",
            "prev_transaction_block_height": 123028,
            "required_iters": 1079116,
            "reward_claims_incorporated": [
                {
                    "amount": 1750000000000,
                    "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a0000000000000000000000000001e094",
                    "puzzle_hash": "0x3c32c6b636535e39e37e0218e7206c66806803c7ea724eda48aff17eb5ec6081"
                },
                {
                    "amount": 250000000000,
                    "parent_coin_info": "0x3ff07eb358e8255a65c30a2dce0e5fbb0000000000000000000000000001e094",
                    "puzzle_hash": "0x3c32c6b636535e39e37e0218e7206c66806803c7ea724eda48aff17eb5ec6081"
                }
            ],
            "reward_infusion_new_challenge": "0x6e5dcf6ae0ef350b38e02ab7f911f0834a76e87d5598a851afda9c967db47c65",
            "signage_point_index": 4,
            "sub_epoch_summary_included": null,
            "sub_slot_iters": 121634816,
            "timestamp": 1618304972,
            "total_iters": 410719057740,
            "weight": 1511211
        },
        "space": 339652100035216512,
        "sub_slot_iters": 121634816,
        "sync": {
            "sync_mode": false,
            "sync_progress_height": 0,
            "sync_tip_height": 0,
            "synced": true
        }
    },
    "success": true
}

```

## 1. Get public keys
```bash

curl --insecure --cert ~/.chia/mainnet/config/ssl/wallet/private_wallet.crt --key ~/.chia/mainnet/config/ssl/wallet/private_wallet.key -d '{"":""}' -H "Content-Type: application/json" -X POST https://localhost:9256/get_public_keys


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
Please see [this file](https://github.com/Chia-Network/chia-blockchain/blob/main/tests/wallet/rpc/test_wallet_rpc.py) if you want to see how to spend specific UTXOs (coins), provide a fee, pay to multiple addresses, etc.

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

# 10. Get block record by height
You can also query for the full block, or query for the block record by header hash instead (using the other RPCs).

```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt --key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key -d '{"height": 0}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_block_record_by_height

# Response

{
    "block_record": {
        "challenge_block_info_hash": "0x45e3beae0441bab860109a977c94d9d8accb1927d7ad6e70faf84d18b9b96aaa",
        "challenge_vdf_output": {
            "data": "0x0000dafd52467f3f66cf25c7de3e432188b9c3000b95a09170ace441a214cffc5fa65a39a66f11085fea9c7a121cf7f86a74142506b6d51d6036097bff862707185dbba577707c248f2fca5877337e56905879a2ee840d28860c50ebcacaaaf53a660100"
        },
        "deficit": 15,
        "farmer_puzzle_hash": "0x3d8765d3a597ec1d99663f6c9816d915b9f68613ac94009884c4addaefcce6af",
        "fees": 0,
        "finished_challenge_slot_hashes": [
            "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb"
        ],
        "finished_infused_challenge_slot_hashes": null,
        "finished_reward_slot_hashes": [
            "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb"
        ],
        "header_hash": "0xd780d22c7a87c9e01d98b49a0910f6701c3b95015741316b3fda042e5d7b81d2",
        "height": 0,
        "infused_challenge_vdf_output": null,
        "overflow": false,
        "pool_puzzle_hash": "0xd23da14695a188ae5708dd152263c4db883eb27edeb936178d4d988b8f3ce5fc",
        "prev_hash": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
        "prev_transaction_block_hash": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
        "prev_transaction_block_height": 0,
        "required_iters": 1313186,
        "reward_claims_incorporated": [],
        "reward_infusion_new_challenge": "0xbdde7b5b2bc6025c07a9f5233d8eae167bea654146b272652262b362524c3e85",
        "signage_point_index": 2,
        "sub_epoch_summary_included": null,
        "sub_slot_iters": 134217728,
        "timestamp": 1616162474,
        "total_iters": 11798946,
        "weight": 7
    },
    "success": true
}

```

## 11. Get block

```bash
curl --insecure --cert ~/.chia/mainnet/config/ssl/full_node/private_full_node.crt --key ~/.chia/mainnet/config/ssl/full_node/private_full_node.key -d '{"header_hash": "0xd780d22c7a87c9e01d98b49a0910f6701c3b95015741316b3fda042e5d7b81d2"}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_block

# Response
{
    "block": {
        "challenge_chain_ip_proof": {
            "normalized_to_identity": true,
            "witness": "0x0100fd641357188b5e18f962148c1ceb8e0816c25c657cf0fd971c1a6ed528cea74529c585eef17df64fa2037b2ed1d3ba4035f7a6caad4e67ca7a41bed242e0e51ac603130ce5c7c6f5f3d22cad0d95403ab6619ef81076115248582f325bc90e0a0100",
            "witness_type": 0
        },
        "challenge_chain_sp_proof": {
            "normalized_to_identity": true,
            "witness": "0x030040c178e0d3470733621c74dde8614c0421d03ad2ce3bb7cad3616646e3762b35568fbae23139119f7affdc7201f45ee284cc76be6e341c795ccb5779cf102305a31bae2f870ea52c87fb0803a4493a2eb1a2cbbce7e467938cb73447edde2d1b0100",
            "witness_type": 0
        },
        "finished_sub_slots": [],
        "foliage": {
            "foliage_block_data": {
                "extension_data": "0x0000000000000000000000000000000000000000000000000000000003a2c7c9",
                "farmer_reward_puzzle_hash": "0x3d8765d3a597ec1d99663f6c9816d915b9f68613ac94009884c4addaefcce6af",
                "pool_signature": "0x95bc50737062a206ed36172634a2cebcb8cf045d044969baabb8504c99668d160635452c31389d16714d5bd61fc46b081975aca7f36a9aa8323ef38d5c751b13cc01cd7dc04d979ea0dd82ab8ef394d49998cf2865d4411d5a05bcf4bb8f2627",
                "pool_target": {
                    "max_height": 0,
                    "puzzle_hash": "0xd23da14695a188ae5708dd152263c4db883eb27edeb936178d4d988b8f3ce5fc"
                },
                "unfinished_reward_block_hash": "0x72befeb88e0b6612e0c5ddc94bf6b64bb2097579d418aede6d6006951720ddb3"
            },
            "foliage_block_data_signature": "0xaa11b01a2bc6bd1d6cdf030b9699aa1ee30a5d318f1fafc66489b1d41d096500f4aaddea9eed4f44e853ad9905d421660a216242c0d39b8fca8ac69964d78d701731940d30d6645320cfa570a1679f5308941093118ba94c279e7158c37f6d37",
            "foliage_transaction_block_hash": "0x8a32059ed30637d342ad40a7324fdbd14d4060eb91ba381480e52bb249e34648",
            "foliage_transaction_block_signature": "0xafe1dbb908edd6b466115fbd9755f86462a36d94da5b94aa00c9f7708b6f8c5df7b877e813f87c9bbbdef691845414da10449d51a6ac8ef86c5befb4511a2b7da4c82a38a9e14b3d94b96c7219ee5abd930ecee202ba697d6d27a6a7f914d999",
            "prev_block_hash": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
            "reward_block_hash": "0xbdde7b5b2bc6025c07a9f5233d8eae167bea654146b272652262b362524c3e85"
        },
        "foliage_transaction_block": {
            "additions_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "filter_hash": "0x6e340b9cffb37a989ca544e6bb780a2c78901d3fb33738768511a30617afa01d",
            "prev_transaction_block_hash": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
            "removals_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "timestamp": 1616162474,
            "transactions_info_hash": "0x4d65bf0d9286451647b8bff948b0e8cf7ad57125958eb84d4d32a7cfde441e03"
        },
        "infused_challenge_chain_ip_proof": null,
        "reward_chain_block": {
            "challenge_chain_ip_vdf": {
                "challenge": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
                "number_of_iterations": 11798946,
                "output": {
                    "data": "0x0000dafd52467f3f66cf25c7de3e432188b9c3000b95a09170ace441a214cffc5fa65a39a66f11085fea9c7a121cf7f86a74142506b6d51d6036097bff862707185dbba577707c248f2fca5877337e56905879a2ee840d28860c50ebcacaaaf53a660100"
                }
            },
            "challenge_chain_sp_signature": "0x94ef18a3cb94e91b987eaab5e7d2ed33ca69598b2278c4bea87ac6ef0217842b9db79bb6906719f671971ab328d2a47709cea008e64e5099d563757b78c3ed8bb0f7757294eca643b08e7ab4410e5628037ddc4cf47f613fedfbfb97672d2b08",
            "challenge_chain_sp_vdf": {
                "challenge": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
                "number_of_iterations": 4194304,
                "output": {
                    "data": "0x0300445cbcaa176166d9a50b9699e9394be46766c8f6494a1a9afd95bb5dc652ee42101278ad7358baf4ae727e4f5a6f732e3a8c26d9d11365081275a6d4b36dda63a905baffdaebab3311d8d6e2f356edf3bb1cf90e5654e688869d66d1c60676440100"
                }
            },
            "height": 0,
            "infused_challenge_chain_ip_vdf": null,
            "is_transaction_block": true,
            "pos_ss_cc_challenge_hash": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
            "proof_of_space": {
                "challenge": "0xbd9c2a80bbb08c340c365a96af54d60319cc7695436e11b8b2a2f8cf147bc872",
                "plot_public_key": "0xa5288853edf400f6152f1e3c5fb9ee5055d4d9d02d79a35a00183b0064a8d9bb3571f04ee3cbc4432a7ce7413d5dacaa",
                "pool_contract_puzzle_hash": null,
                "pool_public_key": "0xa15afd4ef858cdc97769e61d79a0b03d5bf108d46f2bc47d7cd549ce43f350a121821a9834c0a68c267306718c2c2bf8",
                "proof": "0x8d7c7c7763847953a9b3e56d8145b8a712b2bc8116c0dfc2db7cbbf270ef253b8539877e992b32087f5a45c218acaf21a02d8ceded9d19fd67b74c6aa91a164e69d13ad99e0559ead07b7237809de00e8a412683e5acfa407675c09bac790d94ae40b5998c21f1522635dfaf235ad1b5853120f3876f6e0fab04d3945887063c78ab3e1babbd30da02a120f59511143411be53db3c8b40815ee8f82aee2863643540d524c8f7042cb4b63861da7f0e35e7ed41f97bd24b9d9294090bee1eb9a2410946048519772665a7f414f20776a5ca215d1af269e591e033e96c8611dda93a6e00d58929bd3a734f2886c273c55b70c2348689b19d5234d778a48620c79d",
                "size": 32
            },
            "reward_chain_ip_vdf": {
                "challenge": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
                "number_of_iterations": 11798946,
                "output": {
                    "data": "0x0000dafd52467f3f66cf25c7de3e432188b9c3000b95a09170ace441a214cffc5fa65a39a66f11085fea9c7a121cf7f86a74142506b6d51d6036097bff862707185dbba577707c248f2fca5877337e56905879a2ee840d28860c50ebcacaaaf53a660100"
                }
            },
            "reward_chain_sp_signature": "0x94ef18a3cb94e91b987eaab5e7d2ed33ca69598b2278c4bea87ac6ef0217842b9db79bb6906719f671971ab328d2a47709cea008e64e5099d563757b78c3ed8bb0f7757294eca643b08e7ab4410e5628037ddc4cf47f613fedfbfb97672d2b08",
            "reward_chain_sp_vdf": {
                "challenge": "0xccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb",
                "number_of_iterations": 4194304,
                "output": {
                    "data": "0x0300445cbcaa176166d9a50b9699e9394be46766c8f6494a1a9afd95bb5dc652ee42101278ad7358baf4ae727e4f5a6f732e3a8c26d9d11365081275a6d4b36dda63a905baffdaebab3311d8d6e2f356edf3bb1cf90e5654e688869d66d1c60676440100"
                }
            },
            "signage_point_index": 2,
            "total_iters": 11798946,
            "weight": 7
        },
        "reward_chain_ip_proof": {
            "normalized_to_identity": false,
            "witness": "0x0100069fef389d0cab01b732a1f9df5a613791a675c6d374a273815ac52456791f5b54990e239101092f8c9b1f10f00831b3081a8bacb4ed30a4f1a3b880985627231bde6dfab8a923543cf4a479c167a4efaa99bc757bed4ae8ce323ee425fd0a2301000000000000280230d9e99169933c68691769e36eafc2c5cd77643c5764ae881670d73ce5b6f354a5530100da6ad4bdeebea9ee562f5ecd75941b87b562ab3dfdb11f7ecc32a95da2b43860014153be21b70af27f78a24186be61eb7d3e1b8a45e4f7c92faa03d5427d432769726ffbad69c860cef51f0da30c18aa2b717612ea592de302be3278d950291b0100000000000078062c873f93d42f5aea24607ed244308cdbd7e7312bf6d0ed7d540d38ec6e643794862f0100e9794b47a84b25a60816aed4aef701c72188c5f7d5a44ed96a0e06a36b9a7915f3c38697fbb289a6fdafb2fa4c8169c4a7997f6a1b84a8f33219734cbeb50d2ef55570c7d06e3583acdbe993d8e521ce3211704eb316761ea8f94bb50cb9110a0100",
            "witness_type": 2
        },
        "reward_chain_sp_proof": {
            "normalized_to_identity": false,
            "witness": "0x0200b32e249915029b85ba7307fd4102d3352c52a30032d44639b2eb5d3a77497e269b9d9a9be91cd89d32a011a519c889a2f23ae1b08d56c99581afb57c29eed22a33ac88fd3f2075d7312ab4fe9b2ba3f7b714bc3b8d20ec0254d560819356eb40010000000000000e38a0a555927b04d5972453306cd46576785f0077db62f16adab830cd191ae3d1c63cf100000317fd6f5378f72c4ec504b2e460a9669de31dd1026e7dca756c67130a02a0ddf594a206ec70c9c6b06829608b3ccb1b3439a34ac99ac94eba7ef0730198387877ea6845834b56c13371c079b2201fd1e5e49cfe1fc716daeab329b55cbbb82c010000000000002aaaa8f5c834580070be7b0d3c5a49290b71ca8f8759d4beb423841c97f92b6b6409b02d030059b7199e86a68a9114d14ef44778600845068fe30d768acce03bfae6d0271a653f1317fe678e4e8546f76b2d9ce7cc52afdbd9fa6882ad916856f445e355937945ea840bd29058b8652404f8e5d3e13c78cfd2c5cd3fe9e6f404395d8e9b33800100",
            "witness_type": 2
        },
        "transactions_generator": null,
        "transactions_generator_ref_list": [],
        "transactions_info": {
            "aggregated_signature": "0xc00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "cost": 0,
            "fees": 0,
            "generator_refs_root": "0x0101010101010101010101010101010101010101010101010101010101010101",
            "generator_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "reward_claims_incorporated": []
        }
    },
    "success": true
}


```
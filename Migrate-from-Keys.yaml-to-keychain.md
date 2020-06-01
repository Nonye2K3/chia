## Example: 

### Example keys.yaml contents:
```
pool_sks:
- 2af395e80e9e65ae23851db8a8ac43a5725752afcab932db07850d826ca4d325
- 6e8caaaa7390b76c955fa580dc2bddbf15ae80dff884be169a45d7f4aa94ac79
pool_target: 6a0a95abb20759ed6b86d4654645884a6f1bbaa7e39f3815cb431e37e8b2494f
sk_seed: c77b1e84038f2ac970fac7fa37b5e6695831ffc9f4a3cf160a537fcd2408630c
wallet_sk: 0000000100000000000000000067bdce9f28cc0aee21e5feb869824fb068bd719d47485476b4581b39b822f5dd6aa7cccc7b94e06699c9d3dd41ad80838435152a33cbed57c3e2ae8c60c5a46b
wallet_target: 6a0a95abb20759ed6b86d4654645884a6f1bbaa7e39f3815cb431e37e8b2494f
```

### First add wallet_sk key:        
Doing this will also automatically generate pool/wallet target for this wallet_sk. 

```
chia keys add -k  0000000100000000000000000067bdce9f28cc0aee21e5feb869824fb068bd719d47485476b4581b39b822f5dd6aa7cccc7b94e06699c9d3dd41ad80838435152a33cbed57c3e2ae8c60c5a46b
```


### Then add two pool_sks keys:
```
chia keys add_not_extended -k 2af395e80e9e65ae23851db8a8ac43a5725752afcab932db07850d826ca4d325
chia keys add_not_extended -k 6e8caaaa7390b76c955fa580dc2bddbf15ae80dff884be169a45d7f4aa94ac79
```

### To make sure config.yaml got updated with latest target:
```
Chia init
```

That's it, sk_seed doesn't need to be migrated, as new plots are using a random key by default.
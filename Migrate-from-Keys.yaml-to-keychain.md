# Windows and MacOS

On beta1.6, keys are stored in the OS keychain instead of in keys.yaml.
When you start the 1.6 Chia program for the first time, keys will automatically get migrated from previous versions. (~/.chia/beta-1.0bx folders). Make sure to stop all 1.5 servers before launching 1.6 for the first time, so there is no conflict in servers.

After migrating your old keys, if you want to migrate additional keys from other keys.yaml files, do the following:

1. Close the Chia program and make sure no servers are running in Task Manager/activity monitor
2. Delete the ~/.chia/beta-1.0b6/ directory (This will not delete keys from keychain)
3. Backup the ~/.chia/beta-1.0b5/config/keys.yaml 
4. Put the keys.yaml file that you want to migrate into ~/.chia/beta-1.0b5/config/keys.yaml
5. Place a valid config.yaml into ~/.chia/beta-1.0b5/config/ also.
6. Launch the Chia program again

Note that only wallet_sk and pool_sks are migrated. sk_seed doesn't need to be migrated, as new plots are using a random key by default. The pool and wallet targets are generated when Chia starts. This process will add additional keys, it will not remove any keys from 1.6 keychain.

On Mac, each process also requires entering a password for each migrated key, so please be patient and click "Always Allow" after entering the password each time.

# Source install

Using this example keys.yaml:
```
pool_sks:
- 2af395e80e9e65ae23851db8a8ac43a5725752afcab932db07850d826ca4d325
- 6e8caaaa7390b76c955fa580dc2bddbf15ae80dff884be169a45d7f4aa94ac79
pool_target: 6a0a95abb20759ed6b86d4654645884a6f1bbaa7e39f3815cb431e37e8b2494f
sk_seed: c77b1e84038f2ac970fac7fa37b5e6695831ffc9f4a3cf160a537fcd2408630c
wallet_sk: 0000000100000000000000000067bdce9f28cc0aee21e5feb869824fb068bd719d47485476b4581b39b822f5dd6aa7cccc7b94e06699c9d3dd41ad80838435152a33cbed57c3e2ae8c60c5a46b
wallet_target: 6a0a95abb20759ed6b86d4654645884a6f1bbaa7e39f3815cb431e37e8b2494f
```
First add wallet_sk key. Doing this will also automatically generate pool/wallet target for this wallet_sk.
```
chia keys add -k  0000000100000000000000000067bdce9f28cc0aee21e5feb869824fb068bd719d47485476b4581b39b822f5dd6aa7cccc7b94e06699c9d3dd41ad80838435152a33cbed57c3e2ae8c60c5a46b
```
Then add the two pool_sks keys:
```
chia keys add_not_extended -k 2af395e80e9e65ae23851db8a8ac43a5725752afcab932db07850d826ca4d325
chia keys add_not_extended -k 6e8caaaa7390b76c955fa580dc2bddbf15ae80dff884be169a45d7f4aa94ac79
```
Make sure config.yaml got updated with latest wallet target:
```
chia init
```
That's it. sk_seed doesn't need to be migrated, as new plots are using a random key by default.
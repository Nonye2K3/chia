On beta1.6, keys are stored in the OS keychain instead of in keys.yaml.
When you start the 1.6 Chia program for the first time, keys will automatically get migrated from previous versions. (~/.chia/beta-1.0bx folders).

After migrating your old keys, if you want to migrate additional keys from other keys.yaml files, do the following:

1. Close the Chia program and make sure no servers are running in Task Manager/activity monitor
2. Delete the ~/.chia/beta-1.0b6/ directory (This will not delete keys from keychain)
3. Backup the ~/.chia/beta-1.0b5/config/keys.yaml 
4. Put the keys you want to migrate into ~/.chia/beta-1.0b5/config/keys.yaml
5. Launch the Chia program again

Note that only wallet_sk and pool_sks are migrated. sk_seed doesn't need to be migrated, as new plots are using a random key by default. The pool and wallet targets are generated when Chia starts.

On Mac, each process also requires entering a password for each migrated key, so please be patient and click "Always Allow" after entering the password each time. 
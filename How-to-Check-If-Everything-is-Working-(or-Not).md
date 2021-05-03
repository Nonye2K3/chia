
:hammer_and_wrench: **A work-in-progress** :hammer_and_wrench:

This doc assumes you know how to use the CLI. Using the CLI is the best way to troubleshoot (and to do everything Chia too). The [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) and [CLI Commands Reference](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference) have useful info to get you familiar with the CLI.

## Where to Find Things: The File Paths
The file paths for Linux, macOS, and Windows versions of Chia are similar. 
```
/home/user
├─ .chia/
│   └── mainnet/
│      ├─ config/
│      │      ├─ config.yaml
│      │      └─ ssl/
│      │            └─ (and more...)
│      ├─ db/
│      ├─ log/
│      │      └─ debug.log
│      ├─ run/
│      │      └─ (and more...)
│      └─ wallet/
│             └─ (and more...)
└── /chia-blockchain
       └─ (and more...)
```

### Linux & macOS
* Chia config: `~/.chia/mainnet/config/config.yaml`
* Chia log: `~/.chia/mainnet/log/debug.log`

### Windows
* Chia config:  `C:\Users\%USERNAME%\.chia\mainnet\config\config.yaml`
* Chia log:  `C:\Users\%USERNAME%\.chia\mainnet\log\debug.log`

# Logs
In `config.yaml` you can set the level of detail for your logs. 

Look for this section in `config.yaml`. It’s useful to change the logger setting `log_level` from `WARNING` to `INFO` to get the detail needed to troubleshoot.

```yaml
logging: &id001
    log_filename: log/debug.log
    log_level: INFO
    log_stdout: false
```

You can run `grep`  (Linux, macOS) or `Select-String` (Windows) to search through your logs for relevant information. 

# Is It Working?

If you want to quickly find errors, run this:
* Linux/macOS: `cat ~/.chia/mainnet/log/debug.log | grep error`
* Windows: `Get-Content -Path "~\.chia\mainnet\log\debug.log" | Select-String -Pattern "error"`

## Harvester
The time it takes to do a proof challenge should be below 30 seconds. If you see higher times, something is wrong with your setup.

Here are some commands you can use to examine `debug.log` for problems.

* Linux/macOS: `tail ~/.chia/mainnet/log/debug.log | grep eligible`
* Windows:
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “eligible”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Found [^0] proof”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Farmed unfinished_block”`
	* `Get-Content -Path "~\.chia\mainnet\log\debug.log" -Wait | Select-String -Pattern "found"`
    



## Plotting

You can find the documentation for the `check` command on the [CLI Commands Reference - check](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference#check) page

* To check all your plots, run `chia plots check`. This will check all directories you have listed in your `config.yaml` to contain plots.
* Use `chia plots check -h` to see the options for this command
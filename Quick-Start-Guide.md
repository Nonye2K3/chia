# Install
To install chia-blockchain, follow [these install instructions](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL) according to your operating system. This software only supports 64 bit operating systems.

All configuration data is stored in a directory structure at the $CHIA_ROOT environment variable or at ~/.chia/VERSION-DIR/. You can find databases, and logs there. Optionally, you can set $CHIA_ROOT to the .chia directory in your home directory with `export CHIA_ROOT=~/.chia` and if you add it to your .bashrc or .zshrc to it will remain set across logouts and reboots. If you set $CHIA_ROOT you will have to migrate configuration items by hand or unset the variable for `chia init` to work with `unset CHIA_ROOT`.

If you are using the MacOS or Windows builds, your keys are created during the first run. We recommend saving the mnemonic. If you intend to use these plots on mainnet you should securely store the 24 words. You can start plotting a plot file using the Plot tab or the command line. This can take a long time depending on the [size of the plots](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)
(the k variable). To be competitive on the current network you will probably have to have a few k=32 or larger plots but a k=32 plot currently takes about 12 hours to plot on an [M.2 PCIe NVMe SSD](https://en.wikipedia.org/wiki/M.2) and requires 287 GiB of temporary working space to create a final plot file of 101.3 GiB. Your likelihood of winning a given plot is only driven by the final size of files.

We believe that plots created with Beta 1.8 and newer version of the chia software will work on mainnet at launch. The minimum plot size will be k=32. The original design assumed that k=30 will be the minimum plot size but testing on faster plotting algorithms has ruled it and k=31 out. We are speeding up the time it takes to plot (especially so in release Beta 14) and have significantly decreased the temporary space needed starting in Beta 12.

## Windows
There are commands available in `~\AppData\Local\Chia-Blockchain\app-0.1.15\resources\app.asar.unpacked\daemon\` Try `.\chia -h` or `.\chia plots -h` for example. Also, you can view your debug.log as it runs in PowerShell using Get-Content, `Get-Content ~\.chia\VERSION\log\debug.log -wait`. You can also checkout these [Windows Tips](https://github.com/Chia-Network/chia-blockchain/wiki/Windows-Tips).

## MacOS
There are commands available in `/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon` Try `./chia -h` or `.chia plots -h` for example. You can view your debug.log as it runs in from Terminal, `tail -f ~/.chia/VERSION/log/debug.log`.


## Development/source builds

If you've installed via the installers you can skip these steps.

Remember that once you complete your install you **must be in the [Python virtual environment](https://docs.python-guide.org/dev/virtualenvs/)** which you access from the chia-blockchain directory, or the Windows "Chia Blockchain" directory, or your home directory if you opted for a binary install. Enter the virtual environment with the command `.   ./activate`. Both dots are critical and once executed correctly your cli prompt will look something like `(venv) username@machine:~$` with ``(venv)`` prepended. 

Use `deactivate` should you want to exit the venv. If you're not a fan of dots, an equivalent alternative on most platforms is `source venv/bin/activate` and you'll see that method in places in this documentation.

### Migrate or set up configuration files
```bash
chia init
```

### Generate keys
Create some keys by running the following script if you don't already have keys:
```bash
chia keys generate
```

### Run a full node + farmer + harvester + wallet
To run a full node on port 8444, and connect to the testnet, run the following command. Logs are usually at ~/.chia/VERSION/logs/debug.log or ~\.chia\VERSION\logs\debug.log on Windows

```bash
sh install-gui.sh
cd electron-react
npm run electron &
```

Farmers are entities in the network who use their drive space to try to create
blocks (like Bitcoin's miners), and earn block rewards. 

You can use the command line tools and change the working directories and output directory for plotting, with the "-t" (temp), "-2" (second temp), and "-d" (destination) arguments to the `chia plots create` command. `-n 2` will create two plots of type k=32 and take about 12 hours on NVMe drives in the example below.
```bash
chia plots create -k 32 -n 2
chia plots check -n 100
```
Note that in the dev build the commands are `chia plots create` and `chia plots check`.

# Run a timelord

*Note*
If you want to run a timelord on Linux, see [LINUX_TIMELORD.md](https://github.com/Chia-Network/chia-blockchain/blob/master/LINUX_TIMELORD.md). Information on blue boxes coming soon.

Timelords execute sequential verifiable delay functions (proofs of time or VDFs), that get added to
blocks to make them valid. This requires fast CPUs and a few cores per VDF as well as completing the install steps above and running the following from the chia-blockchain directory:
```bash
. ./activate
sh install-timelord.sh
chia start timelord &
```
# Alternatively run the local simulation
You can instead run the simulation, which runs all servers and multiple full nodes, locally. Note the the simulation is local only and requires installation of timelords and VDFs. The introducer will only know the local ips of the full nodes, so it cannot broadcast the correct ips to external peers. This should work on MacOS and Linux.

```bash
chia start sim
```

## Tips
Ubuntu 20.04 LTS or newer, Amazon Linux 2, and CentOS 7.7 or newer are the
easiest linux install environments.

UPnP is enabled by default to open port 8444 for incoming connections.
If this causes issues, you can disable it in config.yaml.
Some routers may require port forwarding, or enabling UPnP
in the router's configuration.

## uvloop

For potentially increased networking performance on non Windows platforms using development installs,
install uvloop:
```bash
pip install -e ".[uvloop]"
```

# RPC Interface

The Node has an RPC Interface with [documentation](https://github.com/Chia-Network/chia-blockchain/wiki/RPC-Node-Interface).


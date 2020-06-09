# Install
To install chia-blockchain, follow [these install instructions](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL) according to your operating system. This software only supports 64 bit operating systems.

All configuration and plot data is stored in a directory structure at the $CHIA_ROOT environment variable or at ~/.chia/VERSION-DIR/. You can find databases, keys, plots, and logs there. Optionally, you can set $CHIA_ROOT to the .chia directory in your home directory with `export CHIA_ROOT=~/.chia` and if you add it to your .bashrc or .zshrc to it will remain set across logouts and reboots. If you set $CHIA_ROOT you will have to migrate configuration items by hand or unset the variable for `chia init` to work with `unset CHIA_ROOT`.

If you are using the MacOS or Windows builds, your keys have been migrated for you or created during the first run. You can start plotting a plot file using the Plotter sub tab on the Farming Tab. This can take a long time depending on the [size of the plots](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)
(the k variable). To be competitive on the current network you will probably have to have a few k=30 or larger plots but a k=30 plot currently takes about 7 hours to plot on an [M.2 PCIe NVMe SSD](https://en.wikipedia.org/wiki/M.2) and requires 128 GB of temporary working space to create a final plot file of 23.8GB. You can choose to create multiple k=29 plots that will take a little less than half of that time and space. Your likelihood of winning a given plot is only driven by the final size of files.

## Windows
You can view your debug.log as it runs in PowerShell using Get-Content, `Get-Content ~\.chia\VERSION\log\debug.log -wait`. There are commands available in `~\AppData\Local\Chia-Blockchain\app-0.1.7\resources\app.asar.unpacked\daemon\` Try `.\create_plots -h` or `.\check_plots -h` for example.

## MacOS
You can view your debug.log as it runs in from Terminal, `tail -f ~/.chia/VERSION/log/debug.log`. There are commands available in `/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon` Try `./create_plots -h` or `./check_plots -h` for example.



## Development/source builds

Remember that once you complete your install you **must be in the [Python virtual environment](https://docs.python-guide.org/dev/virtualenvs/)** which you access from the chia-blockchain directory, or the Windows "Chia Blockchain" directory, or your home directory if you opted for a binary install. Enter the virtual environment with the command `.   ./activate`. Both dots are critical and once executed correctly your cli prompt will look something like `(venv) username@machine:~$` with ``(venv)`` prepended. 

Use `deactivate` should you want to exit the venv. If you're not a fan of dots, an equivalent alternative on most platforms is `source venv/bin/activate` and you'll see that method in places in this documentation.

## Migrate or set up configuration files
```bash
chia init
```

## Generate keys
First, create some keys by running the following script if you don't already have keys:
```bash
chia keys generate
```

## Run a full node + farmer + harvester + wallet
To run a full node on port 8444, and connect to the testnet, run the following command. Logs are usually at ~/.chia/VERSION/logs/debug.log or ~\.chia\VERSION\logs\debug.log on Windows

```bash
cd electron-react
npm run build
npm run electron
```

Farmers are entities in the network who use their drive space to try to create
blocks (like Bitcoin's miners), and earn block rewards. 

You can use the command line tools and change the working directories and output directory for plotting, with the "-t" (temp), "-2" (second temp), and "-d" (destination) arguments to the `create_plots` command. `-n 2` will create two plots of type k=29 and take about 6 hours on NVMe drives in the example below.
```bash
create_plots -k 29 -n 2
check_plots -n 100
```
Note that in the dev build the commands are `chia-create-plots` and `chia-check-plots`.

## Run a timelord

*Note*
If you want to run a timelord on Linux, see [LINUX_TIMELORD.md](https://github.com/Chia-Network/chia-blockchain/blob/master/LINUX_TIMELORD.md).

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
Ubuntu 18.04 LTS or newer, Amazon Linux 2, and CentOS 7.7 or newer are the
easiest linux install environments.

UPnP is enabled by default to open port 8444 for incoming connections.
If this causes issues, you can disable it in config.yaml.
Some routers may require port forwarding, or enabling UPnP
in the router's configuration.

Due to the nature of proof of space lookups by the harvester in the current
release you should limit the number of plots on a physical drive to 50 or less.
This limit will significantly increase soon.

## uvloop

For potentially increased networking performance on non Windows platforms using development installs,
install uvloop:
```bash
pip install -e ".[uvloop]"
```

# RPC Interface

The Node has an RPC Interface with [documentation](https://github.com/Chia-Network/chia-blockchain/wiki/RPC-Node-Interface).


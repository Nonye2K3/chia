# Install the code
To install chia-blockchain, follow [these install instructions](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL) according to your operating system. This project only supports 64 bit operating systems.

All configuration and plot data is stored in a directory structure at the $CHIA_ROOT environment variable or at ~/.chia/VERSION-DIR/ if that variable is not set. You can find databases, keys, plots, and logs there. Optionally, you can set $CHIA_ROOT to the .chia directory in your home directory with `export CHIA_ROOT=~/.chia` and if you add it to your .bashrc or .zshrc to it will remain set across logouts and reboots.

Remember that once you complete your install you **must be in the [Python virtual environment](https://docs.python-guide.org/dev/virtualenvs/)** which you access from the chia-blockchain directory (or your home directory if you opted for a binary install) with the command `.   ./activate` or `.\venv\Scipts\activate.ps1` on Windows PowerShell. Both dots are critical and once executed correctly your cli prompt will look something like `(venv) username@machine:~$` with ``(venv)`` prepended. 

Use `deactivate` should you want to exit the venv. Windows users will find the venv with `cd "~\AppData\Local\Programs\Chia Network\Chia Blockchain\"`. If you're not a fan of dots, an equivalent alternative on most platforms is `source venv/bin/activate` and you'll see that method in places in this documentation.

# Migrate or set up configuration files
```bash
chia init
```

# Generate keys
First, create some keys by running the following script if you don't already have keys:
```bash
chia generate keys
```

# Run a full node + wallet
To run a full node on port 8444, and connect to the testnet, run the following command. Logs are usually at ~/.chia/VERSION/logs/debug.log or ~\.chia\VERSION\logs\debug.log on Windows

```bash
chia start node &
chia start wallet &
```
If you're using Windows native or WSL 2, you should instead run:
```bash
chia start node &
chia start wallet-server &
```
And then run `Chia Wallet` from the Chia Wallet icon on the Desktop or in the Start Menu on Windows. Windows specific help is available in the [readme.txt](https://github.com/Chia-Network/chia-blockchain/blob/master/electron-wix/blockchain/readme.txt) that installs on your desktop with the Windows native install.

# Run a farmer + full node + wallet
In addition to running a full node, as explained above, you can also run a farmer.
Farmers are entities in the network who use their drive space to try to create
blocks (like Bitcoin's miners), and earn block rewards. First, you must generate some drive plots, which
can take a long time depending on the [size of the plots](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)
(the k variable). To be competitive on the current network you will probably have to have a few k=29 or larger plots but a k=29 plot currently takes about 4.5 hours to plot on an [M.2 PCIe NVMe SSD](https://en.wikipedia.org/wiki/M.2).
Once you have a few plots, run the farmer + full node with the following commands. A full node is also started when you start the farmer.

You can change the working directory and output directory for plotting, with the "-t" (temp), "-2" (second temp), and "-d" (destination) arguments to the `chia-create-plots` command.
```bash
chia-create-plots -k 29 -n 2
chia start farmer &
chia start wallet &
```
If you're using Windows/WSL 2, you should instead run:
```bash
chia-create-plots -k 29 -n 2
chia start farmer &
chia start wallet-server &
```
Then run `Chia Wallet` from the Chia Windows Wallet installer in Windows.

# Run a timelord + full node + wallet

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


# Tips
Ubuntu 18.04 LTS or newer, Amazon Linux 2, and CentOS 7.7 or newer are the
easiest linux install environments.

UPnP is enabled by default to open port 8444 for incoming connections.
If this causes issues, you can disable it in config.yaml.
Some routers may require port forwarding, or enabling UPnP
in the router's configuration.

Due to the nature of proof of space lookups by the harvester in the current
release you should limit the number of plots on a physical drive to 50 or less.
This limit will significantly increase soon.

# uvloop

For potentially increased networking performance on non Windows platforms,
install uvloop:
```bash
pip install -e ".[uvloop]"
```

# RPC Interface

The Node has an RPC Interface with [documentation](https://github.com/Chia-Network/chia-blockchain/wiki/RPC-Node-Interface).


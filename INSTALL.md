To install the chia-blockchain, follow the instructions according to your operating system.
After installing, follow the remaining instructions in the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) to run the software. You should read the [release notes](https://github.com/Chia-Network/chia-blockchain/releases) and the wiki/repository [FAQ](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ).

| Jump to: | [Windows](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#Windows) |[MacOS](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#MacOS) | [Ubuntu](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#ubuntudebian) | [WSL2](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#WSL2) | [Amazon Linux 2](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#amazon-linux-2) | [CentOS/RHEL](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#centosrhel-77-or-newer) | [Other platforms](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#other-install-methods-and-environments) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
 
All keys and plots from version prior to Beta 8 (released July 16, 2020) are deprecated and can be deleted. Plots from both Beta 8 and newer should work on mainnet. 

## Drive format support

Chia plot files are at least 108GB in size (for K32). To plot successfully requires drives formatted to support large files; e.g. NTFS, APFS, exFAT, ext4, etc. Drives with FAT formatting (FAT12, FAT16, or FAT32) will fail plotting midway through. Future versions of Chia will check for unsupported drives, but for now it's up to each user to check their drive format.

## Sleep kills plots

The Chia plotting process takes multiple of hours to complete. If the computer or hard drives goes to sleep during the plotting process, the plotting fails and you will need to start over. Please ensure all sleep, hibernate and power saving modes for your computer and hard drives are disabled before starting the Chia plotting process. In the future, Chia will have resume plot feature. In the meantime, if you do get a failed plot, delete all `*.tmp` files before starting a new plot.

## Updating from Release Candidate to 1.0:

Keys and configs from RC3 and newer should automatically migrate. For more details, read the [FAQ](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#how-do-i-upgrade-and-keep-my-keys-and-plots). **No testnet/TXCH coins migrate to mainnet. Mainnet coins are forever, however.**

# Windows

Install the Windows installer - [Chia Blockchain Windows](https://download.chia.net/latest/Setup-Win64.exe)

As the Chia code signing certificate is new you will likely have to ask to keep the download and when you run the installer, you will have to choose "More Info" and "Run Anyway" to be able to run the installer. There is no need to use the command line. Some Windows anti-virus applications are seeing the download as a false positive. You can see the entire source code and build method here so we think it's safe for you to ask those tools to ignore it.

You can now proceed to the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide)

# MacOS
MacOS Mojave (10.14.x) or newer is required.

Install the MacOS dmg installer - [Chia Blockchain MacOS](https://download.chia.net/latest/Setup-MacOS.dmg)

When the installer first runs it will import or create multiple keys and add them to the MacOS keychain. You may be prompted up to 3 times for your password. We suggest choosing "always allow."

You can now proceed to the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide)

To build a development version, make sure [brew](https://brew.sh/) is available before starting the setup and that python 3.7 or newer is installed.
```bash
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
. ./activate

sh install-gui.sh

cd chia-blockchain-gui
npm run electron &
```

# Ubuntu/Debian

Install dependencies for Ubuntu 20.04 LTS. If you are installing on Ubuntu 18.04 LTS you should use Python 3.7 instead: `sudo apt-get install python3.7-venv python3.7-distutils git -y`
```bash
sudo apt-get update
sudo apt-get upgrade -y

# Checkout the source and install
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate

sh install-gui.sh

cd chia-blockchain-gui
npm run electron &
```

To Update/Upgrade from previous version
```
cd chia-blockchain
chia stop -d all
deactivate
git fetch
git checkout 1.0.0
```
If you get RELEASE.dev0 then delete the package-lock.json in chia-blockchain-gui and install.sh again
```
sh install.sh

. ./activate

chia init

sh install-gui.sh

cd chia-blockchain-gui
npm run electron &

```

# WSL2

You can run chia-blockchain in Ubuntu 20.04 LTS via WSL2 on Windows.

NOTE: WSL2 plotting is currently only *slightly* faster than plotting on the native windows client. WSL2 requires significant tweaking to set up correctly. If you find that daunting, it's probably easier to just use the native windows client.

**You can not run the GUI** as WSL2 doesn't yet support graphical interfaces from WSL2. 

## Check if you already have WSL2 or WSL1 installed:
From PowerShell, type:
```
wsl -l -v
```

If you get a listing of help topics for wsl commands, you have WSL1, and need to upgrade. To upgrade, [follow the instructions here](https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2). If you get a blank result or a listing of installed Linux versions, you have WSL2 and are OK to proceed.

## If WSL is not installed:
From an Administrator PowerShell:
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all
```
You will be prompted to reboot. 

## Installing a new WSL2 instance:
Install Ubuntu 20.04 LTS from the Microsoft Store and run it and complete its initial install steps. You now have a linux bash shell environment that can run linux native software on Windows.

Then follow the steps below which are the same as the usual Ubuntu instructions above with a target of Python 3.8.
```bash
sudo apt-get update
sudo apt-get upgrade -y

git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate

```
Running a standalone Windows wallet gui is deprecated but may return in later versions. You can run the Windows version and share keys. You can also plot in WSL2 and migrate the plots to a Windows farmed plot directory.

## Increasing the WSL Maximum Storage Capacity
WSL2 uses a Virtual Hardware Disk (VHD) to store files, and it automatically resizes as files grow. **However, the VHD has an initial maximum size of 256 GB.** Therefore, the default WSL2 VHD is probably only capable of plotting k=30 plots. To plot anything larger, you will need to increase the maximum allowable size. [Follow the guide here.](https://docs.microsoft.com/en-us/windows/wsl/compare-versions#expanding-the-size-of-your-wsl-2-virtual-hardware-disk)

## Setting a maximum limit to WSL2 memory access
If you try plotting Chia in WSL2 without limiting the memory access, WSL2 will use 100% of your available machine's memory, and your computer will get bogged down and begin swapping memory to your hard drive. This will severely cripple your plotting speeds. To set the maximum memory that WSL2 is allowed to use, create a configuration file [as described in this guide](https://www.bleepingcomputer.com/news/microsoft/windows-10-wsl2-now-allows-you-to-configure-global-options/).

## WSL VHD Plotting Nuance
Plotting within WSL2 can write to either the native VHD (which is EXT4) or to any other drive, which can be NTFS or any other FS-type. Writing to the native VHD is faster than writing out to another drive.

Plotting uses three commands for directory control:

`-t` for initial temp directory. Phases 1 and 2 happen here.

`-2` for secondary temp directory. Phase 3 (compression) happens here.

`-d` for final destination. Phase 4 happens here.

Plotting works such that `-t` and `-2` require the exact same amount of storage space. Therefore, if `-t` and `-2` point to the same drive, that drive needs 2x the final file size + 1x the max working file size.

For maximum speed, `-t` and `-2` should be inside the WSL2 filesystem. Something like: `-t ~/chia_temp -2 ~/chia_temp`. Just beware that the WSL2 VHD will need a much larger maximum capacity.

`-d` can point to any other drive for the final destination.


# Amazon Linux 2

```bash
sudo yum update -y
sudo yum install python3 git -y

git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate

# gui
cd chia-blockchain-gui
npm run build
npm run electron

# Or install chia-blockchain as a binary package
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10

pip install chia-blockchain==1.0.0
```

# CentOS/RHEL/Fedora 7/33 and above

```bash
sudo yum update -y

# Compiling python 3.7 is generally required on CentOS 7.7 and newer
sudo yum install gcc openssl-devel bzip2-devel libffi libffi-devel -y
sudo yum install libsqlite3x-devel -y
# possible that on some RHEL based you also need to install
sudo yum groupinstall "Development Tools" -y
sudo yum install python3-devel gmp-devel  boost-devel libsodium-devel -y

sudo wget https://www.python.org/ftp/python/3.7.7/Python-3.7.7.tgz
sudo tar -zxvf Python-3.7.7.tgz ; cd Python-3.7.7
./configure --enable-optimizations; sudo make -j$(nproc) altinstall; cd ..

# Download and install the source version
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
sh install-gui.sh
. ./activate

# gui
cd chia-blockchain-gui
npm run build
npm run electron

# Or install from binary wheels
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10

pip install chia-blockchain==1.0.0

```
# Other install methods and environments
* [Raspberry Pi 3/4](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi)
* [FreeBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/FreeBSD-Install)
* [Ubuntu Binary Install](https://github.com/Chia-Network/chia-blockchain/wiki/Ubuntu-Binary-Install)
* [OpenBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/OpenBSD-Install)


You need Python 3.7 or newer.

Chia strives to provide [binary wheels](https://pythonwheels.com/) for modern systems. If your system does not have binary wheels, you may need to install development tools to build some Python extensions from source. If you're attempting to install from source, setting the environment variable BUILD_VDF_CLIENT to N will skip trying to build Timelord components that aren't very cross platform, e.g. `export BUILD_VDF_CLIENT=N`.

## Create a virtual environment

Your installation goes inside a [virtual environment](https://docs.python-guide.org/dev/virtualenvs/).

There are lots of ways to create and manage a virtual environment. This is just one.

```bash
python3.7 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

Wheels can be in source or binary format. Binary wheels are specific to an operating system and python version number. Source wheels require development tools.

Chia hosts some binary wheels that are not available from [PyPI](https://pypi.org/). This step is optional, but it may succeed where building from source can take a while or fail in hard-to-debug ways. If wheels are not available for your system, this step will fail. But you can try it anyway.

```bash
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10
```

Install chia-blockchain.

```bash
pip install chia-blockchain==1.0.0
```

Before you use chia-blockchain in future, you must "enter" your virtual environment.

```bash
source venv/bin/activate
chia -h
```
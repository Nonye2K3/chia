To install the chia-blockchain, follow the instructions according to your operating system.
After installing, follow the remaining instructions in the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) to run the software. Make sure to review [README.md](https://github.com/Chia-Network/chia-blockchain/blob/master/README.md) as well.

| Jump to: | [Ubuntu](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#ubuntudebian) | [Windows](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#windows) | [Amazon Linux 2](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#amazon-linux-2) | [CentOS/RHEL](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#centosrhel-77-or-newer) | [Other platforms](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#other-install-methods-and-environments) |
| :--- | :---: | :---: | :---: | :---: | :---: |

# MacOS
MacOS Mojave (10.14.x) or newer is required.

Install the MacOS dmg installer - [Chia Blockchain MacOS](https://download.chia.net/beta-1.6-macos/Chia-0.1.6.dmg)

When the installer first runs it will import or create multiple keys and add them to the MacOS keychain. You may be prompted up to 9 times for your password. We suggest choosing "always allow."

To build a development version, make sure [brew](https://brew.sh/) is available before starting the setup and that python 3.7 or newer is installed.
```bash
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
. ./activate

cd electron-react
npm run build
npm run electron
```

# Ubuntu/Debian

Install dependencies for Ubuntu 20.04 LTS. If you are installing on Ubuntu 18.04 LTS you should use Python 3.7 instead: `sudo apt-get install python3.7-venv python3.7-distutils git -y`
```bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y  python3.8-venv python3.8-distutils git
sudo apt-get install -y npm nodejs

# On Ubuntu 18.04 LTS you will need to upgrade node.js
sudo npm cache clean -f
sudo npm install -g n
sudo n stable

# Checkout the source and install
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate
cd electron-react
npm run build
npm run electron

```

# Windows

## Windows native
Download the Windows Installer:
[https://download.chia.net/beta-1.6-win64/ChiaSetup-0.1.6.exe](https://download.chia.net/beta-1.6-win64/ChiaSetup-0.1.6.exe)

When you run the installer, you will have to choose "More Info" and "Run Anyway" to be able to run the currently unsigned installer. There is no need to use the command line, however there are some command line tools located in `~\AppData\Local\Chia\app-0.1.6\resources\app.asar.unpacked\daemon\` Try `.\create_plots -h` or `.\check_plots -h` for example.

You can now proceed to the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide)

## Install WSL2 + Ubuntu 20.04 LTS

From an Administrator PowerShell
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all
```
You will be prompted to reboot. Once that is complete, install Ubuntu 20.04 LTS from the Microsoft Store and run it and complete its initial install steps. You now have a linux bash shell environment that can run linux native software on Windows.

Then follow the steps below which are the same as the usual Ubuntu instructions above with a target of Python 3.8.
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3.8-venv python3.8-distutils git -y

# Either checkout the source and install
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate

# Or install chia-blockchain as a binary package
python3.8 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10 cbor2==5.1.0

pip install chia-blockchain==1.0b6
```

Running a standalone Windows wallet gui is deprecated but may return in later versions.

# Amazon Linux 2

```bash
sudo yum update -y
sudo yum install python3 git -y

git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh

. ./activate

# gui
cd electron-react
npm run build
npm run electron

# Or install chia-blockchain as a binary package
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10 cbor2==5.1.0

pip install chia-blockchain==1.0b6
```

# CentOS/RHEL 7.7 or newer

```bash
sudo yum update -y

# Compiling python 3.7 is generally required on CentOS 7.7 and newer
sudo yum install gcc openssl-devel bzip2-devel libffi libffi-devel -y
sudo yum install libsqlite3x-devel -y

wget https://www.python.org/ftp/python/3.7.7/Python-3.7.7.tgz
tar -zxvf Python-3.7.7.tgz ; cd Python-3.7.7
./configure --enable-optimizations; sudo make -j$(nproc) altinstall; cd ..

# Download and install the source version
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
. ./activate

# gui
cd electron-react
npm run build
npm run electron

# Or install from binary wheels
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10 cbor2==5.1.0

pip install chia-blockchain==1.0b6
```
# Other install methods and environments
* [Raspberry Pi 3/4](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi)
* [FreeBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/FreeBSD-Install)
* [Ubuntu Binary Install](https://github.com/Chia-Network/chia-blockchain/wiki/Ubuntu-Binary-Install)


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
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10 cbor2==5.1.0
```

Install chia-blockchain.

```bash
pip install chia-blockchain==1.0b6
```

Before you use chia-blockchain in future, you must "enter" your virtual environment.

```bash
source venv/bin/activate
chia -h
```
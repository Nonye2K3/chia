# Method 1 - Tested on FreeBSD 11.3-RELEASE

```bash
pkg install lang/gcc

# if you are ssh or remoting into a machine you might want to use 'screen' so that process will continue even if you logout. For more information: https://www.freebsd.org/cgi/man.cgi?query=screen
pkg install screen
pkg install zsh # if you do not already have this installed

# create and enter a venv
python3 -m venv venv
source venv/bin/activate # when using the default tsch shell use: 'activate.csh'

BUILD_VDF_CLIENT=N CXX=g++9 CC=gcc9 pip install chia-blockchain==1.0b5  # takes a while, builds a lot
export LD_LIBRARY_PATH=/usr/local/lib/gcc9

# create config files
chia init

# set "enable_upnp: False" in config.yaml

sed -i .bak 's/enable_upnp: True/enable_upnp: False' ~/.chia/beta-1.0b5/config/config.yaml

```

Now go to town with `chia-start-node` or whatever.

## gcc notes

After installing gcc9, this message appears:

```
To ensure binaries built with this toolchain find appropriate versions
of the necessary run-time libraries, you may want to link using

  -Wl,-rpath=/usr/local/lib/gcc9
```

So it's probably possible to build the libraries in a way that doesn't require `export LD_LIBRARY_PATH=/usr/local/lib/gcc9`. If you know how, click "edit" and dish.

# Method 2 - Building from source

The following procedure has been tested on FreeBSD 11.3 and 12.1.

## Prerequisite package installation

First, install prerequisite packages (as root, or using sudo):

```bash
pkg install git gmake cmake node npm py37-sqlite3-3.7.7_7
```
Note: If a more recent version of Python is already installed, the py37-sqlite3-3.7.7_7 package should be replaced with the version matching the installed version of Python 3.x (which can be found by executing the `pkg search py3.*sqlite3` command).

## Python Cryptography

On FreeBSD 11.3, OpenSSL is too old for the current 3.x versions of Python cryptography. You will need to install [cryptography 2.9.2 from freshports](https://www.freshports.org/security/py-cryptography) before attempting to build chia-blockchian.

```bash
pkg install py37-cryptography
```

## Fetch source code

Then, as any user, use git to fetch the chia-blockchain source code, using SSH or HTTPS:

```bash
# clone via SSH
git clone git@github.com:Chia-Network/chia-blockchain.git
# OR
# clone via HTTPS
git clone https://github.com/Chia-Network/chia-blockchain.git
```

## Build
Change directory into the chia-blockchain directory, and run the main install script. All programs will be installed into a Python virtual environment in the "venv" sub-directory:

```bash
cd chia-blockchain

# for now, build using the dev branch (current master branch has build issues)
git checkout dev

sh install.sh
```

The command line tools should now be available for use in the created venv, which can be activated using:

```bash
cd chia-blockchain
. ./activate
```

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

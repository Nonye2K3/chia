# Building Chia from Source for FreeBSD

_**Tested on FreeBSD 11.3- and 11.4-RELEASE**_

***

## Upgrading Existing Chia Installs

If you're upgrading from a previously built chia installation, exit from your previous venv environment (```deactivate```), create a new directory in which to place the latest Chia (e.g. ```mkdir ~/chia-1.0.5 && cd ~/chia-1.0.5```), clone the latest repo (```git clone https://github.com/Chia-Network/chia-blockchain.git -b latest```), enter it and create a new Python virtual environment within it (```python3 -m venv venv```). Now, activate the newest environment (```. venv/bin/activate```), upgrade pip (```pip install --upgrade pip```). Now you may skip down to the [clvm_rs install section](#clvm_rs) and begin there.

***

## Why This Manual Installation?

Currently the only way to ensure Chia builds on FreeBSD is to do it from the source. By following these instructions to the letter, you should have no problem building the latest Chia from source on a FreeBSD 11.3 or 11.4. This should also work on FreeBSD 12, possibly with some modifications - e.g. if the ports py-cryptography version is newer than 3.3.2, simply edit as needed - or if your preferred Python version is 3.8+ it should all still work considering you modify the package names as necessary.

### Notes on FreeNAS (TrueNAS)

If you had been using NFS or Samba sharing to expose your plots to a harvester on another OS, such as Linux, you can instead build Chia within a jail (see the FreeNAS manual for 'jails'), expose your plot directories to it and run the harvester within. In my experience, it provides lower-latency and more reliable access to the plots since the disks are direct-attached and not being provided through an extra few layers of network protocols.

If you are using a fresh jail created by the FreeNAS web GUI you may need to install openssh and setup a ssh key to login as root because by default it appears PAM password logins do not work. The jail shell CLI provided by the FreeNAS GUI allows copy and pasting so you can easily paste your public-key into /root/.ssh/authorized_keys && chmod -R 700 /root/.ssh.

These instructions would be applicable to 11.3 and 11.4 jails created within FreeNAS 11 only. Version 12 (FreeBSD 12) ✔

### Other Notes

These instructions will have you building both chia-blockchain and clvm_rs from github source, and python-cryptography from FreeBSD's ports.

The result of this build will be the "chia version" showing the current release branch ahead by 1 and in "dev0"; for instance building 1.0.1 results in "chia version" returning "1.0.2.dev0". If someone knows why this is and how to fix it, please, edit and correct this! It does not happen on Linux.

_**These instructions assume a fresh FreeBSD 11 installation!**_

### Discouraged?

Following the instructions in this document will result in a working Chia CLI build on FreeBSD 11 if you follow step-by-step starting from a vanilla FreeBSD installation. Is something broken? Compare the commands you typed, accessible in your **bash** shell history, and match them with each command in this document. If you feel you've messed something up, do the following:

```
# if you have (venv) in your shell prompt, type deactivate
deactivate
# remove the chia-blockchain directory which will contain clvm_rs and the Python venv
rm -rf chia-blockchain
# ... now start again! You don't need to do all the setup steps but instead may start at the upgrade notes above if you had finished up to the py-cryptography ports build.
```

### Pre-requisite package installation

_If starting the build again after a failure and you have not re-installed FreeBSD, don't just skip this package installation section! You may have missed one or more software packages critical to the build._

The 'pkg', 'portsnap' and port build are to be run as root. Everything else can be run from a normal non-root user.

As root, update pkg and ports, and then install all packages as instructed below.

```
# Update your packages and ports; if ports are already installed as part of your fresh install run portsnap update instead of fetch/extract.
pkg update
portsnap fetch && portsnap extract

# Install bash if you have not; the default csh will not suffice for the build scripts.
pkg install bash
# change your shell to bash
chsh -s /usr/local/bin/bash
# run bash
/usr/local/bin/bash
```

Make sure you change the shell for your non-root chia-blockchain user. If you're opting to run Chia as root, you can skip this. If you are root, run this as it appears below; otherwise, you can omit the username because you are already that user.

```
chsh -s /usr/local/bin/bash NONROOT_USERNAME
```

Now proceed with installing the mandatory development tools.

```
pkg install lang/gcc9 gcc gmake cmake

```

### gcc notes

After installing gcc version 9.0, this message appears:

```
To ensure binaries built with this toolchain find appropriate versions
of the necessary run-time libraries, you may want to link using

  -Wl,-rpath=/usr/local/lib/gcc9
```

It's probably possible to build the libraries in a way that doesn't require `export LD_LIBRARY_PATH=/usr/local/lib/gcc9`. If you know how click "edit" and dish.

### Install rust, Python, and everything else.

```
pkg install lang/rust
pkg install lang/python37 py37-pip py37-setuptools py37-wheel py37-sqlite3 py37-cffi py37-virtualenv py37-maturin python 
pkg install node npm git openssl
```

If you are ssh'ing into the machine you might want to use 'screen' so that processes will continue even if you logout. For more information: https://www.freebsd.org/cgi/man.cgi?query=screen. 'tmux' is also a great alternative especially if you use iTerm2 on macOS as it supports native tabs and windows with the '-CC' CLI option.

```
# optional packages
pkg install screen tmux
```

### Repo Cloning and Virtual Environment (venv) Activation

**From this point on, with the exception of the security/py-cryptography port build process (and any other exceptions noted), you may proceed as a normal user.**

```
# Clone the latest chia-blockchain repository, via HTTP:
git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
# or with SSH:
git clone git@github.com:Chia-Network/chia-blockchain.git -b latest
# Note: you can specify the branch by adding "--branch <version>" like: git clone http://github.com/Chia-Network/chia-blockchain.git --branch 1.0.1

```

Create a virtual environment directory 'venv' from *within* the 'chia-blockchain' directory and activate it before proceeding
```
cd chia-blockchain
python3 -m venv venv
source venv/bin/activate 
```

You are now in the virtual environment that Python (and so chia) will use. You should have a "(venv)" prefix to your terminal prompt to confirm the venv is working.

Upgrade pip:
```
pip install --upgrade pip
```

### Building py-cryptography from ports

_**You'll need to switch to root for this part.**_

```
cd /usr/ports/security/py-cryptography

# Instruct 'make' that the SSL library is openssl.
echo "DEFAULT_VERSIONS+=ssl=openssl" >> /etc/make.conf

make
```

You'll probably see a bunch of warnings and notices; these are not errors and it will build.

Do NOT run make install. We will do our own py-cryptography install because 'make install' does not copy to our virtual environment. (If you know how to change this, please edit).


Once complete switch back to your non-root user if you so optioned. You must now be in your venv once again.

### clvm_rs

Build and install the current version of clvm_rs (0.1.6).

```
git clone http://github.com/Chia-Network/clvm_rs.git --branch 0.1.6
cd clvm_rs
maturin develop --release
pip install git+https://github.com/Chia-Network/clvm@use_clvm_rs
```

clvm_rs 0.1.6 is now installed in your virtual environment.

### Install py-cryptography to the venv

Copy py-cryptography and its meta-data from the staging directory to your virtual environment:

```
cp -R /usr/ports/security/py-cryptography/work-py37/stage/usr/local/lib/python3.7/site-packages/cryptography ${VIRTUAL_ENV}/lib/python3.7/site-packages/cryptography
cp -R /usr/ports/security/py-cryptography/work-py37/stage/usr/local/lib/python3.7/site-packages/ ${VIRTUAL_ENV}/lib/python3.7/site-packages/cryptography-3.3.2-py3.7.egg-info
```

Clear any Python byte-code cache files that may contain the old path. These should be re-built by the interpreter but we like a clean environment.
```
find ${VIRTUAL_ENV}/lib/python3.7/site-packages/cryptography -name __pycache__ | xargs -I{} rm -rf "{}"
```

### Chia modifications and Building Chia Itself

Switch to your chia-blockchain clone directory. You will need to edit two files.

Using your favorite text editor, modify setup.py to edit the cryptography package version to 3.3.2.
```
"cryptography==3.4.6" --> to --> "cryptography==3.3.2"
```

Now you must modify chia/util/keychain.py to provide a static key when using the Python keyring. This is mandatory otherwise every time the keyring is accessed your passphrase will need to be entered on the command line, and for the CLI daemon this will not do.

On line 25 of chia/util/keychain.py, change:

```
elif platform=="linux":
```
to:
```
elif platform=="linux" or platform.startswith("freebsd"):
```

On line 27 of the same file, change the passphrase from "your keyring password" to whatever you wish your passphrase to be. This is intended to be fixed in future versions but, for the time being, Linux and FreeBSD must have the keyphrase provided statically.
```
keyring.keyring_key = "your keyring password"  # type: ignore
```
can be changed like so:
```
keyring.keyring_key = "Too Many Secrets"
```

Now, you will build Chia!

```
sh install.sh
```

Once done, run:

```
chia init
```

NOTE: if you need to disable UPnP - a protocol which automatically sets up port-forwarding on routers using NAT which is a typical setup at any residence with broadband - set "enable_upnp: False" in config.yaml. You can use the one-liner below or do it yourself.

```
sed -i .bak 's/enable_upnp: True/enable_upnp: False' ~/.chia/mainnet/config/config.yaml
```

While you don't absolutely need port 8444 forwarded to your Chia node, it is advised that you do so that other peers may connect to you instead of you solely connecting to them. For the average at-home farmer it is advised you do not disable UPnP unless you absolutely know what you're doing or have another node on your local network already using the port and are planning to [Farm on Many Machines](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines)).

***

## Installed and Ready to Farm!

That's it! Provided the instructions were followed to the T, and the build is a fresh FreeBSD 11.3 or 11.4, either hardware or FreeNAS jailed, you should be good to go! Now go to town with `chia start node` or whatever floats your boat.

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

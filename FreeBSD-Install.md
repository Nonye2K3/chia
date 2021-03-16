# Building Chia from Source for FreeBSD 11

_**Tested on FreeBSD 11.3- and 11.4-RELEASE**_

***


Currently, the only way to ensure a FreeBSD build is to do it from source. By following these instructions to the letter, you should have no problem building the latest Chia from source on a FreeBSD 11.3 or 11.4. This should also work on FreeBSD 12, possibly with some modifications - for instance if the ports py-cryptography version is newer than 3.3.2, simply edit as needed - or if your preferred Python version is 3.8 or 3.9 I believe it should all still work considering you modify the package names as necessary.

## FreeNAS / TrueNAS

If you had been using NFS or Samba sharing to expose your plots to a harvester on another OS, such as Linux, you can instead build Chia within a jail (see the FreeNAS manual for 'jails'), expose your plot directories to it and run the harvester within. In my experience, it provides lower-latency and more reliable access to the plots since the disks are direct-attached and not being provided through an extra few layers of network protocols.

If you are using a fresh jail created by the FreeNAS web GUI you may need to install openssh and setup a ssh key to login as root because by default it appears PAM password logins do not work. The jail shell CLI provided by the FreeNAS GUI allows copy and pasting so you can easily paste your public-key into /root/.ssh/authorized_keys && chmod -R 700 /root/.ssh.

## Other Notes

These instruction result in the current dev version showing up in "chia version"; e.g. branch 1.0rc9 -> "1.0rc10.dev0". If someone knows how to keep it in the current release/stable build, please edit this. These instructions will have you building both chia-blockchain and clvm_rs from github's source, and python-cryptography from FreeBSD's ports. This works on vanilla FreeBSD 11.3 and .4 installations and FreeNAS (TrueNAS) jails.

**These instructions assume a fresh FreeBSD installation!**

### Pre-requisite package installation

The 'pkg', 'portsnap' and port build are to be run as root. Everything else can be run from a normal non-root user.

```
# Update your packages and ports; if ports are already installed as part of your fresh install run portsnap update instead of fetch/extract.
pkg update
portsnap fetch && portsnap extract

# Install bash and zsh if you have not; the default csh will not suffice for the build scripts.
pkg install bash zsh
# change your shell to bash (or zsh)
chsh -s /usr/local/bin/bash
# run bash
/usr/local/bin/bash

# Install necessary development tools 
pkg install lang/gcc9 gcc gmake cmake

```

### gcc notes

After installing gcc9, this message appears:

```
To ensure binaries built with this toolchain find appropriate versions
of the necessary run-time libraries, you may want to link using

  -Wl,-rpath=/usr/local/lib/gcc9
```

It's probably possible to build the libraries in a way that doesn't require `export LD_LIBRARY_PATH=/usr/local/lib/gcc9`. If you know how click "edit" and dish.

### Install rust, Python, and everything else.

```
pkg install lang/rust
pkg install lang/py37 py37-pip py37-setuptools py37-wheel py37-sqlite3 py37-cffi py37-virtualenv python
pkg install node npm git openssl
```

If you are sshing into the machine you might want to use 'screen' so that processes will continue even if you logout. For more information: https://www.freebsd.org/cgi/man.cgi?query=screen. 'tmux' is also a great alternative especially if you use iTerm2 on macOS as it supports native tabs and windows with the '-CC' CLI option.

```
pkg install screen tmux
```

**From here you may run as a normal user with any exceptions noted.**

### Repo Cloning and Virtual Environment (venv) Activation

```
# Clone the latest chia-blockchain repository; via HTTP/HTTP/S:
git clone http://github.com/Chia-Network/chia-blockchain.git
# Via SSH:
git clone git@github.com:Chia-Network/chia-blockchain.git
# Note: you can specify the branch by adding "--branch <version>" like: git clone http://github.com/Chia-Network/chia-blockchain.git --branch 1.0rc9

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

### Building clvm_rs 0.1.4

```
git clone http://github.com/Chia-Network/clvm_rs.git --branch 0.1.4
cd clvm_rs
pip install maturin
maturin develop --release
pip install git+https://github.com/Chia-Network/clvm@use_clvm_rs
```

clvm_rs 0.1.4 is now installed in your virtual environment.

### Building py-cryptography from ports

You'll need to switch to root for this part.

```
cd /usr/ports/security/py-cryptography

# Instruct 'make' that the SSL library is openssl.
echo "DEFAULT_VERSIONS+=ssl=openssl" >> /etc/make.conf

make
```

You'll probably see a bunch of warnings and notices; these are not errors and it will build. After it is done building, switch back to your non-root user (if you optioned so). Ensure you are in your venv after switching!

We're going to do our own py-cryptography install since 'make install' does not copy to our venv. (If you know how to change this, please edit).

Copy py-cryptography and its meta-data from the staging directory to your virtual environment.

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

Now you must modify src/util/keychain.py to provide a static key when using the Python keyring. This is mandatory otherwise every time the keyring is accessed your passphrase will need to be entered on the command line, and for the CLI daemon this will not do.

On line 25 of src/util/keychain.py, change:

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

Build Chia!

```
sh install.sh
```
Once done, run:

```
chia init
```

If needing to disable UPnP, set "enable_upnp: False" in config.yaml.

```
sed -i .bak 's/enable_upnp: True/enable_upnp: False' ~/.chia/testnet/config/config.yaml
```

That's it! Provided the instructions were followed to the T, and the build is a fresh FreeBSD 11.3 or 11.4, either hardware or FreeNAS jailed, you should be good to go! Now go to town with `chia start node` or whatever.

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

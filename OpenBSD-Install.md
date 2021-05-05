# Install Chia on OpenBSD

_Tested with Chia 1.1.4 on OpenBSD/amd64 6.8_

```sh
# install required packages
doas pkg_add git python-3.8.6 rust cmake gmake gmpxx

# create a new user with the login class "daemon" so that it can use all
# available memory for plotting, then switch to that user
doas useradd -m -Ldaemon chia
doas -u chia ksh -l
cd

# clone repos
git clone https://github.com/Chia-Network/chia-blockchain.git --branch latest
git clone https://github.com/Chia-Network/clvm_rs.git --branch 0.1.7
git clone https://github.com/PyO3/maturin.git --branch v0.10.3
git clone https://github.com/timkuijsten/chiavdf.git --branch openbsd # chiavdf/pull/71

export BUILD_VDF_CLIENT=N

# create python virtual environment for Chia
cd chia-blockchain/
python3 -m venv venv
. ./venv/bin/activate
pip install --upgrade pip

cd ../chiavdf/
pip install .

cd ../maturin/
# don't pass static compiler flags to the rust linker because that would cause
# a core dump, possibly because of resource limits
sed -i 's|cargo_args.extend(\["--", "-C", "link-arg=-s"\])|#cargo_args.extend(\["--", "-C", "link-arg=-s"\])|' setup.py
pip install .

cd ../clvm_rs/
maturin develop --release

# XXX should be a more elegant way...
cp target/release/libclvm_rs.so ../chia-blockchain/clvm_rs.so

cd ../chia-blockchain/
# use our previous compile results
sed -i 's|"chiavdf==1.0.1"|"chiavdf==1.0.2.dev1"|' setup.py

# use a hardcoded random secret so the software can run headless and without
# user intervention
sed -i 's|elif platform == "linux":|elif platform == "linux" or platform.startswith("openbsd"):|' chia/util/keychain.py
_keyring=$(dd status=none if=/dev/random bs=8 count=1 | od -H | tr -d ' ' | head -1 | cut -b8-25)
sed -i 's|keyring.keyring_key = "your keyring password"|keyring.keyring_key = "'"$_keyring"'"|' chia/util/keychain.py
unset _keyring

sh install.sh

# DONE, Chia is installed now, start using it by creating a config and keys

chia init
chia keys generate

# if you are going to setup port forwarding, disable upnp
chia configure --enable-upnp false

chia start node wallet farmer harvester
```

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

# GUI Build / Usage

*WARNING: the following has only been tested with Chia 1.0beta7 on OpenBSD/amd64 6.7*

The build instructions in the previous sections above must be completed successfully before attempting to build the GUI using the procedure below.

*WARNING: Although the following steps have been used successfully, the resulting GUI will be run with an older version of electron than is recommended by the Chia Network team. This may result in unexpected problems.*

## Prerequisite package installation

As root (or using doas / sudo), first install some additional OpenBSD packages required for GUI usage:

```bash
pkg_add -i electron
```

## Build

```bash
cd chia-blockchain
. ./activate

cd chia-blockchain-gui

# build / set up GUI
npm run build

# Remove failed electron 8.2.5 install and fall back to the OpenBSD
# ports tree 8.2.0 electron, which currently (as of 6/10/2020) works.
#
# This may not continue to work in the future.  A full solution to
# this requires official OpenBSD electron builds, provided by the
# electron project itself.

rm -rf node_modules/electron
```

## Launch GUI
The GUI can now be launched using the following commands:

```bash
cd chia-blockchain
. ./activate

cd chia-blockchain-gui
npm run electron
```
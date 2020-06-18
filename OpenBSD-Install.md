The installation procedure below has been tested on OpenBSD 6.7, amd64.

# Command line tools

## Prerequisite package installation

As root (or using doas / sudo), first install some prerequisite OpenBSD packages:

```bash
pkg_add -i git python3 cmake gmake node gmpxx
```

## Fetch source code

Then, as any user (preferably not root), use git to fetch the chia-blockchain source code, using SSH or HTTPS:

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

*Note: During the install above an electron installation will fail. For CLI usage this can be ignored.*

The command line tools should now be available for use in the created Python virtual environment, which can be activated using:

```bash
cd chia-blockchain
. ./activate
```

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

# GUI Build / Usage

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

cd electron-react

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

cd electron-react
npm run electron
```
# IMPORTANT: These installation notes will apply for a future release of the Chia Network tools. The current 1.0beta7 release does not cleanly build using the procedure below.

The installation procedure below has been tested on OpenBSD 6.7, amd64.

# Prerequisite package installation

As root (or using doas / sudo), first install some prerequisite OpenBSD ports:

```bash
pkg_add -i git python3 cmake gmake electron node gmpxx
```

# Fetch source code

Then, as any user, use git to fetch the chia-blockchain source code, using SSH or HTTPS:

```bash
# clone via SSH
git clone git@github.com:Chia-Network/chia-blockchain.git
# OR
# clone via HTTPS
git clone https://github.com/Chia-Network/chia-blockchain.git
```

# Build
Change directory into the chia-blockchain directory, and run the main install script. All programs will be installed into a Python virtual environment in the "venv" sub-directory:

```bash
cd chia-blockchain
sh install.sh
```

The command line tools should now be available for use in the created venv, which can be activated using:

```bash
cd chia-blockchain
. ./activate
```

More details can be found in the [Chia Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide).

# GUI Build / Usage

## WARNING: although the following steps have been used successfully, the resulting GUI will be run with an older version of electron than is recommended by the Chia Network team. This may result in unexpected problems.

```bash
cd chia-blockchain

. ./activate

cd electron-react

# build / set up GUI
npm run build

# Remove failed electron 8.2.5 install and fall back to the OpenBSD ports tree 8.2.0 electron,
# which currently (as of 6/10/2020) works but may not continue to work in the future.
#
# A full solution to this requires official OpenBSD electron builds, provided by the
# electron project itself.

rm -rf node_modules/electron

# Launch GUI
npm run electron
```
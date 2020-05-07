The following recipe was tested on a Pi 4 running Ubuntu Server 20.04 LTS 64 bit. 64 bit OSes and python 3.7+ are required but helpfully Ubuntu 20.04 has python 3.8 out of the box.

This was tested with [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) and Image _Ubuntu Server 20.04 LTS (Pi 3/4) 64 bit_.


Make sure you have some swap space, 2048MB is suggested:
```bash
sudo dd if=/dev/zero of=/swap bs=1M count=2048
sudo chmod 600 /swap ; sudo mkswap /swap ; sudo swapon /swap
```
Add this line to /etc/fstab if you want swap available on reboot. This is less necessary as swap is only required during the building of chiapos, chiavdf, and blspy. However, if you plan to run Ubuntu Desktop and the GUI Wallet, you will need the swap space on subsequent reboots.
```bash
/swap swap swap defaults 0 0
```
Install build prerequisites:
```bash
sudo apt-get update; sudo apt-get upgrade -y
sudo apt-get install build-essential cmake libgmp-dev libffi-dev libssl-dev -y
sudo apt-get install python3-venv libboost-python-dev -y
```
This install will pull chia-blockchain components from [PyPi](https://pypi.org/)
```bash
python3 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install wheel
pip install setuptools_scm
```
There is one piece of magic. This environment variable is set so that chiavdf doesn't attempt to compile Timelord components. The Pi isn't cut out to be a Timelord and the Timelord requirements are very x86-64 specific currently.
```bash
export BUILD_VDF_CLIENT=N
```
Before attempt to install chia-blockchain, install these components first:
```
pip install miniupnpc setproctitle setuptools_scm
```
Finally attempt to install chia-blockchain:
```
pip install chia-blockchain==1.0b5
```

The chia-blockchain source or source python module can be downloaded locally as an alternate final build step. The `sh install.sh` script in the source repository should "just work" once you've completed the steps up to `pip install chia-blockchain` though you will want your `venv` to be in the chia-blockchain directory and not the current directory as assumed above. If the install.sh script doesn't work for you try `pip install .` in the chia-blockchain directory instead.

This should work on Pi 3 with 64 bit Ubuntu but has not been tested. Please update this if that changes.

As noted above the Raspberry Pi is not cut out to be a Timelord or a plotter. It makes an excellent node/farmer/harvester however and is an economical machine to run and farm plots made on faster plotting machines and then transferred to it to harvest/farm.

## Tips for installing GUI Wallet

Ubuntu 20.04 LTS requires `sudo apt-get install libxss1` for Electron

In Beta 1.5 the `chia start wallet &` command often will not work. Instead try `npm run --prefix electron-ui start &` from the chia-blockchain repository checkout directory.
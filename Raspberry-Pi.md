The following recipe was tested on a Pi 4 running Ubuntu Server 20.04 LTS 64 bit. 64 bit OSes and python 3.7+ are required but helpfully Ubuntu 20.04 has python 3.8 out of the box.

This was tested with [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) and Image _Ubuntu Server 20.04 LTS (Pi 3/4) 64 bit_. We now make available manylinux2014 ARM64 binary wheels for the main chia dependencies.

Make sure you have some swap space, 2048MB is suggested:
```bash
sudo dd if=/dev/zero of=/swap bs=1M count=2048
sudo chmod 600 /swap ; sudo mkswap /swap ; sudo swapon /swap
```
Add this line to /etc/fstab if you want swap available on reboot. This is less necessary as swap is only required during the building of chiapos, chiavdf, and blspy. However, if you plan to run Ubuntu Desktop and the GUI, you will need the swap space on subsequent reboots.
```bash
/swap swap swap defaults 0 0
```
Install build prerequisites:
```bash
sudo apt-get update; sudo apt-get upgrade -y
sudo apt-get install build-essential cmake libgmp-dev libffi-dev libssl-dev -y
sudo apt-get install python3-venv libboost-python-dev -y
```
Starting with version 1.0 beta 6 you will need a cmake version of 3.14 or newer. Ubuntu 20.04LTS ships with a perfectly adequate CMake version 3.16.3. Compiling third party dependencies - especially pycryptography - can take a while.

```bash
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
. ./activate
chia init

# Optionally generate keys
chia keys generate

# Or you can import from 24 words
chia keys -m [24 words]
```

There is one piece of magic. You don't need this magic anymore now that chiavdf comes from a binary wheel on PyPi but we're leaving this here for people trying to build in other environments. This environment variable is set so that chiavdf doesn't attempt to compile Timelord components. The Pi isn't cut out to be a Timelord and the Timelord requirements are very x86-64 specific currently.
```bash
export BUILD_VDF_CLIENT=N
```

This should work on Pi 3 with 64 bit Ubuntu but has not been tested. Please update this if that changes.

As noted above the Raspberry Pi is not cut out to be a Timelord. With our switch from AES to Chacha8 it is feasible to plot with the Pi but it's slow. Modern desktops and laptops plot in the 0.07 - 0.10 GiB/minute range and the Pi 4 plots at 0.025 GiB/minute. [Plotting times for Pi 4](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes#raspberry-pi-4) and other machines are available. Pi makes an excellent node/farmer/harvester however and is an economical machine to run and farm plots made on faster plotting machines and then transferred to it to harvest/farm.

## Installing and running the GUI

```
sh install-gui.sh
cd electron-react
npm run electron &
```

## Headless

You can run without the GUI using commands like `chia init` and `chia start farmer`. Be sure to check out `chia show -h` if you do.
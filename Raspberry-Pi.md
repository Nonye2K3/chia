The following recipe was tested on a Pi 4 (4GiB RAM) running both Ubuntu Server 20.04 LTS 64 bit and Raspbian 64 bit. 64 bit OSes and python 3.7+ are required but helpfully Ubuntu 20.04 has python 3.8 out of the box and Raspbian ships with python 3.7.

This was tested with [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/), using image _Ubuntu Server 20.04 LTS (Pi 3/4) 64 bit_, and Raspbian 64 bit using the _2020-08-20-raspios-buster-arm64.zip_ image. We make available manylinux2014 ARM64 binary wheels for the main chia dependencies which makes installing on Raspberry Pi pretty easy. 

You will need to set up or adjust swap space if you want to build or run the GUI. If you just want to run headless you can skip the swap steps.

For Ubuntu 20.04 LTS, 1024 is suggested:
```bash
sudo dd if=/dev/zero of=/swap bs=1M count=1024
sudo chmod 600 /swap ; sudo mkswap /swap ; sudo swapon /swap
```

Add this line to /etc/fstab so that swap available on reboot. If you plan to run Ubuntu Desktop and the GUI, you will need the swap space on subsequent reboots.

```bash
/swap swap swap defaults 0 0
```

For Raspbian 64:

You need 1000/1024MiB of swap space. Here is an excellent [walk through of increasing swap space](https://pimylifeup.com/raspberry-pi-swap-file/) on Raspbian 64.

Now:

```bash
git clone https://github.com/Chia-Network/chia-blockchain.git
cd chia-blockchain

sh install.sh
. ./activate
chia init

# Optionally generate keys - can be done in the GUI later
chia keys generate

# Or you can import from 24 words
chia keys add -m [24 words]
```

There is one piece of magic. You don't need this magic anymore now that chiavdf comes from a binary wheel on PyPi but we're leaving this here for people trying to build in other environments. This environment variable is set so that chiavdf doesn't attempt to compile Timelord components. The Pi isn't cut out to be a Timelord and the Timelord requirements are very x86-64 specific currently.
```bash
export BUILD_VDF_CLIENT=N
```

This should work on Pi 3 with 64 bit Ubuntu but has not been tested. Please update this if that changes.

The Raspberry Pi is not cut out to be a Timelord. With our switch from AES to Chacha8 it is feasible to plot with the Pi but it's slow. Modern desktops and laptops plot in the 0.07 - 0.10 GiB/minute range and the Pi 4 plots at 0.025 GiB/minute. [Plotting times for Pi 4](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes#raspberry-pi-4) and other machines are available. Pi makes an excellent node/farmer/harvester however and is an economical machine to run and farm plots made on faster plotting machines and then transferred to it to harvest/farm.

## Installing and running the GUI on Ubuntu 20.04 or Raspbian 64 bit

```
sh install-gui.sh
cd electron-react
npm run electron &
```

## Headless

You can run without the GUI using commands like `chia init`, `chia start farmer`, and `watch 'chia show -s -c'`. Be sure to check out `chia show -h` if you do.
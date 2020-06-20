New install instructions at: https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL#ubuntudebian

This was tested on

```bash
sudo docker run --network host --name ubuntu --rm -i -t ubuntu bash
```

which is very bare bones Ubuntu 18.04 install. This runs well on 18.04 LTS or newer versions of Ubuntu.

Most will want to start here. Ensure you have python 3.7 installed:
```bash
sudo apt update && apt install python3.7-venv python3.7-distutils -y
```

Create a virtual env and upgrade pip, as pip 19.2.3 does not support binary wheels on Linux:
```bash
python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
```

Install three binary wheels Chia Network's alternate source:
```bash
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10 cbor2==5.1.0
```

Install everything else:
```bash
pip install chia-blockchain==CURRENTVERSION
```
Where CURRENTVERSION is e.g. `1.0b5`.
Now you can start using chia with a command like:
```bash
chia-start-node &
```
Refer to the [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) for details on how to run the Chia blockchain components.
This was tested on

```bash
sudo docker run --network host --name ubuntu --rm -i -t ubuntu bash
```

which is very bare bones Ubuntu 18.04 install. This runs well on 18.04 LTS or newer versions of Ubuntu.

Ensure you have python 3.7 installed:
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
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.0.2 setproctitle==1.1.10 cbor2==5.0.1
```

Install everything else:
```bash
pip install chia-blockchain==1.0.beta2
```

Now you can start using chia with a command like:
```bash
chia-start-node &
```

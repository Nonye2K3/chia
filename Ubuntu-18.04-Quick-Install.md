This was tested on

```bash
sudo docker run --network host --name ubuntu --rm -i -t ubuntu bash
```

which is very bare bones Ubuntu 18.04 install.


Set up ubuntu.

```bash
cd ~
apt update && apt install gnupg -y
```

Add new source. Not required on Ubuntu 19 or later.

```bash
echo deb http://ppa.launchpad.net/deadsnakes/ppa/ubuntu bionic main > /etc/apt/sources.list.d/deadsnakes-ubuntu-ppa-bionic.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA6932366A755776
apt update && apt install python3.7-venv -y
```


Create a virtual env and upgrade pip, as pip 19.2.3 does not support binary wheels on Linux.

```bash
python3.7 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```


Install two binary wheels from alternate source

```bash
pip install -i https://hosted.chia.net/simple/ miniupnpc==0.1.dev5 setproctitle==1.1.10
```

Install tools for cbor2

```bash
apt install python3.7-dev gcc -y
```

Install cbor2
```bash
pip install certifi  # or it can't validate certs to download setuptools_scm
pip install cbor2==5.0.1
```


Install everything else

```bash
pip install -i https://test.pypi.org/simple/ chia-blockchain==1.6.dev625 --extra-index-url https://pypi.org/simple/
```


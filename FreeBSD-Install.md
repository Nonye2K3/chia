# Tested on FreeBSD 11.3-RELEASE

```bash
pkg install gcc9-9.2.0
BUILD_VDF_CLIENT=N CXX=g++9 CC=gcc9 pip install chia-blockchain==1.0b1  # takes a while, builds a lot
export LD_LIBRARY_PATH=/usr/local/lib/gcc9

# create config files (a hack for 1.0b1)
chia

# set "enable_upnp: False" in config.yaml

sed -i .bak 's/enable_upnp: True/enable_upnp: False' ~/.chia/beta-1.0b1/config/config.yaml

```

Now go to town with `chia-start-node` or whatever.
# Tested on FreeBSD 11.3-RELEASE

```bash
pkg install gcc9-9.2.0

# create and enter a venv
python3.7 -m venv venv
source venv/bin/activate

BUILD_VDF_CLIENT=N CXX=g++9 CC=gcc9 pip install chia-blockchain==1.0b2  # takes a while, builds a lot
export LD_LIBRARY_PATH=/usr/local/lib/gcc9

# create config files (a hack for 1.0b2)
chia

# set "enable_upnp: False" in config.yaml

sed -i .bak 's/enable_upnp: True/enable_upnp: False' ~/.chia/beta-1.0b2/config/config.yaml

```

Now go to town with `chia-start-node` or whatever.

## gcc notes

After installing gcc9, this message appears:

```
To ensure binaries built with this toolchain find appropriate versions
of the necessary run-time libraries, you may want to link using

  -Wl,-rpath=/usr/local/lib/gcc9
```

So it's probably possible to build the libraries in a way that doesn't require `export LD_LIBRARY_PATH=/usr/local/lib/gcc9`. If you know how, click "edit" and dish.
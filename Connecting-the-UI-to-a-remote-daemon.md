# Setup

## On the daemon host

### Expose the daemon to the network

In `config.yaml`, change `self_hostname` from `localhost` to `0.0.0.0`. This binds the daemon to all IPv4 addresses on the local machine.

Next, open the port that the daemon is listening on (55400 by default). The UI assumes that the daemon is already running and it will _not_ attempt to start a remote host. Using [ufw](https://help.ubuntu.com/community/UFW) and restricting traffic to just the UI's host:

````bash
sudo ufw allow from <IP of UI machine> to any port 55400 proto tcp
````

### Copy the daemon's cert files

To secure their connection, the UI will need the daemon's certificates. Copy these files to the UI machine:

````bash
~/.chia/mainnet/config/ssl/daemon/private_daemon.crt
~/.chia/mainnet/config/ssl/daemon/private_daemon.key
````

## On the UI host

_right now (03/31/2021) this only works when the *CHIA_ROOT* environment variable is set to the correct location of `~/.chia/<currentvesion>/`_

Place the daemon's cert files, copied earlier, in the following location:

````bash
~/.chia/mainnet/config/ssl/ui/
~/.chia/mainnet/config/ssl/ui/
````

Find the `ui` section in `config.yaml` and specify the following settings:

````yaml
daemon_host: <name or IP of the daemon host>
daemon_port: 55400
daemon_ssl:
  private_crt: config/ssl/ui/private_daemon.crt
  private_key: config/ssl/ui/private_daemon.key
````

# Troubleshooting

## GUI Client

### Can the GUI find the config folder?
The first thing to check is that the daemon's websocket URI shows up on the title bar. It should look like this:

![image](https://user-images.githubusercontent.com/5160233/111890456-6ca97f00-89b7-11eb-8f20-a8dc80d0d138.png)

If the title bar doesn't look like that ensure that the `CHIA_ROOT` environment variable is set to the path to your `mainnet` folder (`~/.chia/mainnet/` on Ubuntu). Also make sure there isn't a [syntax error](https://yamlchecker.com/) in config.yaml.

### Can the GUI find the remote daemon's certs?

Double check that in the `ui` section the crt and key paths are correct. It _shouldn't_ point to the folder where the local certs are stored. It has to point to the folder wheer you copied the dameon's certs.

## Connectivity

### Has the daemon been bound to a routable IP address?
On the daemon host run `sudo netstat -tulpn | grep 55400` or your OS's equivalent. It should show something similar to `tcp        0    0 0.0.0.0:55400    0.0.0.0:*    LISTEN    2925/chia_daemon`.

If you see `127.0.0.1` it means you haven't changed the daemon's bind IP address. The loopback address is not routable on the network. Double check that `self_hostname: 0.0.0.0` is correct in the config. Also make sure you have fully restarted the daemon:

````bash
chia stop all -d
chia start farmer
````

### Is the daemon's port opened on the firewall?

Run `sudo ufw status | grep 55400` or your OS and firewall equivalent. You should see something like `55400/tcp                  ALLOW`.
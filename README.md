# DNSrebinder

DNSrebinder is a minimal DNS server that can be used to test/verify DNS rebinding vulnerabilities. It is based on the Python DNS library [dnslib](https://github.com/paulc/dnslib). DNSrebinder allows you to define various settings on the command line, including the number of requests before the actual rebinding should occur.

## Installation

The recommended way is to use a Python virtual environment

```
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
```

On systems that are using systemd, you need to temporary disable systemd-resolved as this service listen on port 53:

```
sudo systemctl stop systemd-resolved
```
To re-enable it

```
sudo systemctl start systemd-resolved
```

Please make sure that you have a DNS-NS record that points to the system that is running DNSrebinder.


## Usage

Example usage:
```bash
$ python3 dnsrebinder.py --domain rebind.mydomain.eu. --rebind 127.0.0.1 --ip 8.8.8.8 --counter 1 --restart-after 1
...
```

This starts a DNS server on port 53 listening on UDP and TCP. The first one(--counter 1) requests will be answered with 8.8.8.8. The one (--restart-after 1) request following right after that will be answered with the rebind address 127.0.0.1 (--rebind 127.0.0.1) and then it will restart.

From the victim perspective it looks like : `legitimate` - `rebinded` - `legitimate` - `rebinded`, and so on...


Options overview:
```bash
$ python3 dnsrebinder.py -h
usage: dnsrebinder.py [-h] [--port PORT] [--tcp] [--udp]
                      --domain DOMAIN [--ttl TTL] [--bind BIND]
                      [--ip IP] [--rebind REBIND] [--counter COUNTER]
                      [--restart-after RESTART_AFTER]

Start a DNS implemented in Python. Usually DNSs use UDP on port 53.

options:
  -h, --help            show this help message and exit
  --port PORT           The port to listen on.
  --tcp                 Listen to TCP connections.
  --udp                 Listen to UDP datagrams.
  --domain DOMAIN       The domain to listen for
  --ttl TTL             TTL value of DNS responses
  --bind BIND           IP Adress for server to listen on
  --ip IP               IP Adress used to respond
  --rebind REBIND       IP address for rebind
  --counter COUNTER     Number of requests before rebinding
  --restart-after RESTART_AFTER
                        Number of rebind responses to send before
                        restarting from the legitimate address (0 =
                        never restart, keep rebinding forever)
```

## Contributing

Feel free to contribute.

## Authors
* **Timo Müller** - *Original script* - [mtimo44](https://twitter.com/mtimo44)
* **Hans-Martin Münch** - *Re-Write with dnslib* - [h0ng10](https://twitter.com/h0ng10)
* **Karsten Zeides** - *Command line options, cleanup* [zeides](https://github.com/zeides)
* **gbrls** - *Output cleanup* [gbrls](https://github.com/gbrls)
* **Neoreo** - *Command line options* [Neoreo](https://github.com/Ne0re0)
# Hetzner DNS Authenticator certbot plugin
[![codecov](https://codecov.io/gh/ctrlaltcoop/certbot-dns-hetzner/branch/master/graph/badge.svg)](https://codecov.io/gh/ctrlaltcoop/certbot-dns-hetzner)
![Tests, Coverage](https://github.com/ctrlaltcoop/certbot-dns-hetzner/workflows/Tests,%20Coverage/badge.svg?branch=master)
[![PyPI version](https://badge.fury.io/py/certbot-dns-hetzner.svg)](https://badge.fury.io/py/certbot-dns-hetzner)
![Supported Python](https://img.shields.io/pypi/pyversions/certbot-dns-hetzner)

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/certbot-dns-hetzner)

This certbot plugin automates the process of
completing a dns-01 challenge by creating, and
subsequently removing, TXT records using the Hetzner DNS API.

## Install

Install this package via snap after certbot has been installed via snap

```
snap install certbot-dns-hetzner
snap connect certbot:plugin certbot-dns-hetzner
snap connect certbot-dns-hetzner:certbot-metadata certbot:certbot-metadata
```

to verify that the installation was successfull execute the `certbot plugins`, which should list the plugin 

```
* dns-hetzner
Description: Obtain certificates using a DNS TXT record (if you are using
Hetzner for DNS).
Interfaces: IAuthenticator, IPlugin
Entry point: dns-hetzner = certbot_dns_hetzner.dns_hetzner:Authenticator
```

when the plugin is not listed, you should check that `snap connections` has the following entries

```
Interface            Plug                                  Slot                         Notes
content[metadata-1]  certbot-dns-hetzner:certbot-metadata  certbot:certbot-metadata     manual
content[certbot-1]   certbot:plugin                        certbot-dns-hetzner:certbot  manual
```

## Usage

To start using DNS authentication for the Hetzner DNS API, pass the following arguments on certbot's command line:

| Option                                                     | Description                                      |
|------------------------------------------------------------|--------------------------------------------------|
| `--authenticator dns-hetzner`                              | select the authenticator plugin (Required)       |
| `--dns-hetzner-credentials`                                | Hetzner DNS API credentials INI file. (Required) |
| `--dns-hetzner-propagation-seconds`                        | Seconds to wait for the TXT record to propagate  |

## Credentials


From the hetzner DNS control panel at https://dns.hetzner.com go to "API Tokens" and add a personal access token.

An example ``credentials.ini`` file:

```ini
dns_hetzner_api_token = nohnah4zoo9Kiejee9aGh0thoopee2sa
```
## Examples
To acquire a certificate for `example.com`
```shell script
certbot certonly \\
 --authenticator dns-hetzner \\
 --dns-hetzner-credentials /path/to/my/hetzner.ini \\
 -d example.com
```

To acquire a certificate for ``*.example.com``
```shell script
   certbot certonly \\
     --authenticator dns-hetzner \\
     --dns-hetzner-credentials /path/to/my/hetzner.ini \\
     -d '*.example.com'
```
     
## Thanks to

Of course certbot, which examples and documentation I used to implement this plugin. And to https://github.com/m42e/certbot-dns-ispconfig which served as an excellent example and README template as well.


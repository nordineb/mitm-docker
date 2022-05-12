# MITM with docker
Run mitmproxy and a VPN server in a docker container for easily sniffing traffic from mobile apps. The mobile and the host must be on the same network.
 

## Run the docker container
Customize the file `vpn.env` (VPN_IPSEC_PSK, VPN_USER, VPN_PASSWORD)
```bash
docker-compose up
```

## VPN and root certificate on the mobile
Customize the file `.envrc`, it should match the values in the file `vpn.env` (VPN_IPSEC_PSK, VPN_USER, VPN_PASSWORD. 
The IP should be the IP of the docker host.

```bash
source .envrc
perl -pe 's;(\\*)(\$([a-zA-Z_][a-zA-Z_0-9]*)|\$\{([a-zA-Z_][a-zA-Z_0-9]*)\})?;substr($1,0,int(length($1)/2)).($2&&length($1)%2?$2:$ENV{$3||$4});eg' vpn-proxied.tmpl.mobileconfig > vpn-proxied.mobileconfig
```

* Send (airdrop) `vpn-proxied.mobileconfig` to the phone
* Once the profile is received, install it
* Start the VPN client
* Open `http://mitm.it` using Safari and download the Root CA for your device
* Under Settings, install the profile
* Enable the Root certificate under General > About > Trusted Certificates > 

## Usage
open http://localhost:8081 on the host to see MITM UI  
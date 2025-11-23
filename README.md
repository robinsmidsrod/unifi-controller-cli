# UniFi Controller CLI

Command-line interface to UniFi Controller API.

## Usage

```
$ unifi-controller-cli help
Usage: unifi-controller-cli [<options>] <command>

Options:
 -f config_file (string, default: config.ini in script directory)
 -b base_url    (string, default: https://unifi:8443/)
 -h hosts_file  (string, default: empty)
 -u username    (string, default: empty, required)
 -p password    (string, default: empty, required)
 -s site_desc   (string, default: empty, uses site 'default')
 -d debug       (boolean, default: false)

No command specified, showing help:

device dump                  - dump devices
device list [<name>]         - show devices (by name)
device port [<name>]         - show device ports (by name)
client dump                  - dump clients
client list [<name>]         - show clients (by name)
client update [<name>]       - update client name and note (by name)
switch_profile dump          - dump switch profiles
switch_profile list [<name>] - show switch profiles (by name)
help                         - This page
```

Show all defined clients:
```
$ unifi-controller-cli client list
Client -                    - f4:79:60:88:XX:XX - HuaweiTe -
Client - appletv            - b8:78:2e:1c:XX:XX - Apple    - Apple TV - basement living room
Client - bedroom-chromecast - 48:d6:d5:00:XX:XX - Google   - Chromecast - Google - bedroom
Client - denon              - 00:05:cd:37:XX:XX - D&MHoldi - Denon Receiver - living room
Client - dryer              - 68:a4:0e:53:XX:XX - BshHausg - Dryer - Bosch WTX8HKL9SN - laundry
Client - eh_bedroom         - 2c:f4:32:3c:XX:XX - Espressi - NodeMCUv3 - ESPHome Bedroom
Client - eh_laundry         - 48:3f:da:7e:XX:XX -          - NodeMCUv3 - ESPHome Laundry
Client - epson              - 00:26:ab:8e:XX:XX - SeikoEps - Epson Stylus Photo PX720WD - office
Client - hp                 - a0:1d:48:b7:XX:XX - HewlettP - HP EliteBook 8470p Laptop - office
```

Update client names and notes from hosts.csv:
```
$ cat /path/to/hosts.csv
mac;addr;hostname;comment
b8:27:eb:11:XX:XX;5;octopi;Raspberry Pi 3B+ - office
3c:52:82:29:XX:XX;8;laserjet;HP Color LaserJet Pro M252dw - office

$ unifi-controller-cli client update
Modified 3c:52:82:29:XX:XX, name=laserjet, note=HP Color LaserJet Pro M252dw - office
```


Show switch port names and profile assignments for a single switch:
```
$ unifi-controller-cli device port office-switch
 1 - office-pc                              - LAN
 2 - hp-laptop                              - LAN
 3 - wall-3-server-room                     - LAN + Internal and IPTV VLANs
 4 - wall-4-server-room (aggregates port 3) - All
 5 - office-ap                              - LAN + Internal VLANs
 6 - laserjet                               - LAN
 7 - wall-1-livingroom-left                 - LAN
 8 - wall-2-livingroom-right                - LAN + Internal and IPTV VLANs

```

Show all switch port profiles and their VLAN assignments:
```
$ unifi-controller-cli switch_profile list
Switch Profile - All                   - all       -                  -
Switch Profile - IPTV                  - native    - IPTV             -
Switch Profile - Internet              - native    - Internet         -
Switch Profile - Disabled              - disabled  -                  -
Switch Profile - Guest                 - native    - Guest            -
Switch Profile - LAN                   - native    - LAN              -
Switch Profile - LAN + Internal VLANs  - customize - LAN              - Guest
Switch Profile - None + ISP VLANs      - customize -                  - IPTV, Internet
```

## Installation

Make sure you have the [Carton module dependency manager](https://metacpan.org/pod/Carton) for Perl installed.

```
$ git clone https://github.com/robinsmidsrod/unifi-controller-cli.git
$ cd unifi-controller-cli
$ git checkout main
$ carton install --deployment
# Useful if you want it in your PATH
$ cd ~/bin && ln -s ~/unifi-controller-cli/unifi-controller-cli .
```

## Configuration

Create the file `config.ini` next to *unifi-controller-cli* script with the following content:
```
base_url=https://unifi:8443/
username=someuser
password=somepassword
site_desc=Somesite
hosts_file=/path/to/hosts.csv
debug=0
```
Ensure the config file is private, because it contains credentials:
```
$ chmod 0600 config.ini
```

## UniFi API documentation

Documentation for the UniFi API can be found here:

https://ubntwiki.com/products/software/unifi-controller/api

## Legal

Copyright Robin Smidsrød 2021

Licensed under [MIT License](LICENSE).

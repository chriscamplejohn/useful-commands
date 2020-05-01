# Intro
These are useful commands for working with Linux networking.

# Glossary
| Term | Description |
| ---- | ----------- |
| NAT | Network Address Translation |

# Tools
| Name | Description |
| ---- | ----------- |
| [Aircrack-ng](https://www.aircrack-ng.org/) | Suite of tools for checking wireless network security |

# Commands

### Install drivers for Realtek RTL8812AU chipset USB WiFi device
```apt install realtek-rtl88xxau-dkms```

### List All Network Interfaces
This is also the entry point for most network interface operations

```ifconfig```

### List Wireless Network Intefaces
This is also the entry point for most wireless network interface operations

```iwconfig```

### Disable an interface
Change wlan0 as appropriate for the interface to disable

```ifconfig wlan0 down```

### Enable an interface
Change wlan0 as appropriate for the interface to enable

```ifconfig wlan0 up```

### Change MAC address
NOTE: This is a temporary change until the machine reboots!

NOTE: The network manager may interfere and change it back - see below to stop it doing that

Change wlan0 as appropriate for the interface to change the MAC address on

Change 00:11:22:33:44:55 as appropriate for the MAC address you want to use

```ifconfig wlan0 hw ether 00:11:22:33:44:55```

### Stop Network Manager changing MAC address back
Edit network manager config file (using whatever means you like). e.g.

```leafpad /etc/NetworkManager/NetworkManager.conf```

Add the following

```
[device]
wifi.scan-rand-mac-address=no

[connection]
ethernet.cloned-mac-address=preserve
wifi.cloned-mac-address=preserve
```

The Network Manager now needs restarting - see below

### Restart Network Manager

```service network-manager restart```

### Check for any processes that might interfere with airmon-ng
```airmon-ng check```

### Kill any processes that might interfere with airmon-ng
```airmon-ng check kill```

### Check if chipset support injection
Change access point BSSID (-a) as required

Change the network ESSID (-e) as required

Change wlan0 as appropriate for the interface to monitor on 

```aireplay-ng -9 -a 00:00:00:00:00:00 -e "MyNetworkESSID" wlan0```

Should see

```Injection is working!```

### Put card in monitor mode
NOTE: Interface must not be up

WARNING: MUST disable any processes that might interfere with airmon-ng

Change wlan0 as appropriate for the interface to monitor on

```iwconfig wlan0 mode monitor```

OR

```airmon-ng start wlan0```

NOTE: Probably want to bring the interface up now

### Sniff for 2.4GHz wireless networks
NOTE: The wireless card needs to be in monitor mode

Change wlan0 as appropriate for the interface to monitor on

```airodump-ng wlan0```

### Sniff for 5GHz wireless networks
NOTE: The wireless card needs to be in monitor mode

Change wlan0 as appropriate for the interface to monitor on

```airodump-ng --band a wlan0```

### Sniff for 2.4GHz and 5GHz wirless networks
NOTE: The wireless card needs to be in monitor mode

Change wlan0 as appropriate for the interface to monitor on

```airodump-ng --band abg wlan0```

### Sniff a network with a given BSSID on a given channel
Change BSSID to the required access point MAC address

Change the channel to the required channel

Change wlan0 as appropriate for the interface to monitor on 

```airodump-ng --bssid 00:00:00:00:00:00 --channel 100 wlan0```

### Sniff for 2.4GHz networks and show WPS status

Change wlan0 as appropriate for the interface to monitor on

```airodump-ng --wps wlan0``

### Sniff a network with a given BSSID on a given channel and write capture to file

Change BSSID to the required access point MAC address

Change the channel to the required channel

Change the filename (test) as required

Change wlan0 as appropriate for the interface to monitor on 

```airodump-ng --bssid 30:23:03:9A:0A:9C --channel 4 --write test wlan0```

### Deauthenticate a particular wireless MAC from the access point

NOTE: I have seen people say you have to be running airodump for this to work, but don't see why that would be the case...

Change the number of deauths to carry out (0 means infinite)

Change the BSSID (-a) of the access point

Change the MAC (-c) of the client

Change wlan0 as appropriate for the name of the interface to use

```aireplay-ng --deauth 0 -a 00:00:00:00:00:00 -c 00:00:00:00:00:00 wlan0```

### Decrypt a WEP password from a capture
NOTE: Ideally you want upwards of 5000 unique IVs, you might need more...

Change the name of the capture file as required

```aircrack-ng basic_wep-01.cap```

### Connect to (associate with) network without authenticating

This basically makes the access point talk to you instead of ignoring you

NOTE: Make sure you are using airodump-ng to monitor the correct network and channel

Change the access point (-a) MAC as appropriate

Change the client (-h) MAC as appropriate - this is the address of the adapter you are using (get from ifconfig)

Change wlan0 as appropriate for the name of the interface to use

```aireplay-ng --fakeauth 0 -a 00:00:00:00:00:00 -h 00:00:00:00:00:00 wlan0```

### Replay ARP requests
This is useful for generating data (and lots of new IVs) on a network if not much is happening. It does however need to see an ARP request that it can replay (which won't happen if nothing is connected)

NOTE: Make sure you are associated with/connected to the network

Change the access point BSSID (-b) as required

Change the client (-h) MAC as appropriate - this is the address of the adapter you are using (get from ifconfig)

Change wlan0 as appropriate for the name of the interface to use

```aireplay-ng --arpreplay -b 00:00:00:00:00:00 -h 00:00:00:00:00:00 wlan0```

### Build reaver/wash from source
This is useful if you have problems with the installed version e.g. if using the Realtek RTL8812AU chip

```
apt -y install build-essential libpcap-dev

git clone https://github.com/t6x/reaver-wps-fork-t6x

cd "reaver-wps-fork-t6x/src"

./configure

make && make install
```

### View WiFi networks with WPS information
Change wlan0 as appropriate for the name of the interface to use

```wash --interface wlan0```

### Crack WPS PIN using reaver
WARNING: Reaver can associate with the access point, however this is not as reliable as getting aireplay-ng to do it

WARNING: It is highly unlikely this attack will prove useful unless their is an access point with terrible defaults (or been badly reconfigured)

NOTE: Need to get the command ready to associate with the network and execute when reaver starts running (reaver deals with changing the card to the correct channel) - the --no-associate disables the reaver association

NOTE: -vvv enables detailed diagnostic information

NOTE: If you have the pin you can specify it in --pin, this will allow you to get the password easily!

Change the access point BSSID (--bssid) as required

Change the channel (--channel) as required

Change the interface (--interface) as appropriate for the name of the interface to use

```reaver --bssid 00:00:00:00:00:00 --channel 11 --interface wlan0 -vvv --no-associate```

NOTE: As soon as you run this command, start the airodump-ng association command

### Capture WPA handshake
The handshake can be used to check if passwords are valid

Change the access point BSSID (--bssid) as required

Change the channel (--channel) as required

Change the file to capture to (--write) as required

Change wlan0 as appropriate for the name of the interface to use

```airodump-ng --bssid 00:00:00:00:00:00 --channel 11 --write wpa_handshake wlan0```

NOTE: You just have to wait for an authentication, or you can force a deauthentication (see command elsewhere in this document)

NOTE: When airodump-ng has captured the handshake you will see the following appear in the header row (00:00:00:00:00:00 will be the bssid of the network the handshake was captured for)

```WPA handshake: 00:00:00:00:00:00```

### Brute force WPA password using handshake and wordlist (dictionary attack)

NOTE: Need to capture the handshake using the method above

NOTE: Services are available that you can give the handshake to to crack for ones that are proving tricky

NOTE: You can use GPUs to speed up the cracking

NOTE: You can pipe the wordlist in from the generator (e.g. crunch) to avoid having to store it

Change wpa_handshake-01.cap to the capture file that contains the handshake

Change the wordlist (-w) to the file containing the wordlist

```aircrack-ng wpa_handshake-01.cap -w test.txt```